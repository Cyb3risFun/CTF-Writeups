# **_Transformation_**
## Description
> I wonder what this really is... enc ''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])

## Solution
Look at the contents in file
```console
❯ cat enc
灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彥ㄴㅡて㝽
```
The decription included the encryption method `''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])`
What this code does is grab 2 characters in flag and merge the binary of its representive Ascii code, which makes the new data 16 bits, take a look at the below example might give you a better understanding
```py
# First character
>>> format(ord('p'),'08b')
'01110000'
# Shift left by 8, making it 16 bits
>>> format(ord('p')<<8,'016b')
'0111000000000000'
# Second Character
>>> format(ord('i'),'08b')
'01101001'
# Merge 
>>> format((ord('p')<<8)+ord('i'),'016b')
'0111000001101001'
```
Write a script to reverse it
```py
# Script.py
from pwn import *
data=read('enc').decode()
for i in data:
    # most significant 8 bits, first character 
    print(chr(ord(i)>>8),end='')
    # least significant 8 bits, second character
    print(chr(ord(i)-((ord(i)>>8)<<8)),end='')
```
Below is a step by step example on how the script reversed the flag
```py
# Encrpted data
>>> format((ord('p')<<8)+ord('i'),'016b')
'0111000001101001'
# Shift right by 8 to get the first character
>>> format(int('0111000001101001',2)>>8,'016b')
'0000000001110000'
# First character
>>> chr(int('0000000001110000',2))
'p'
# Shift the first character left by 8
>>> format(int('0000000001110000',2)<<8,'016b')
'0111000000000000'
# Deduct it from the encrypted data to get the 2 character
>>> format(int('0111000001101001',2)-int('0111000000000000',2),'016b')
'0000000001101001'
# Second Character
>>> chr(int('0000000001101001',2))
'i'
```
> picoCTF{16_bits_inst34d_of_8_e141a0f7}
