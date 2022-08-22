# **_Not Crypto_**
## Description
> there's crypto in here but the challenge is not crypto... 🤔

## Solution
Analyze the executable with `Ghidra`, browse around and find a function which contains the whole cryptogrophy process
Notice that at the end of the function, it validates the input with `memcmp`
```c
    if (local_48 == local_1e8) {
      iVar24 = memcmp(local_88,local_198,0x40);
      if (iVar24 == 0) {
        puts("Yep, that\'s it!");
      }
      else {
        iVar24 = 1;
        puts("Nope, come back later");
      }
```
Look at the assembly code, the `memcmp` call is at address `0x001013b9`, whereas the address begins at `0x00100000` in `Ghidra`, so the offset is `0x13b9`
```assembly
        001013aa 48 8b 74        MOV        RSI,qword ptr [RSP + local_1c8]
                 24 40
        001013af ba 40 00        MOV        EDX,0x40
                 00 00
        001013b4 48 8b 7c        MOV        RDI,qword ptr [RSP + local_1c0]
                 24 48
        001013b9 e8 a2 fc        CALL       <EXTERNAL>::memcmp                               
                 ff ff
```
Run the executable in debugging mode, the address begins at `0x00555555554000`
```console
❯ gdb not-crypto -q
gef➤  start
gef➤  vmmap
[ Legend:  Code | Heap | Stack ]
Start              End                Offset             
0x00555555554000 0x00555555555000 0x00000000000000
```
Add the offset to the address to get the breakpoint address
```py
❯ python3 -q
>>> hex(0x00555555554000+0x13b9)
'0x5555555553b9'
```
Set the breakpoint, we also know that its reading `0x40` characters from the input, which is `64`
```console
gef➤  b *0x5555555553b9
Breakpoint 1 at 0x5555555553b9
gef➤  run
I heard you wanted to bargain for a flag... whatcha got?
1111111111111111111111111111111111111111111111111111111111111111
Breakpoint 1, 0x00005555555553b9 in ?? ()
```
The assembly code shows that the user input is saved in `%rsi` and the flag is in `%rdi`
```console
gef➤  printf "%s\n", $rdi
picoCTF{c0mp1l3r_0pt1m1z4t10n_15_pur3_w1z4rdry_but_n0_pr0bl3m?}
```