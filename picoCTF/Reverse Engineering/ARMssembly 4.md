# **_ARMssembly 4_**
## Description
> What integer does this program print with argument 1215610622? File: chall_4.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution
Anaylze the assembly code, notice there are lots condition statements leading to different functions, we just have to follow the condition which meets
```assembly
func1:
	stp	x29, x30, [sp, -32]!
	add	x29, sp, 0
	str	w0, [x29, 28]	  # *(x29+28)=w0
	ldr	w0, [x29, 28]	  # w0=*(x29+28)
	cmp	w0, 100			  # if w0<100:
	bls	.L2				  #		L2()
	ldr	w0, [x29, 28]	  # w0=*(x29+28)
	add	w0, w0, 100		  # w0=w0+100 
	bl	func2			  # func2()
	b	.L3
```
User input is `1215610622`, which will be stored at `w0`
`1215610622 > 100` so `1215610622+100` and goto `func2`
```assembly
func2:
	stp	x29, x30, [sp, -32]!
	add	x29, sp, 0
	str	w0, [x29, 28]	
	ldr	w0, [x29, 28]		# w0=*(x29+28)
	cmp	w0, 499				# if w0>499:
	bhi	.L5					#	L5()
	ldr	w0, [x29, 28]
	sub	w0, w0, #86
	bl	func4
	b	.L6
```
`1215610622+100 > 499` goto `L5`
```assembly
.L5:
	ldr	w0, [x29, 28]		# w0=*(x29+28)
	add	w0, w0, 13			# w0=w0+13
	bl	func5				# func5()
```
`1215610622+100+13` goto `func5`
```assembly
func5:
	stp	x29, x30, [sp, -32]!
	add	x29, sp, 0
	str	w0, [x29, 28]
	ldr	w0, [x29, 28]		# w0=*(x29+28)
	bl	func8				# func8()
	str	w0, [x29, 28]
	ldr	w0, [x29, 28]
	ldp	x29, x30, [sp], 32
	ret
	.size	func5, .-func5
	.align	2
	.global	func6
	.type	func6, %function
```
Goto `func8`
```assembly
func8:
	sub	sp, sp, #16
	str	w0, [sp, 12]
	ldr	w0, [sp, 12]		# w0=*(x29+28)
	add	w0, w0, 2  			# w0=w0+2
	add	sp, sp, 16
	ret
	.size	func8, .-func8
	.section	.rodata
	.align	3
```
`1215610622+100+13+2` 
```console
❯ python3 -q
>>> f'{1215610622+100+13+2:08x}'
'4874bf71'
```
> picoCTF{4874bf71}
