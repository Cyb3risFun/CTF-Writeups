# **_Speeds and Feeds_**
## Description
> There is something on my shop network running at nc mercury.picoctf.net 16524, but I can't tell what it is. Can you?

## Solution
Connect to the target
```py
❯ nc mercury.picoctf.net 16524
G17 G21 G40 G90 G64 P0.003 F50
G0Z0.1
G0Z0.1
G0X0.8276Y3.8621
G1Z0.1
G1X0.8276Y-1.9310
G0Z0.1
G0X1.1034Y3.8621
G1Z0.1
G1X1.1034Y-1.9310
G0Z0.1
G0X1.1034Y3.0345
G1Z0.1
.
.
.
```
A bit of research leads to G code
This [page](https://ncviewer.com/) helps me get the flag

![image](https://user-images.githubusercontent.com/70738420/184560092-ba75080b-7d2f-4846-8021-86e9fbf45304.png)

> picoCTF{num3r1cal_c0ntr0l_1395ffad}