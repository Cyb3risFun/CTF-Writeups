# **_C0rrupt_**
## Description
> We found this file. Recover the flag.

## Solution 
`file` couldn't identify the file, this file header is corrupted
```console
❯ file mystery
mystery: data
```
Check its header and identify the file type
```console
❯ xxd mystery| head
00000000: 8965 4e34 0d0a b0aa 0000 000d 4322 4452  .eN4........C"DR
00000010: 0000 066a 0000 0447 0802 0000 007c 8bab  ...j...G.....|..
00000020: 7800 0000 0173 5247 4200 aece 1ce9 0000  x....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0009 7048 5973 aa00 1625 0000 1625 0149  ..pHYs...%...%.I
00000050: 5224 f0aa aaff a5ab 4445 5478 5eec bd3f  R$......DETx^..?
00000060: 8e64 cd71 bd2d 8b20 2080 9041 8302 08d0  .d.q.-.  ..A....
00000070: f9ed 40a0 f36e 407b 9023 8f1e d720 8b3e  ..@..n@{.#... .>
00000080: b7c1 0d70 0374 b503 ae41 6bf8 bea8 fbdc  ...p.t...Ak.....
00000090: 3e7d 2a22 336f de5b 55dd 3d3d f920 9188  >}*"3o.[U.==. ..
```
From the `89 65 4e 34 0d 0a b0 aa` header, which looks similar to `png`'s magic header `89 50 4E 47 0D 0A 1A 0A`, and the  `sRGB` chunk I think this should be a `png` file
Edit the magic header and `IHDR` Chunk value, change the file extention to `.png`
```console
❯ xxd mystery| head
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 066a 0000 0447 0802 0000 007c 8bab  ...j...G.....|..
00000020: 7800 0000 0173 5247 4200 aece 1ce9 0000  x....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0009 7048 5973 aa00 1625 0000 1625 0149  ..pHYs...%...%.I
00000050: 5224 f0aa aaff a5ab 4445 5478 5eec bd3f  R$......DETx^..?
00000060: 8e64 cd71 bd2d 8b20 2080 9041 8302 08d0  .d.q.-.  ..A....
00000070: f9ed 40a0 f36e 407b 9023 8f1e d720 8b3e  ..@..n@{.#... .>
00000080: b7c1 0d70 0374 b503 ae41 6bf8 bea8 fbdc  ...p.t...Ak.....
00000090: 3e7d 2a22 336f de5b 55dd 3d3d f920 9188  >}*"3o.[U.==. ..
❯ mv mystery mystery.png
```
File is still corrupted, `pngcheck` to validate the headers and chunk
```console
❯ pngcheck mystery.png
mystery.png  CRC error in chunk pHYs (computed 38d82c82, expected 495224f0)
ERROR: mystery.png
```
From [wiki](https://www.wikiwand.com/en/Portable_Network_Graphics)
>pHYs holds the intended pixel size (or pixel aspect ratio); the pHYs contains "Pixels per unit, X axis" (4 bytes), "Pixels per unit, Y axis" (4 bytes), and "Unit specifier" (1 byte) for a total of 9 bytes.

Take a look at the `pHYs` chunk
```console
❯ xxd -g 1 -s 0x3e -l $((4+4+9+4)) mystery.png
0000003e: 00 00 00 09 70 48 59 73 aa 00 16 25 00 00 16 25  ....pHYs...%...%
0000004e: 01 49 52 24 f0                                   .IR$.
```
According to the `pHYs` chunk definition, we can get the below information
> Chunk length : 00 00 00 09
Chunk Type : 70 48 59 73
Pixels per unit, X axis : aa 00 16 25
Pixels per unit, Y axis : 00 00 16 25
Unit specifier : 01
CRC : 49 52 24 f0

Notice that Pixels per unit for X and Y axis has a big difference, `aa 00 16 25` for the X axis seems too large, lets make it the same value as Y axis
```console
❯ xxd -g 1 -s 0x3e -l $((4+4+9+4)) mystery.png
0000003e: 00 00 00 09 70 48 59 73 00 00 16 25 00 00 16 25  ....pHYs...%...%
0000004e: 01 49 52 24 f0                                   .IR$.
❯ pngcheck -v mystery.png
File: mystery.png (202940 bytes)
  chunk IHDR at offset 0x0000c, length 13
    1642 x 1095 image, 24-bit RGB, non-interlaced
  chunk sRGB at offset 0x00025, length 1
    rendering intent = perceptual
  chunk gAMA at offset 0x00032, length 4: 0.45455
  chunk pHYs at offset 0x00042, length 9: 5669x5669 pixels/meter (144 dpi)
:  invalid chunk length (too large)
ERRORS DETECTED in mystery.png
```
`Invalid chunk length (too large)`, lets see what chunk follows after `pHYs` chunk
```console
❯ xxd -g 1 -s 0x53 -l 30 mystery.png
00000053: aa aa ff a5 ab 44 45 54 78 5e ec bd 3f 8e 64 cd  .....DETx^..?.d.
00000063: 71 bd 2d 8b 20 20 80 90 41 83 02 08 d0 f9        q.-.  ..A.....
```
Its a `DET` chunk? But there is no such chunk type, maybe its `IDAT`
Again from [Wiki](https://www.wikiwand.com/en/Portable_Network_Graphics)
>IDAT contains the image, which may be split among multiple IDAT chunks. Such splitting increases filesize slightly, but makes it possible to generate a PNG in a streaming manner. The IDAT chunk contains the actual image data, which is the output stream of the compression algorithm.

And from the [PNG Specification](https://www.w3.org/TR/PNG-Chunks.html)
>There can be multiple IDAT chunks; if so, they must appear consecutively with no other intervening chunks.

Since its consecutive, we can calculate the chunk size by finding the next `IDAT` chunk, or `IEND` chunk if there is only one `IDAT` chunk
```console
❯ binwalk -R IDAT mystery.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
65544         0x10008         Raw signature (IDAT)
131080        0x20008         Raw signature (IDAT)
196616        0x30008         Raw signature (IDAT)
```
Next `IDAT` Chunk begins at `0x10008 -4`. which is `0x10004`; `-4` for the chunk size bytes which precedes the `IDAT` chunk itself 
This means that our current `IDAT` chunk content starts at `0x5B` and ends at `0x10004 -4`, which is `0x10000`; `-4`  for the 4 bytes CRC
So the actual chunk size should be `0x10000-0x5B`, `0xFFA5`
```console
❯ xxd -g 1 -s 0x53 -l 8 mystery.png
00000053: 00 00 ff a5 49 44 41 54                          ....IDAT
❯ pngcheck mystery.png
OK: mystery.png (1642x1095, 24-bit RGB, non-interlaced, 96.3%).
```
![image](https://user-images.githubusercontent.com/70738420/182043657-a01cfde2-4389-4907-94e2-323e59e8e564.png)

>picoCTF{c0rrupt10n_1847995}

Thanks to [Dvd848](https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/c0rrupt.md)
