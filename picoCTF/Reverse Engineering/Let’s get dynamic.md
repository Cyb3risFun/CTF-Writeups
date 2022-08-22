# **_Let's get dynamic_**
## Description
> Can you tell what this file is reading? chall.S

## Solution
Analyze the assembly code and notice that it calls `memcmp` to validate the input after the encryption
```assembly
	movl	$49, %edx
	movq	%rcx, %rsi
	movq	%rax, %rdi
	call	memcmp@PLT
```
Compile the assembly code
```console
❯ gcc -g chall.S
```
Run it in debug mode
```console
❯ gdb ./a.out -q
gef➤  start
gef➤  x/90i main
    .
    .
    .
   0x5555555552e2 <main+361>:   mov    edx,0x31
   0x5555555552e7 <main+366>:   mov    rsi,rcx
   0x5555555552ea <main+369>:   mov    rdi,rax
   0x5555555552ed <main+372>:   call   0x555555555060 <memcmp@plt>
    . 
    .
    .
```
Set a break point and check the compared values
```console
gef➤  b  *0x5555555552ea
Breakpoint 1 at 0x5555555552ea: file chall.c, line 94.
gef➤  c
Continuing.
RandomStringForTest

Breakpoint 1, main () at chall.c:94
gef➤  printf "%s\n", $rsi
picoCTF{dyn4
```
We got part of the flag, further analyzing the code we know that the flag is decoded in a loop with the length returned by `strlen`
```assembly
.L3:
	movl	-276(%rbp), %eax
	cltq
	movzbl	-144(%rbp,%rax), %edx
	movl	-276(%rbp), %eax
	cltq
	movzbl	-80(%rbp,%rax), %eax
	xorl	%eax, %edx
	movl	-276(%rbp), %eax
	xorl	%edx, %eax
	xorl	$19, %eax
	movl	%eax, %edx
	movl	-276(%rbp), %eax
	cltq
	movb	%dl, -272(%rbp,%rax)
	addl	$1, -276(%rbp)
.L2:
	movl	-276(%rbp), %eax
	movslq	%eax, %rbx
	leaq	-144(%rbp), %rax
	movq	%rax, %rdi
	call	strlen@PLT
	cmpq	%rax, %rbx
	jb	.L3
	leaq	-272(%rbp), %rcx
	leaq	-208(%rbp), %rax
	movl	$49, %edx
	movq	%rcx, %rsi
	movq	%rax, %rdi
	call	memcmp@PLT
	testl	%eax, %eax
	je	.L4
	leaq	.LC1(%rip), %rdi
	call	puts@PLT
	movl	$0, %eax
	jmp	.L6
```
Comparison is between `%rax` and `%rbx`, where `%rax` is the length and `%rbx` is the counter
Look for the address of comparison and set a breakpoint
```assembly
   0x5555555552c7 <main+334>:   mov    rdi,rax
   0x5555555552ca <main+337>:   call   0x555555555040 <strlen@plt>
   0x5555555552cf <main+342>:   cmp    rbx,rax
```

```assembly
gef➤  b *0x5555555552cf
Breakpoint 2 at 0x5555555552cf: file chall.c, line 88.
gef➤  i r $rax
rax            0xc                 0xc
```
`%rax` return to be `0xc` which is the same length of the partial flag we got
We need the complete flag, and we know that the `memcmp` is comparing `0x31` bytes, which is `49` characters
Edit the assembly code, instead of returning from `strlen`, pass `49` into `%rax`
```assembly
.L2:
	movl	-276(%rbp), %eax
	movslq	%eax, %rbx
	leaq	-144(%rbp), %rax
	movq	%rax, %rdi
	mov		$49, %rax    	// Add this line
	//call	strlen@PLT      // Comment or remove this
	cmpq	%rax, %rbx
	jb	.L3
	leaq	-272(%rbp), %rcx
	leaq	-208(%rbp), %rax
	movl	$49, %edx
	movq	%rcx, %rsi
	movq	%rax, %rdi
	call	memcmp@PLT
	testl	%eax, %eax
	je	.L4
	leaq	.LC1(%rip), %rdi
	call	puts@PLT
	movl	$0, %eax
	jmp	.L6
```
Compile the code again, enter debug mode, set the breakpoint and get the complete flag
```console
❯ gcc -g chall.S
❯ gdb ./a.out -q
gef➤  b *0x5555555552ea
Breakpoint 1 at 0x5555555552ea
gef➤  run

test

gef➤  printf "%s\n", $rsi
picoCTF{dyn4m1c_4n4ly1s_1s_5up3r_us3ful_9266fa82}
```