# **_ARMssembly 0_**
## Description
> What integer does this program print with arguments 4134207980 and 950176538? File: chall.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution
Read the ARM Assembly code, func1 is most important
```arm
func1:
        sub     sp, sp, #16
        str     w0, [sp, 12]
        str     w1, [sp, 8]
        ldr     w1, [sp, 12]
        ldr     w0, [sp, 8]
        # compare and save the larger input to w0
        cmp     w1, w0
        bls     .L2
        ldr     w0, [sp, 12]
        b       .L3
```
`4134207980` is obviously larger than `950176538`
Description mentioned the flag should be in hex format
```py
>>> hex(4134207980)
'0xf66b01ec'
```
Remove `0x` and make the flag
> picoCTF{f66b01ec}