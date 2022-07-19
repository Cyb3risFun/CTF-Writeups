# **_Tunn3l V1s10n_**
## Desciption
> We found this file. Recover the flag.

## Solution
Identify the file type with `file`, it just returns `data`
```console
❯ file tunn3l_v1s10n
tunn3l_v1s10n: data
```
Lets view its hex and identify it with its `magic header`
```console
❯ xxd tunn3l_v1s10n | head -10
00000000: 424d 8e26 2c00 0000 0000 bad0 0000 bad0  BM.&,...........
00000010: 0000 6e04 0000 3201 0000 0100 1800 0000  ..n...2.........
00000020: 0000 5826 2c00 2516 0000 2516 0000 0000  ..X&,.%...%.....
00000030: 0000 0000 0000 231a 1727 1e1b 2920 1d2a  ......#..'..) .*
00000040: 211e 261d 1a31 2825 352c 2933 2a27 382f  !.&..1(%5,)3*'8/
00000050: 2c2f 2623 332a 262d 2420 3b32 2e32 2925  ,/&#3*&-$ ;2.2)%
00000060: 3027 2333 2a26 382c 2836 2b27 392d 2b2f  0'#3*&8,(6+'9-+/
00000070: 2623 1d12 0e23 1711 2916 0e55 3d31 9776  &#...#..)..U=1.v
00000080: 668b 6652 996d 569e 7058 9e6f 549c 6f54  f.fR.mV.pX.oT.oT
00000090: ab7e 63ba 8c6d bd8a 69c8 9771 c193 71c1  .~c..m..i..q..q.
```
We saw the magic header `424d`, search online and we know that this should be a `bmp` file, change its file extention and try to open it
``` console
❯ mv tunn3l_v1s10n tunn3l_v1s10n.bmp
```
However it runs into an error
Maybe the file header is corrupted, lookup online for the `bmp` headers, I found [this](https://www.file-recovery.com/bmp-signature-format.htm) site
Notice those bytes which `must be` a specific value and compare them to our file
> Remember that the hex should be represented in little indian format

We know that the header should be 


> 42 4D xx xx xx xx 00 00 00 00 36 00 00 00 28 00 00 00 xx xx xx xx xx xx xx xx 01 00 

Compare to our file
> 42 4d 8e 26 2c 00 00 00 00 00 ba d0 00 00 ba d0 00 00 6e 04 00 00 32 01 00 00 01 00 

Modify the hex to meet the correct format with a `hex editor`
This is how the header should look like
```
❯ xxd tunn3l_v1s10n.bmp | head -2
00000000: 424d 8e26 2c00 0000 0000 3600 0000 2800  BM.&,.....6...(.
00000010: 0000 6e04 0000 3201 0000 0100 1800 0000  ..n...2.........
```
Open the image 

![image](https://user-images.githubusercontent.com/70738420/179710803-03e46d43-7fd0-4aa0-8720-d8b966a4dd8d.png)

It says `notaflag{sorry}`, lets check the hex values again 
Remember the 18-21 byte, `6e 04 00 00`, represents the image width and 22-25, `32 01 00 00`, represents the height, which means that we have `1134` width in pixels and `306` height
Height seems a bit low comparing to the `1134` width, lets try increasing the height
```console
❯ xxd tunn3l_v1s10n.bmp | head -2
00000000: 424d 8e26 2c00 0000 0000 3600 0000 2800  BM.&,.....6...(.
00000010: 0000 6e04 0000 3203 0000 0100 1800 0000  ..n...2.........
```
I increased the height into `32 03` and got this image

![image](https://user-images.githubusercontent.com/70738420/179713955-309fa274-a5da-4902-8615-37b1c3038f0b.png)

We can peek part of the flag now, lets increase it more
```console
❯ xxd tunn3l_v1s10n.bmp | head -2
00000000: 424d 8e26 2c00 0000 0000 3600 0000 2800  BM.&,.....6...(.
00000010: 0000 6e04 0000 4203 0000 0100 1800 0000  ..n...B.........
```
We get the full flag now

![image](https://user-images.githubusercontent.com/70738420/179714461-03b582e9-aeb4-4b35-8c59-bc9b8e8aff39.png)