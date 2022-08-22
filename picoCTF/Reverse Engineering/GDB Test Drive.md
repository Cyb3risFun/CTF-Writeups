# **_GDB Test Drive_**
## Description
> Can you get the flag?
Download this binary.
Here's the test drive instructions:
$ chmod +x gdbme
$ gdb gdbme
(gdb) layout asm
(gdb) break *(main+99)
(gdb) run
(gdb) jump *(main+104)

## Solution
Follow the instructions and get the flag
```console
❯ gdb gdbme -q
Reading symbols from gdbme...
(No debugging symbols found in gdbme)
(gdb) break *(main+99)
Breakpoint 1 at 0x132a
(gdb) run
Starting program: /home/armani/PicoCTF/Reverse Engineering/GDB Test Drive/gdbme

Breakpoint 1, 0x000055555555532a in main ()
(gdb) jump *(main+104)
Continuing at 0x55555555532f.
picoCTF{d3bugg3r_dr1v3_72bd8355}
[Inferior 1 (process 742) exited normally]
```
> picoCTF{d3bugg3r_dr1v3_72bd8355}