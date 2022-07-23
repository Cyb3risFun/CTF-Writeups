# **_What Lies Within_**
## Description
> There's something in the building. Can you retrieve the flag?

## Solution
Determined the file extension, viewed the image and checked metadata, nothing helpful
```console
❯ file buildings.png
buildings.png: PNG image data, 657 x 438, 8-bit/color RGBA, non-interlaced
❯ exiftool buildings.png
ExifTool Version Number         : 12.42
File Name                       : buildings.png
Directory                       : .
File Size                       : 625 kB
File Modification Date/Time     : 2020:10:26 11:30:20-07:00
File Access Date/Time           : 2022:07:22 22:53:42-07:00
File Inode Change Date/Time     : 2022:07:22 22:53:37-07:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 657
Image Height                    : 438
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Image Size                      : 657x438
Megapixels                      : 0.288
```
Run `binwalk` against the file and found a `zlib` archive hidden
```console
❯ binwalk buildings.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 657 x 438, 8-bit/color RGBA, non-interlaced
41            0x29            Zlib compressed data, compressed
```
Extracted the file but its empty
Run `steghide` and `zsteg` against the file
```console
❯ steghide extract -sf buildings.png
Enter passphrase:
steghide: the file format of the file "buildings.png" is not supported.
❯ zsteg buildings.png
b1,r,lsb,xy         .. text: "^5>R5YZrG"
b1,rgb,lsb,xy       .. text: "picoCTF{h1d1ng_1n_th3_b1t5}"
b1,abgr,msb,xy      .. file: PGP Secret Sub-key -
b2,b,lsb,xy         .. text: "XuH}p#8Iy="
b3,abgr,msb,xy      .. text: "t@Wp-_tH_v\r"
b4,r,lsb,xy         .. text: "fdD\"\"\"\" "
b4,r,msb,xy         .. text: "%Q#gpSv0c05"
b4,g,lsb,xy         .. text: "fDfffDD\"\""
b4,g,msb,xy         .. text: "f\"fff\"\"DD"
b4,b,lsb,xy         .. text: "\"$BDDDDf"
b4,b,msb,xy         .. text: "wwBDDDfUU53w"
b4,rgb,msb,xy       .. text: "dUcv%F#A`"
b4,bgr,msb,xy       .. text: " V\"c7Ga4"
b4,abgr,msb,xy      .. text: "gOC_$_@o"
```
`zsteg` found the flag
> picoCTF{h1d1ng_1n_th3_b1t5}