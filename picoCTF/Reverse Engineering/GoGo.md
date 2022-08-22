# **_GoGo_**
## Description
> Hmmm this is a weird file... enter_password. There is a instance of the service running at mercury.picoctf.net:20140.

## Solution
Analyze the file with `Ghidra`, browse around and notice that there is a function `main.checkPassword`, which contains this part of code
```c
  uVar2 = 0;
  iVar3 = 0;
  while( true ) {
    if (0x1f < (int)uVar2) {
      if (iVar3 == 0x20) {
        return;
      }
      return;
    }
    if ((param_2 <= uVar2) || (0x1f < uVar2)) break;
    if ((*(byte *)(param_1 + uVar2) ^ *(byte *)((int)&local_40 + uVar2)) == local_20[uVar2]) {
      iVar3 = iVar3 + 1;
    }
    uVar2 = uVar2 + 1;
  }
```
The function loops through the 32 character input, `xor` with a key then compares to the encoded result 
Revert the expected input by `xor` the encoded result with key
By checking the assembly code, we know the key is stored in `esp+0x04` and result at `esp+0x24`
```console
❯ gdb enter_password -q
Reading symbols from enter_password...
warning: Missing auto-load script at offset 0 in section .debug_gdb_scripts
of file /home/armani/PicoCTF/Reverse Engineering/GoGo/enter_password.
Use `info auto-load python-scripts [REGEXP]' to list them.
(gdb) b *0x080d4b28
Breakpoint 1 at 0x80d4b28: file /opt/hacksports/shared/staging/gogo_1_4641419246175075/problem_files/enter_password.go, line 71.
(gdb) run
Starting program: /home/armani/PicoCTF/Reverse Engineering/GoGo/enter_password
[New LWP 2141]
[New LWP 2142]
[New LWP 2143]
[New LWP 2144]
[New LWP 2145]
Enter Password: 11111111111111111111111111111111

Thread 1 "enter_password" hit Breakpoint 1, 0x080d4b28 in main.checkPassword (input=..., ~r1=172) at /opt/hacksports/shared/staging/gogo_1_4641419246175075/problem_files/enter_password.go:71
71      /opt/hacksports/shared/staging/gogo_1_4641419246175075/problem_files/enter_password.go: No such file or directory.
(gdb) x/32bx $esp+0x04
0x18443f28:     0x38    0x36    0x31    0x38    0x33    0x36      0x66    0x31
0x18443f30:     0x33    0x65    0x33    0x64    0x36    0x32      0x37    0x64
0x18443f38:     0x66    0x61    0x33    0x37    0x35    0x62      0x64    0x62
0x18443f40:     0x38    0x33    0x38    0x39    0x32    0x31      0x34    0x65
(gdb) x/32bx $esp+0x24
0x18443f48:     0x4a    0x53    0x47    0x5d    0x41    0x45    0x03    0x54
0x18443f50:     0x5d    0x02    0x5a    0x0a    0x53    0x57    0x45    0x0d
0x18443f58:     0x05    0x00    0x5d    0x55    0x54    0x10    0x01    0x0e
0x18443f60:     0x41    0x55    0x57    0x4b    0x45    0x50    0x46    0x01
```
Reverse the expected input
```py
from pwn import *

s1='3836313833366631336533643632376466613337356264623833383932313465'
s2="4a53475d414503545d025a0a5357450d05005d555410010e4155574b45504601"

print(xor(unhex(s1),unhex(s2)).decode())
```
```console
❯ python3 script.py
reverseengineericanbarelyforward
```
Run the program and enter the password, its looking for another unhashed key
```console
❯ ./enter_password
Enter Password: reverseengineericanbarelyforward
=========================================
This challenge is interrupted by psociety
What is the unhashed key?
```
Going back to `Ghidra`, notice that the main function also called a `main.ambush` function, which calculates a md5sum
When reversing the password, I noticed that the xor key looks like a md5 hash
```py
❯ python3 -q
>>> s1='3836313833366631336533643632376466613337356264623833383932313465'
>>> s2="4a53475d414503545d025a0a5357450d05005d555410010e4155574b45504601"
>>> from pwn import *
>>> unhex(s1)
b'861836f13e3d627dfa375bdb8389214e'
>>> unhex(s2)
b'JSG]AE\x03T]\x02Z\nSWE\r\x05\x00]UT\x10\x01\x0eAUWKEPF\x01'
```
Crack the hash

![image](https://user-images.githubusercontent.com/70738420/185762579-71a855d9-be73-48c8-93c6-fb3b3fe52f3f.png)

Connect to the target and enter the password and unhashed key
```console
❯ nc mercury.picoctf.net 20140
Enter Password: reverseengineericanbarelyforward
=========================================
This challenge is interrupted by psociety
What is the unhashed key?
goldfish
Flag is:  picoCTF{p1kap1ka_p1c02720c216}
```
