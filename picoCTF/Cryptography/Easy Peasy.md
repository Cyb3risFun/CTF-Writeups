#  _Easy Peasy_
## Description
A one-time pad is unbreakable, but can you manage to recover the flag? (Wrap with picoCTF{}) `nc mercury.picoctf.net 58913` `otp.py`
## Solution
Anaylse `otp.py`, notice that `KEY_LEN = 50000`, and the encryption method used is to grab a same length of text from key, then `xor` encrypt the string, the key length is 50000 and it reuses the key after the whole key is used
```py
#!/usr/bin/python3 -u
import os.path

KEY_FILE = "key"
KEY_LEN = 50000
FLAG_FILE = "flag"


def startup(key_location):
	flag = open(FLAG_FILE).read()
	kf = open(KEY_FILE, "rb").read()

	start = key_location
	stop = key_location + len(flag)

	key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), flag, key))
	print("This is the encrypted flag!\n{}\n".format("".join(result)))

	return key_location

def encrypt(key_location):
	ui = input("What data would you like to encrypt? ").rstrip()
	if len(ui) == 0 or len(ui) > KEY_LEN:
		return -1

	start = key_location
	stop = key_location + len(ui)

	kf = open(KEY_FILE, "rb").read()

	if stop >= KEY_LEN:
		stop = stop % KEY_LEN
		key = kf[start:] + kf[:stop]
	else:
		key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), ui, key))

	print("Here ya go!\n{}\n".format("".join(result)))

	return key_location


print("******************Welcome to our OTP implementation!******************")
c = startup(0)
while c >= 0:
	c = encrypt(c)
```
Connect to the target and we get the encrypted flag `51124f4d194969633e4b52026f4c07513a6f4d05516e1e50536c4954066a1c57`
```console
❯ nc mercury.picoctf.net 58913
******************Welcome to our OTP implementation!******************
This is the encrypted flag!
51124f4d194969633e4b52026f4c07513a6f4d05516e1e50536c4954066a1c57

What data would you like to encrypt?
```
Get the length of the flag so we know how long of the key was used to encrypt the flag, `64` was given
However, in `otp.py`  we saw that the output was `format` in to `hex`, therefore the flag is only `32 characters` long
```console
❯ python3
Python 3.10.4 (main, Mar 24 2022, 13:07:27) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> len("51124f4d194969633e4b52026f4c07513a6f4d05516e1e50536c4954066a1c57")
64
```
To get the key which was used to encrypt the flag, we input `49968` characters, making `stop=key_location+len(ui)` &rarr; `50000=32+49968`, which means the whole key is used and next decryption will start from the beginning of the key
In such case, we can pass another 32 character string for the second decrytion, we can then reverse the `xor` to get the encryption key 
The output was too long so I cutted it of
```console
❯ python3 -c "print('a'*49968);print('b'*32)" | nc mercury.picoctf.net 58913
******************Welcome to our OTP implementation!******************
This is the encrypted flag!
51124f4d194969633e4b52026f4c07513a6f4d05516e1e50536c4954066a1c57

What data would you like to encrypt? Here ya go!
033d1903053d1959523403203d1958550f3d1959553d1900513d1900073d190353583d190556053d190554393d1958053d1959503d1904523d1902073d1959583d1904543d1959543d1905000f......2503e3d1900553d1907023d1902033d

What data would you like to encrypt? Here ya go!
0045484c191f39323e1a52543e1a52013b3e1a04503e1a06003e1a07073e1a07
```
Decode the flag
```console
❯ python3
>>> str.encode('b'*32).hex()
'6262626262626262626262626262626262626262626262626262626262626262'
# 'b'*32 string in hex 
>>> hex_b32=0x6262626262626262626262626262626262626262626262626262626262626262
# encrypted 'b'*32 
>>> enc_b32=0x0045484c191f39323e1a52543e1a52013b3e1a04503e1a06003e1a07073e1a07
# encrypted flag
>>> enc_flag=0x51124f4d194969633e4b52026f4c07513a6f4d05516e1e50536c4954066a1c57
# decrypt key
>>> key="{:x}".format(hex_b32^enc_b32)
>>> key
'62272a2e7b7d5b505c7830365c783063595c7866325c7864625c7865655c7865'
>>> key=0x62272a2e7b7d5b505c7830365c783063595c7866325c7864625c7865655c7865
# decrypt flag
>>> flag="{:x}".format(enc_flag^key)
>>> flag
'3335656362343233623362343334373263333563633266343130313163366432'
# Convert flag from hex to bytes
>>> bytes.fromhex(flag)
b'35ecb423b3b43472c35cc2f41011c6d2'
```
 
