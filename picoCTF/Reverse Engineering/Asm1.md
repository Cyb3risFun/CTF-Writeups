# **_Asm1_**
## Description
> What does asm1(0x6fa) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source

## Solution
Anaylze the assembly code
```assembly
asm1:
	<+0>:	push   ebp
	<+1>:	mov    ebp,esp
	<+3>:	cmp    DWORD PTR [ebp+0x8],0x3a2		if 0x6fa > 0x3a2:
	<+10>:	jg     0x512 <asm1+37>						goto <asm1+37>
	<+12>:	cmp    DWORD PTR [ebp+0x8],0x358		
	<+19>:	jne    0x50a <asm1+29>
	<+21>:	mov    eax,DWORD PTR [ebp+0x8]
	<+24>:	add    eax,0x12
	<+27>:	jmp    0x529 <asm1+60>
	<+29>:	mov    eax,DWORD PTR [ebp+0x8]
	<+32>:	sub    eax,0x12
	<+35>:	jmp    0x529 <asm1+60>
	<+37>:	cmp    DWORD PTR [ebp+0x8],0x6fa	  if 0x6fa != 0x6fa:
	<+44>:	jne    0x523 <asm1+54>						goto <asm1+54
	<+46>:	mov    eax,DWORD PTR [ebp+0x8]		  eax=0x6fa
	<+49>:	sub    eax,0x12						  eax-=0x12
	<+52>:	jmp    0x529 <asm1+60>				  goto <asm1+60>
	<+54>:	mov    eax,DWORD PTR [ebp+0x8]
	<+57>:	add    eax,0x12						  
	<+60>:	pop    ebp							  print (0x6fa-0x12)
	<+61>:	ret    
```
```console
>>> hex(0x6fa-0x12)
'0x6e8'
```
>0x6e8