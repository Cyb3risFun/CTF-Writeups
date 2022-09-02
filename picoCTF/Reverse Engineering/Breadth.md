# **_Breadth_**
## Description
> Surely this is what people mean when they say "horizontal scaling," right?
TOP SECRET INFO:
Our operatives managed to exfiltrate an in-development version of this challenge, where the function with the real flag had a mistake in it. Can you help us get the flag?

## Solution
There are two versions of executable, run `strings` against both files returns a huge list of flags, but we don't know which one is the actual flag
I piped all the flags extracted with `strings` command into two files and tried to find if there is an extra flag, but it appears that both outputs the same list
So I compared both executables bytes by bytes
```console
❯ cmp breadth.v1 breadth.v2 -l
    725 315 103
    726 120 122
    727 141 345
    728  57 327
    729  61 117
    730 350 165
    731 223 237
    732 150 371
    733  31 234
    734 163 127
    735 271   6
    736 220  14
    737 377  53
    738 313 302
    739 307 337
    740 327  47
    741 270 121
    742 250 247
    743 361 335
    744 264 251
 610380 124 104
 610383 270 110
 610384  72  75
 610385 200  76
 610386  67 307
 610387 320  33
 610388 110   4
 610389  71 164
 610390 302  12
 610391 164 303
 610392  10 146
 610393 303  17
 610394  17  37
 610395  37 204
 610396 200   0
2438996  61  62
```
`610380` in hex is `0x9504c`, Open `breadth.v2` with `GHidra` and look for the address
```assembly
                             **************************************************************
                             *                          FUNCTION                          *
                             **************************************************************
                             undefined fcnkKTQpF()
             undefined         AL:1           <RETURN>
             undefined8        Stack[-0x10]:8 local_10                                XREF[2]:     00195040(W), 
                                                                                                   00195049(R)  
                             fcnkKTQpF                                       XREF[3]:     Entry Point(*), 0029aac0, 
                                                                                          002dfafc(*)  
        00195040 48 c7 44        MOV        qword ptr [RSP + local_10],0x41bc73e
                 24 f0 3e 
                 c7 1b 04
        00195049 48 8b 44        MOV        RAX,qword ptr [RSP + local_10]
                 24 f0
        0019504e 48 3d 3e        CMP        RAX,0x41bc73e
                 c7 1b 04
        00195054 74 0a           JZ         LAB_00195060
        00195056 c3              RET
        00195057 66              ??         66h    f
        00195058 0f              ??         0Fh
        00195059 1f              ??         1Fh
        0019505a 84              ??         84h
        0019505b 00              ??         00h
        0019505c 00              ??         00h
        0019505d 00              ??         00h
        0019505e 00              ??         00h
        0019505f 00              ??         00h
                             LAB_00195060                                    XREF[1]:     00195054(j)  
        00195060 48 8d 3d        LEA        RDI,[s_picoCTF{VnDB2LUf1VFJkdfDJtdYtFlM_00255e   = "picoCTF{VnDB2LUf1VFJkdfDJtdYt
                 91 0e 0c 00
        00195067 e9 c4 bf        JMP        <EXTERNAL>::puts                                 int puts(char * __s)
                 f6 ff
                             -- Flow Override: CALL_RETURN (CALL_TERMINATOR)
```
Notice that it jumps to `LAB_00195060` which prints a flag, follow and grab the flag
```assembly
                             s_picoCTF{VnDB2LUf1VFJkdfDJtdYtFlM_00255ef8     XREF[1]:     fcnkKTQpF:00195060(*)  
        00255ef8 70 69 63        ds         "picoCTF{VnDB2LUf1VFJkdfDJtdYtFlMexPxXS6X}"
                 6f 43 54 
                 46 7b 56 
```
> picoCTF{VnDB2LUf1VFJkdfDJtdYtFlMexPxXS6X}

