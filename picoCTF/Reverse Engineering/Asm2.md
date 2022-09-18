# **_Asm2_**
## Description
> What does asm2(0x4,0x21) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source

## Solution
Analyse the code
```assembly
asm2:
	<+0>:	push   ebp
	<+1>:	mov    ebp,esp
	<+3>:	sub    esp,0x10
	<+6>:	mov    eax,DWORD PTR [ebp+0xc]       
	<+9>:	mov    DWORD PTR [ebp-0x4],eax			# *(ebp-0x4)=0x21
	<+12>:	mov    eax,DWORD PTR [ebp+0x8]
	<+15>:	mov    DWORD PTR [ebp-0x8],eax			# *(ebp-0x8)=0x4
	<+18>:	jmp    0x509 <asm2+28>					# while *(ebp-0x8)<=0xfb46:
	<+20>:	add    DWORD PTR [ebp-0x4],0x1			#	*(ebp-0x4)+=0x1
	<+24>:	add    DWORD PTR [ebp-0x8],0x74			#	*(abp-0x8)+=0x74
	<+28>:	cmp    DWORD PTR [ebp-0x8],0xfb46
	<+35>:	jle    0x501 <asm2+20>
	<+37>:	mov    eax,DWORD PTR [ebp-0x4]
	<+40>:	leave  
	<+41>:	ret    
```
```py
>>> hex(ceil((0xfb46-0x4+1)/0x74)+0x21)
'0x24c'
```