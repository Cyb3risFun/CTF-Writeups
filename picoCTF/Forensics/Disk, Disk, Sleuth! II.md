# **_Disk, Disk, Sleuth! II_**
## Description
> All we know is the file with the flag is named `down-at-the-bottom.txt`... Disk image: `dds2-alpine.flag.img.gz`

## Solution
Run `strings` against the disk image and `grep` for the flag, but nothing returned
List the file with `fls`
```console
❯ fls dds2-alpine.flag.img
Cannot determine file system type
```
Determine the starting offset for `Linux` partition
```console
❯ mmls dds2-alpine.flag.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000262143   0000260096   Linux (0x83)
❯ fls dds2-alpine.flag.img -o 2048
d/d 26417:      home
d/d 11: lost+found
r/r 12: .dockerenv
d/d 20321:      bin
d/d 4065:       boot
d/d 6097:       dev
d/d 2033:       etc
d/d 8129:       lib
d/d 14225:      media
d/d 16257:      mnt
d/d 18289:      opt
d/d 16258:      proc
d/d 18290:      root
d/d 16259:      run
d/d 18292:      sbin
d/d 12222:      srv
d/d 16260:      sys
d/d 18369:      tmp
d/d 12223:      usr
d/d 14229:      var
V/V 32513:      $OrphanFiles
```
Lets check the `root` directory first
```
❯ fls dds2-alpine.flag.img -o 2048 18290
r/r 18291:      down-at-the-bottom.txt
```
The description told us that the flag is in `down-at-the-bottom.txt`
Lets read the file with `icat` and grab the flag
```console
❯ icat dds2-alpine.flag.img -o 2048 18291
   _     _     _     _     _     _     _     _     _     _     _     _     _
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \
 ( p ) ( i ) ( c ) ( o ) ( C ) ( T ) ( F ) ( { ) ( f ) ( 0 ) ( r ) ( 3 ) ( n )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/
   _     _     _     _     _     _     _     _     _     _     _     _     _
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \
 ( s ) ( 1 ) ( c ) ( 4 ) ( t ) ( 0 ) ( r ) ( _ ) ( n ) ( 0 ) ( v ) ( 1 ) ( c )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/
   _     _     _     _     _     _     _     _     _     _     _
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \
 ( 3 ) ( _ ) ( 0 ) ( d ) ( 9 ) ( d ) ( 9 ) ( e ) ( c ) ( b ) ( } )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/
```
Remove the surrounding symbols with `tr` then add the `_` back in the flag 
```
❯ icat dds2-alpine.flag.img -o 2048 18291 | tr -cd '[:alnum:]{}'
picoCTF{f0r3ns1c4t0rn0v1c30d9d9ecb}%
```
>picoCTF{f0r3ns1c4t0r_n0v1c3_0d9d9ecb}