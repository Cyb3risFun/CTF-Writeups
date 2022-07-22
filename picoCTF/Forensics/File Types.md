# **_File Types_**
## Description
> This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.
You can download the file from here.

## Solution
Identify the file type with `file` command, it returns that it is a `shell archive text`
`shar` file is self-extracting archive which will recreate files when you execute the script
```console
❯ file Flag.pdf
Flag.pdf: shell archive text
```
Change the extention and execute the script 
```
❯ mv Flag.pdf flag.shar
❯ ./flag.shar
x - created lock directory _sh00046.
x - extracting flag (text)
./flag.shar: 119: uudecode: not found
restore of flag failed
flag: MD5 check failed
x - removed lock directory _sh00046.
```
I'm missing the `uudecode` which is a part of the `sharutils`
Install it and run the script again
```
❯ ./flag.shar
x - created lock directory _sh00046.
x - extracting flag (text)
x - removed lock directory _sh00046.
❯ ls
flag  flag.shar
```
A file `flag` is extracted, `file` command indicates that this is a `ar` archive
```
❯ file flag
flag: current ar archive
```
Extract it with `ar` but failed, so I used `binwalk`
```console
❯ binwalk -e flag

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
100           0x64            bzip2 compressed data, block size = 900k
```
Extracted a file `64`, which is a `gzip` file
```console
❯ file 64
64: gzip compressed data
```
Add the accurate file extention and extract it with `gunzip`
```console
❯ mv 64 64.gz
❯ gunzip 64.gz
```
The extracted file is a `lzip` file
```console
❯ file 64
64: lzip compressed data, version: 1
```
Extract it with `lunzip`
```console
❯ lunzip 64
❯ ls
64.out
```
Extracted a `LZ4` file
```
❯ file 64.out
64.out: LZ4 compressed data (v1.4+)
```
Add the accurate file extention and extract it with `unlz4`
```console
❯ mv 64.out 64.lz4
❯ unlz4 64.lz4
Decoding file 64
64.lz4               : decoded 266 bytes
```
Extracted a `LZMA` file
```console
❯ file 64
64: LZMA compressed data, non-streamed, size 254
```
Add the accurate file extention and extract it with `unlzma`
```console
❯ mv 64 64.lzma
❯ unlzma 64.lzma
❯ ls
64
```
Extracted a `lzop` file
```console
❯ file 64
64: lzop compressed data - version 1.040, LZO1X-1, os: Unix
```
Add the accurate file extention and extract it with `lzop -d`
```console
❯ mv 64 64.lzo
❯ lzop -d 64.lzo
❯ ls
64
❯ file 64
64: lzip compressed data, version: 1
```
Extract it with `lunzip`
```console
❯ lunzip 64
❯ ls
64.out
❯ file 64.out
64.out: XZ compressed data, checksum CRC64
```
Add the accurate file extention and extract it with `unxz`
```console
❯ mv 64.out 64.xz
❯ unxz 64.xz
❯ ls
64
❯ file 64
64: ASCII text
```
Finally to the end, lets read the text file
```console
❯ cat 64
7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f37396230316332367d0a
```
Alphabets are only to `f` so this could be in `hex` format, convert it back to ascii and get the flag
```
❯ echo "7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f6630725f3062326375723137795f37396230316332367d0a"|xxd -r -p
picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_79b01c26}
```


