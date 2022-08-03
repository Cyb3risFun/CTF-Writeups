# Investigative Reversing 1
## Description
> We have recovered a binary and a few images: image, image2, image3. See what you can make of it. There should be a flag somewhere.

## Solution
```
❯ xxd -s 0x1e86b mystery.png
0001e86b: 4945 4e44 ae42 6082 4346 7b41 6e31 5f39  IEND.B`.CF{An1_9
0001e87b: 6134 3731 3431 7d60                      a47141}`
❯ xxd -s 0x1e86b mystery2.png
0001e86b: 4945 4e44 ae42 6082 8573                 IEND.B`..s
❯ xxd -s 0x1e86b mystery3.png
0001e86b: 4945 4e44 ae42 6082 6963 5430 7468 615f  IEND.B`.icT0tha_
```
We can gather the text that have been appended
> mystery.png : CF{An1_9a47141}
  mystery2.png : .s
  mystery3.png : icT0tha_

Reverse the binary with `ghidra`
```c
  __stream = fopen("flag.txt","r");
  __stream_00 = fopen("mystery.png","a");
  __stream_01 = fopen("mystery2.png","a");
  __stream_02 = fopen("mystery3.png","a");
  if (__stream == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (__stream_00 == (FILE *)0x0) {
    puts("mystery.png is missing, please run this on the server");
  }
  fread(local_38,0x1a,1,__stream);
  fputc((int)local_38[1],__stream_02);
  fputc((int)(char)(local_38[0] + '\x15'),__stream_01);
  fputc((int)local_38[2],__stream_02);
  local_6b = local_38[3];
  fputc((int)local_33,__stream_02);
  fputc((int)local_34,__stream_00);
  for (local_68 = 6; local_68 < 10; local_68 = local_68 + 1) {
    local_6b = local_6b + '\x01';
    fputc((int)local_38[local_68],__stream_00);
  }
  fputc((int)local_6b,__stream_01);
  for (local_64 = 10; local_64 < 0xf; local_64 = local_64 + 1) {
    fputc((int)local_38[local_64],__stream_02);
  }
  for (local_60 = 0xf; local_60 < 0x1a; local_60 = local_60 + 1) {
    fputc((int)local_38[local_60],__stream_00);
  }
 ```
 Convert it to python may be easier to read if the above output is too messy for you
 ```py
 flag="picoCTF{....}"
stream0=open('mystery.png','a')
stream1=open('mystery2.png','a')
stream2=open('mystery3.png','a')

stream1.write(flag[0]+21)
stream2.write(flag[1])
stream2.write(flag[2])
flag_at_index_3=flag[3]
stream0.write(flag[4])
stream2.write(flag(5))

for x in range(6,10):
    flag_at_index_3=flag_at_index_3+1
    stream0.write(flag[x])
stream1.write(flag_at_index_3)
for y in range(10,15):
    stream2.write(flag[y])
for z in range(15,26):
    stream0.write(flag[z])
```
Write a script or manually reverse the flag
```py
# Script.py

stream0='CF{An1_9a47141}'
stream1= '.s'
stream2='icT0tha_'

s0_index=0
s1_index=0
s2_index=0
flag=''

# index 0
flag+=chr(ord(stream1[s1_index])-21)
s1_index+=1
# index 1,2
flag+=stream2[s2_index:s2_index+2]
s2_index+=2
# index 3
flag+=chr(ord(stream1[s1_index])-4)
s1_index+=1
# index 4
flag+=stream0[s0_index]
s0_index+=1
# index 5
flag+=stream2[s2_index]
s2_index+=1
# index 6-9
flag+=stream0[s0_index:s0_index+4]
s0_index+=4
# index 10-14
flag+=stream2[s2_index:s2_index+5]
s2_index+=5
# index 15-25
flag+=stream0[s0_index:s0_index+11]
s0_index+=11

print(flag)
```
```console
❯ python3 script.py
icoCTF{An0tha_1_9a47141}
```
Add p to the flag
> picoCTF{An0tha_1_9a47141}