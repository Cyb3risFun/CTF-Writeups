# **_Extensions_**
## Description
> This is a really weird text file TXT? Can you find the flag?

## Solution
Determine the correct file extention with `file`
```console
❯ file flag.txt
flag.txt: PNG image data, 1697 x 608, 8-bit/color RGB, non-interlaced
```
It should be a `png` file, change the extention
```console
❯ mv flag.txt flag.png
```
Open the file and grab the flag
> picoCTF{now_you_know_about_extensions}