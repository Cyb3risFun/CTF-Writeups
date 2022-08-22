# **_ARMssembly 3_**
## Description
> What integer does this program print with argument 3634247936? File: chall_3.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution
Analyze the ARM assmebly code
```arm
func1:
	stp	x29, x30, [sp, -48]!
	add	x29, sp, 0
	str	w0, [x29, 28]	# *(x29+28)=3634247936
	str	wzr, [x29, 44]  # *(x29+44)=0
	b	.L2
.L4:
	ldr	w0, [x29, 28] 	# w0=*(x29+28) 
	and	w0, w0, 1		# w0= w0 & 1
	cmp	w0, 0			# if w0==0:
	beq	.L3				# 	L3()
	ldr	w0, [x29, 44]	# w0=*(x29+44)
	bl	func2			# func2()
	str	w0, [x29, 44]
.L3:
	ldr	w0, [x29, 28] 	# w0=*(x29+28)
	lsr	w0, w0, 1		# w0>>1
	str	w0, [x29, 28]	# *(x29+28)=w0
.L2:
	ldr	w0, [x29, 28]	# w0=*(x29+28)
	cmp	w0, 0			# if w0!=0:
	bne	.L4				# 	L4()
	ldr	w0, [x29, 44]
	ldp	x29, x30, [sp], 48
	ret
	.size	func1, .-func1
	.align	2
	.global	func2
	.type	func2, %function
func2:
	sub	sp, sp, #16		
	str	w0, [sp, 12]  # *(sp+12)=w0
	ldr	w0, [sp, 12]  # w0=*(sp+12)
	add	w0, w0, 3	  # w0+=3
	add	sp, sp, 16
	ret
	.size	func2, .-func2
	.section	.rodata
	.align	3
```
First I converted it to the code below
```py
# Script.py

input=3634247936
flag=0

while(input!=0):
    if(input&1==0):
        input=input>>1
    else:
        flag+=3
print(format(int(flag),'08x'))
```
Then I noticed that this doesn't make sense, so I made some changes, maybe I misunderstood some part of the assembly code 
```py
# Script.py

input=3634247936
flag=0

while(input!=0):
    if(input&1!=0):
        flag+=3
    input=input>>1

print(format(int(flag),'08x'))
```
```console
❯ python3 script.py
00000027
```
>picoCTF{00000027}