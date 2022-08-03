# **_Investigative Reversing 2_**
## Description
> We have recovered a binary and an image See what you can make of it. There should be a flag somewhere.

## Solution
Again reverse it with `Ghidra`
```c
  local_60 = fopen("flag.txt","r");
  local_58 = fopen("original.bmp","r");
  local_50 = fopen("encoded.bmp","a");
  if (local_60 == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (local_58 == (FILE *)0x0) {
    puts("original.bmp is missing, please run this on the server");
  }
  sVar1 = fread(&local_7e,1,1,local_58);
  local_7c = (int)sVar1;
  local_68 = 2000;
  for (local_78 = 0; local_78 < local_68; local_78 = local_78 + 1) {
    fputc((int)(char)local_7e,local_50);
    sVar1 = fread(&local_7e,1,1,local_58);
    local_7c = (int)sVar1;
  }
  sVar1 = fread(local_48,0x32,1,local_60);
  local_64 = (int)sVar1;
  if (local_64 < 1) {
    puts("flag is not 50 chars");
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  for (local_74 = 0; local_74 < 0x32; local_74 = local_74 + 1) {
    for (local_70 = 0; local_70 < 8; local_70 = local_70 + 1) {
      local_7d = codedChar(local_70,local_48[local_74] - 5,local_7e);
      fputc((int)(char)local_7d,local_50);
      fread(&local_7e,1,1,local_58);
    }
  }
  
  byte codedChar(int param_1,byte param_2,byte param_3)

{
  byte local_20;
  
  local_20 = param_2;
  if (param_1 != 0) {
    local_20 = (byte)((int)(char)param_2 >> ((byte)param_1 & 0x1f));
  }
  return param_3 & 0xfe | local_20 & 1;
}

```
We know that the script inserted 2000 bytes before the flag, shifted the flag by -5, then encoded the flag at the least significant bit, we can write a script to reverse it
```py
# Script.py

from pwn import *

data=read('encoded.bmp')
data=data[2000:]
flag=''
for i in range(50):
    binary=''
    for x in range(8):
        binary+=str(data[i*8+x] & 1)
    print(binary)
    flag+=chr(int(binary,2)+5)

print(flag)

```
```
❯ python3 script.py
11010110
00100110
01111010
01010110
01111100
11110010
10000010
01101110
10010110
01110100
11001110
11110110
01011010
11010100
10010110
01110100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
11010100
01111010
00001100
01111010
01001100
11111010
11111010
01110100
00101100
00011110
Û+\x7f[÷sÓû_ÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙÙ\x7f\x11Qÿÿy1#
```
Notice that some binary begins with 1, but thats invlaid for ascii code
Ascii code ranges from 0-127, 127 in binary is `01111111`
So this should be little-endian
```py
# Script.py

from pwn import *

data=read('encoded.bmp')
data=data[2000:]
flag=''
for i in range(50):
    binary=''
    for x in range(8):
        binary=str(data[i*8+x] & 1)+binary
    flag+=chr(int(binary,2)+5)

print(flag)
```
```console
❯ python3 script.py
picoCTF{n3xt_0n30000000000000000000000000c5c7dd39}
```
