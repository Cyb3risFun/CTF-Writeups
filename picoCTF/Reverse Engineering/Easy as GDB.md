# **_Easy as GDB_**
## Description
> The flag has got to be checked somewhere... File: brute

## Solution
Analyze the executable with `Ghidra`
```c
undefined4 FUN_000109af(void)

{
  char *__s;
  size_t sVar1;
  undefined4 uVar2;
  int iVar3;
  
  __s = (char *)calloc(0x200,1);
  printf("input the flag: ");
  fgets(__s,0x200,stdin);
  sVar1 = strnlen(&DAT_00012008,0x200);
  uVar2 = FUN_0001082b(__s,sVar1);
  FUN_000107c2(uVar2,sVar1,1);
  iVar3 = FUN_000108c4(uVar2,sVar1);
  if (iVar3 == 1) {
    puts("Correct!");
  }
  else {
    puts("Incorrect.");
  }
  return 0;
}
```
```c
undefined4 FUN_000108c4(char *param_1,uint param_2)

{
  char *__dest;
  char *__dest_00;
  uint local_18;
  
  __dest = (char *)calloc(param_2 + 1,1);
  strncpy(__dest,param_1,param_2);
  FUN_000107c2(__dest,param_2,0xffffffff);
  __dest_00 = (char *)calloc(param_2 + 1,1);
  strncpy(__dest_00,&DAT_00012008,param_2);
  FUN_000107c2(__dest_00,param_2,0xffffffff);
  puts("checking solution...");
  local_18 = 0;
  while( true ) {
    if (param_2 <= local_18) {
      return 1;
    }
    if (__dest[local_18] != __dest_00[local_18]) break;
    local_18 = local_18 + 1;
  }
  return 0xffffffff;
}
```
It encodes the input and compares it with the encoded flag character by character
Check the assembly code and get the offset of comparison, `0x98e`
```assembly
                             LAB_00010978                                    XREF[1]:     000109a5(j)  
        00010978 8b 55 f0        MOV        EDX,dword ptr [EBP + local_14]
        0001097b 8b 45 ec        MOV        EAX,dword ptr [EBP + local_18]
        0001097e 01 d0           ADD        EAX,EDX
        00010980 0f b6 10        MOVZX      EDX,byte ptr [EAX]
        00010983 8b 4d f4        MOV        ECX,dword ptr [EBP + local_10]
        00010986 8b 45 ec        MOV        EAX,dword ptr [EBP + local_18]
        00010989 01 c8           ADD        EAX,ECX
        0001098b 0f b6 00        MOVZX      EAX,byte ptr [EAX]
        0001098e 38 c2           CMP        DL,AL
        00010990 74 09           JZ         LAB_0001099b
        00010992 c7 45 e8        MOV        dword ptr [EBP + local_1c],0xffffffff
                 ff ff ff ff
        00010999 eb 0c           JMP        LAB_000109a7

```
Run the executable in debugging mode, get the starting address
```console
❯ gdb brute -q
gef➤  start
gef➤  vmmap
[ Legend:  Code | Heap | Stack ]
Start      End        Offset     
0x56555000 0x56556000 0x000000
```
Add the offset to the starting address and set the breakpoint
```assembly
gef➤  b *0x5655598e
Breakpoint 1 at 0x5655598e
gef➤  x/5i $pc
=> 0x5655598e:  cmp    dl,al
   0x56555990:  je     0x5655599b
   0x56555992:  mov    DWORD PTR [ebp-0x18],0xffffffff
   0x56555999:  jmp    0x565559a7
   0x5655599b:  add    DWORD PTR [ebp-0x14],0x1
```
It adds `1` to `%ebp-0x14` if the character is correct; Otherwise it ends the loop, return 0xffffffff and jump to `0x565559a7`, which means if we set a break point at `0x565559a7` and get the value of `%ebp-0x14`, we know how many characters has matched the flag
Write a script to bruteforce it
```py
# Script.py

from pwn import *
import string
from time import sleep

p=process('sh',level='error')
p.sendline(b'gdb brute -nx -q')
sleep(0.3)
p.sendline(b'b *0x565559a7')
flag=b'picoCTF{'

while b'}' not in flag:
    for c in string.printable:
        p.sendline(b'run')
        p.sendline(flag+c.encode())
        # print(flag+c.encode())                # Uncomment to check the process
        p.recvuntil(b'solution...')
        p.recvuntil(b'(gdb)')
        p.sendline(b'x/x $ebp-0x14')
        p.recvuntil(b'0xffff')
        p.recvuntil(b':')
        count=p.recvline().decode().strip()
        if(int(count,16)==len(flag)+1):
            flag+=c.encode()
            break
print(flag.decode())
```
```console
❯ python3 script.py
picoCTF{I_5D3_A11DA7_358a9150}
```


