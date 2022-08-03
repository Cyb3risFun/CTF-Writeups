# **_Investigative Reversing 0_**
## Description
> We have recovered a binary and an image. See what you can make of it. There should be a flag somewhere.

## Solution
Run `exiftool` against `mystery.png`, it says `Trailer data after PNG IEND chunk`
```console
❯ exiftool mystery.png
Warning                         : [minor] Trailer data after PNG IEND chunk
```
Lets check the data
```console
❯ binwalk -R IEND mystery.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
125035        0x1E86B         Raw signature (IEND)

❯ xxd -s 0x1e86b mystery.png
0001e86b: 4945 4e44 ae42 6082 7069 636f 4354 4b80  IEND.B`.picoCTK.
0001e87b: 6b35 7a73 6964 3671 5f66 6235 3163 3832  k5zsid6q_fb51c82
0001e88b: 317d                                     1}
```
`picoCTK.k5zsid6q_fb51c821}` seems like a corrupted flag
Reverse the `mystery` binary with `Ghidra`
We can find this in the main function
```c
  for (local_54 = 6; local_54 < 0xf; local_54 = local_54 + 1) {
    fputc((int)(char)(local_38[local_54] + '\x05'),__stream_00);
  }
  fputc((int)(char)(local_29 + -3),__stream_00);
```
It shifts index 6-15(not inclusive) by 5, and index 15 by -3
Reverse the flag manually or with a simple script
```py
# Script.py

c_flag='picoCTK.k5zsid6q_fb51c821}'

flag=c_flag[:6]
for i in range(6,15):
    flag+=chr(ord(c_flag[i])-5)
flag+=chr(ord(c_flag[15])+3)
flag+=c_flag[16:]
print(flag)
```
```console
❯ python3 script.py
picoCTF)f0und_1t_fb51c821}
```
Replace `)` with `{`
> picoCTF{f0und_1t_fb51c821}