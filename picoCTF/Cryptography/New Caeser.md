# _New Caeser_ 
## Description
We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{}) `lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil` `new_caesar.py`
## Solution
Anaylze `new_caesar.py`, notice the key is only one character from the first `16` lowercase alphabets
```python
# new_caesar.py
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

def b16_encode(plain):
	enc = ""
	for c in plain:
		binary = "{0:08b}".format(ord(c))
		enc += ALPHABET[int(binary[:4], 2)]
		enc += ALPHABET[int(binary[4:], 2)]
	return enc

def shift(c, k):
	t1 = ord(c) - LOWERCASE_OFFSET
	t2 = ord(k) - LOWERCASE_OFFSET
	return ALPHABET[(t1 + t2) % len(ALPHABET)]

flag = "redacted"
key = "redacted"
assert all([k in ALPHABET for k in key])
assert len(key) == 1

b16 = b16_encode(flag)
enc = ""
for i, c in enumerate(b16):
	enc += shift(c, key[i % len(key)])
print(enc)
```
Understand the encode functions and reverse it

```python
# decode.py
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]
enc="lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil"

# reverse b16_encode
def b16_decode(plain):
    dec=""
    # 2 characters per loop
    for c in range(0,len(plain),2):
        # reverse binary split
        bin=""
        bin+="{0:04b}".format(ALPHABET.index(plain[c]))
        bin+="{0:04b}".format(ALPHABET.index(plain[c+1]))
        # add to decoded str
        dec+=chr(int(bin,2))
    return dec
    
# reverse shift
def unshift(c,k):
    t1=ord(c)-LOWERCASE_OFFSET
    t2=ord(k)-LOWERCASE_OFFSET
    return ALPHABET[(t1-t2)% len(ALPHABET)]

# loop through all 16 possible key
for key in ALPHABET:
    dec=""
    for i,c in enumerate(enc):
        dec+=unshift(c,key[i%len(key)])

    print(key,":",b16_decode(dec))
```
Run the decode program to get all possible flags, key `f` seems like plain text and is more likely the flag 
```console
❯ python3 decode.py
a : ºÉ¤ÉÊ
         ¤¹·¸¸¹»¹
···
b : ©¸¸¹sxwu¨¦zv§yzu|§¨{yªu¨t¦|w|wv¦z{¦xz
c : §¨bgfdiehidkjhdckfkfeijgi
d : qQqVUS
          XT
WXSZ
YWSR
    ZUZUT
         XY
           VX
e : v
`
@`EDBusGCtFGBItuHFwBuAsIDIDCsGHsEG
f : et_tu?_431db62c5618cd75f1d0b83832b67b46
g : TcNcd.N#" SQ%!R$% 'RS&$U S/Q'"'"!Q%&Q#%
h : CR=RS=B@AABDB@@@
i : 2A,AB
???      ,1?00131
j : !01ûÿý .òþ/ñòýô/ óñ"ý ü.ôÿôÿþ.òó.ðò
k : /
/ ê
ïîìáíàáìãâàìëãîãîíáâïá
l : ùÙùÞÝÛ
ÑßÛÚ      ÐÜ
    ÒÝÒÝÜ
         ÐÑ
           ÞÐ
ÈèÍÌÊýûÏËüÎÏÊÁüýÀÎÿÊýÉûÁÌÁÌËûÏÀûÍÏ
n : íü×üý·×¼»¹ìê¾ºë½¾¹°ëì¿½î¹ì¸ê°»°»ºê¾¿ê¼¾
o : ÜëÆëì¦Æ«ª¨ÛÙ­©Ú¬­¨¯ÚÛ®¬Ý¨Û§Ù¯ª¯ª©Ù­®Ù«­
p : ËÚµÚÛµÊÈÊÊÈ
```