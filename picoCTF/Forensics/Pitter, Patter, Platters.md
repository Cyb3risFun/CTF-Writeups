# **_Pitter, Patter, Platters_**
## Description
> 'Suspicious' is written all over this disk image. Download suspicious.dd.sda1

## Solution 
`fls` to list the files in the disk image
```console
❯ fls suspicious.dd.sda1
d/d 11: lost+found
d/d 2009:       boot
d/d 4017:       tce
r/r 12: suspicious-file.txt
V/V 8033:       $OrphanFiles
```
Found a file `suspicious-file.txt`
```console
❯ icat suspicious.dd.sda1 12
Nothing to see here! But you may want to look here -->
```
It says nothing here, but it looks too suspicious, lets keep digging
Took me a while to figure out how to view the hex of this file 
Run `strings` with `-a` to scan the whole disk image and `-t x` to print the offset in hex format
```console
❯ strings -a -t x suspicious.dd.sda1 | grep Nothing
 200400 Nothing to see here! But you may want to look here -->
```
We know the `suspicious-file.txt` starts around `200400`
Lets check its hex with `xxd`, `-s` to indicate the start offset and `-l` for the length of bytes print
```
❯ xxd -s 0x200400 -l 150 suspicious.dd.sda1
00200400: 4e6f 7468 696e 6720 746f 2073 6565 2068  Nothing to see h
00200410: 6572 6521 2042 7574 2079 6f75 206d 6179  ere! But you may
00200420: 2077 616e 7420 746f 206c 6f6f 6b20 6865   want to look he
00200430: 7265 202d 2d3e 0a7d 0036 0066 0061 0030  re -->.}.6.f.a.0
00200440: 0039 0032 0035 0066 005f 0033 003c 005f  .9.2.5.f._.3.<._
00200450: 007c 004c 006d 005f 0031 0031 0031 0074  .|.L.m._.1.1.1.t
00200460: 0035 005f 0033 0062 007b 0046 0054 0043  .5._.3.b.{.F.T.C
00200470: 006f 0063 0069 0070 0000 0000 0000 0000  .o.c.i.p........
00200480: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00200490: 0000 0000 0000                           ......
```
There is a reversed flag hidden here
Extract it with `od`, remove `\0` with `sed`, then reverse it 
```console
❯ od -A n -j 0x200437 -N 65 -c suspicious.dd.sda1 | sed 's/\\0//g' | tr -d ' \n'| rev
picoCTF{b3_5t111_mL|_<3_f5290af6}%
```
