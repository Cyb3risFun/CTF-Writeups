# **_ARMssembly 1_**
## Description
> For what argument does this program print `win` with variables 68, 2 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution
Analyze the code
```arm
func:
        sub     sp, sp, #32
        str     w0, [sp, 12]    # *(sp+12)=user input
        mov     w0, 68          
        str     w0, [sp, 16]    # *(sp+16)=68
        mov     w0, 2
        str     w0, [sp, 20]    # *(sp+20)=2
        mov     w0, 3
        str     w0, [sp, 24]   # *(sp+24)=3
        ldr     w0, [sp, 20]   # w0=2
        ldr     w1, [sp, 16]   # w1=68
        lsl     w0, w1, w0     # w0=68<<2
        str     w0, [sp, 28]   # *(sp+28)=68<<2
        ldr     w1, [sp, 28]   # w1=68<<2
        ldr     w0, [sp, 24]   # w0=3
        sdiv    w0, w1, w0     # w0=(68<<2)//3
        str     w0, [sp, 28]   # *(sp+28)=(68<<2)//3
        ldr     w1, [sp, 28]   # w1=(68<<2)//3
        ldr     w0, [sp, 12]   # w0=userinput
        sub     w0, w1, w0     # w0=(68<<2)//3-userinput
        str     w0, [sp, 28]   # *(sp+28)=(68<<2)//3-userinput
        ldr     w0, [sp, 28]   # w0=(68<<2)//3-userinput
        add     sp, sp, 32
        ret
main:
        stp     x29, x30, [sp, -48]!
        add     x29, sp, 0
        str     w0, [x29, 28]
        str     x1, [x29, 16]
        ldr     x0, [x29, 16]
        add     x0, x0, 8
        ldr     x0, [x0]
        bl      atoi
        str     w0, [x29, 44]
        ldr     w0, [x29, 44]
        bl      func
        cmp     w0, 0       # compare w0 with 0
        bne     .L4         # Lose if not equal
        adrp    x0, .LC0    # Win if equal
        add     x0, x0, :lo12:.LC0
        bl      puts
        b       .L6
```
Base on the analysis, we know that we should enter `(68<<2)//3` as input
```py
>>> (68<<2)//3
90
```
Flag requires hex and 32 bits
```py
>>> format(90,'08x')
'0000005a'
```
> picoCTF{0000005a}
        