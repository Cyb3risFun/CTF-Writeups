# **_Roboto Sans_**
## Description
The flag is somewhere on this web application not necessarily on the website. Find it.
Check this out.
## Solution
Go to `robots.txt`
```
User-agent *
Disallow: /cgi-bin/
Think you have seen your flag or want to keep looking.

ZmxhZzEudHh0;anMvbXlmaW
anMvbXlmaWxlLnR4dA==
svssshjweuiwl;oiho.bsvdaslejg
Disallow: /wp-admin/
```
Looks like `base64` encoded filenames, lets decode it
```
❯ echo "ZmxhZzEudHh0" |base64 -d
flag1.txt%
❯ echo "anMvbXlmaWxlLnR4dA==" | base64 -d
js/myfile.txt%
❯ echo "svssshjweuiwl" |base64 -d
��,��z�base64: invalid input
```
I went to `flag1.txt` but there wasn't such file, so I went to `js/myfile.txt` then got the flag
```console
❯ curl -s http://saturn.picoctf.net:51108/js/myfile.txt
picoCTF{Who_D03sN7_L1k5_90B0T5_22ce1f22}
