# _Search Source_
## Description
The developer of this website mistakenly left an important artifact in the website source, can you find it?
## Solution
Base on the challenge name and description, its obvious that the flag is somewhere in the source code, so I looked at the html code, but nothing there, so I went to the css file, and the flag was there
```console
❯ curl -s http://saturn.picoctf.net:50761/css/style.css | grep "picoCTF*"
/** banner_main picoCTF{1nsp3ti0n_0f_w3bpag3s_ec95fa49} **/
```