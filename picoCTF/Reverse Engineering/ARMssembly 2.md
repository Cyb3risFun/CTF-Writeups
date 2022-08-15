# **_ARMssembly 2_**
## Description
> What integer does this program print with argument 2403814618? File: chall_2.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution
Anaylse the code
```arm
func1:
	sub	sp, sp, #32
	str	w0, [sp, 12]    # *(sp+12)=2403814618
	str	wzr, [sp, 24]   # *(sp+24)=0
	str	wzr, [sp, 28]	# *(sp+28)=0
	b	.L2
.L3:
	ldr	w0, [sp, 24]	# w0=*(sp+24)
	add	w0, w0, 3		# w0+=3
	str	w0, [sp, 24]	# *(sp+24)=w0
	ldr	w0, [sp, 28]	# w0=*(sp+28)
	add	w0, w0, 1		# w0+=1
	str	w0, [sp, 28]	# *(sp+28)=w0
.L2:
	ldr	w1, [sp, 28]    # w1=*(sp+28)
	ldr	w0, [sp, 12]	# w0=2403814618
	cmp	w1, w0			
	bcc	.L3				# while(w1<w0) L3()
	ldr	w0, [sp, 24]	# w0=*(sp+24)
	add	sp, sp, 32
	ret
```
If we turn it into python code
```py
# Script.py

sp12=2403814618
sp24=0
sp28=0
while(sp28<sp12):
    sp24+=3
    sp28+=1
    
print(sp24)
```
Since the loop has to run `2403814618` times to break the while loop, sp+24 will become `2403814618*3`
Flag requires 32bits hex format
```py
>>> f'{(2403814618*3)&0xffffffff:08x}'
'add5e68e'
```
> picoCTF{add5e68e}