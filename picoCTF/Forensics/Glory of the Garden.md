# **_Glory of the Garden_**
## Description
> This garden contains more than it seems.

## Solution
Opened the image, its just a garden picture
I used `exiftool` and `binwalk` to check if there is any interesting information contained, but didn't find any
So I used `xxd` to check its hex value, and found the flag
```console
❯ xxd garden.jpg | tail -5
00230550: a2bb bdac 9687 98e4 d3b2 e87f ffd9 4865  ..............He
00230560: 7265 2069 7320 6120 666c 6167 2022 7069  re is a flag "pi
00230570: 636f 4354 467b 6d6f 7265 5f74 6861 6e5f  coCTF{more_than_
00230580: 6d33 3374 735f 7468 655f 3379 3336 3537  m33ts_the_3y3657
00230590: 4261 4232 437d 220a                      BaB2C}".
```
Lets use `strings` to extract the flag
```console
❯ strings garden.jpg | grep pico
Here is a flag "picoCTF{more_than_m33ts_the_3y3657BaB2C}"
```