# **_Some Assembly Required 3_**
## Description
`http://mercury.picoctf.net:60022/index.html`
## Solution
Download the `wasm` file
```console
❯ wget http://mercury.picoctf.net:60022/qCCYI0ajpD
--2022-07-14 23:54:51--  http://mercury.picoctf.net:60022/qCCYI0ajpD
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:60022... connected.
HTTP request sent, awaiting response... 200 OK
Length: 942
Saving to: ‘qCCYI0ajpD’

qCCYI0ajpD                    100%[=================================================>]     942  --.-KB/s    in 0s

2022-07-14 23:54:51 (204 MB/s) - ‘qCCYI0ajpD’ saved [942/942]

❯ file qCCYI0ajpD
qCCYI0ajpD: WebAssembly (wasm) binary module version 0x1 (MVP)
```
Decomplie the `wasm` file and notice that in the copy function, it is `xor` encrypting the flag with a secret key
```c
data d_nAa1bd7(offset: 1024) =
  "\9dn\93\c8\b2\b9A\8b\c5\c6\dda\93\c3\c2\da?\c7\93\c1\8b1\95\93\93\8eb\c8"
  "\94\c9\d5d\c0\96\c4\d97\93\93\c2\90\00\00";
data d_b(offset: 1067) = "\f1\a7\f0\07\ed";
.
.
.
function copy(a:int, b:int) {
  var c:int = g_a;
  var d:int = 16;
  var e:int_ptr = c - d;
  e[3] = a;
  e[2] = b;
  var f:int = e[3];
  if (eqz(f)) goto B_a;
  var g:int = 4;
  var h:int = e[2];
  var i:int = 5;
  var j:int = h % i;
  var k:ubyte_ptr = g - j;
  var l:int = k[1067];
  var m:int = 24;
  var n:int = l << m;
  var o:int = n >> m;
  var p:int = e[3];
  var q:int = p ^ o;
  e[3] = q;
  label B_a:
  var r:int = e[3];
  var s:byte_ptr = e[2];
  s[1072] = r;
}
```
The encrypted flag data looks messy, so I decompiled the the `wasm` file to `c` using `wasm2c` and got a hex string for the encrypted flag
```
static const u8 data_segment_data_0[] = {
  0x9d, 0x6e, 0x93, 0xc8, 0xb2, 0xb9, 0x41, 0x8b, 0xc5, 0xc6, 0xdd, 0x61,
  0x93, 0xc3, 0xc2, 0xda, 0x3f, 0xc7, 0x93, 0xc1, 0x8b, 0x31, 0x95, 0x93,
  0x93, 0x8e, 0x62, 0xc8, 0x94, 0xc9, 0xd5, 0x64, 0xc0, 0x96, 0xc4, 0xd9,
  0x37, 0x93, 0x93, 0xc2, 0x90, 0x00, 0x00,
};
```
By anaylzing the decompiled wasm file, we will be able to reverse the encryption and reveal the flag, but to speed things up, I reversed the operation by `xor` the flag pattern `picoC` with the known encypted flag hex; I only passed in 5 characters since the secret key only has 5 characters as well

![image](https://user-images.githubusercontent.com/70738420/179170806-36a48d70-e61b-42c2-8e18-314f22a2d2bd.png)

Notice the key is a reversed version of the decompiled secret key
From the code, to sum up this part, `4-(index_of_flag%5)`, `index_of_flag%5` means that the secret key is being reused every 5 char, `4-index_of_key` means that the key is reversed 
```c
var g:int = 4;
var h:int = e[2];
var i:int = 5;
var j:int = h % i;
```
So I wrote a script to decode the flag
```py
# script.py
# The encrypted flag
enc_flag=[0x9d, 0x6e, 0x93, 0xc8, 0xb2, 0xb9, 0x41, 0x8b, 0xc5, 0xc6, 0xdd, 0x61, 0x93, 0xc3, 0xc2, 0xda, 0x3f, 0xc7, 0x93, 0xc1, 0x8b, 0x31, 0x95, 0x93, 0x93, 0x8e, 0x62, 0xc8, 0x94, 0xc9, 0xd5, 0x64, 0xc0, 0x96, 0xc4, 0xd9, 0x37, 0x93, 0x93, 0xc2, 0x90, 0x00, 0x00]
# Reversed key
dec_key=[0xed,0x07,0xf0,0xa7,0xf1]

flag=""
# Decode every character
for i,c in enumerate(enc_flag):
  flag+=chr(c^dec_key[i%5])

# Print flag
print(flag)
```
Run the script to get the flag
```console
❯ python3 script.py
picoCTF{b70fcd378740f6e4bce8388c01540c43}ð
```