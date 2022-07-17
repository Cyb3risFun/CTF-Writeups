# **_Client-side-again_**
## Description
Can you break into this super secure portal? `https://jupiter.challenges.picoctf.org/problem/56816/`
## Solution
Read the html and find a  `js` script which looks interesting, deobfuscate it
```js
var _0x5a46 = ["37115}", "_again_3", "this", "Password Verified", "Incorrect password", "getElementById", "value", "substring", "picoCTF{", "not_this"];
(function (_0x4bd822, _0x2bd6f7) {
  var _0xb4bdb3 = function (_0x1d68f6) {
    while (--_0x1d68f6) {
      _0x4bd822.push(_0x4bd822.shift());
    }
  };
  _0xb4bdb3(++_0x2bd6f7);
}(_0x5a46, 435));
var _0x4b5b = function (_0x2d8f05, _0x4b81bb) {
  _0x2d8f05 = _0x2d8f05 - 0;
  var _0x4d74cb = _0x5a46[_0x2d8f05];
  return _0x4d74cb;
};
function verify() {
  checkpass = document[_0x4b5b("0x0")]("pass")[_0x4b5b("0x1")];
  split = 4;
  if (checkpass[_0x4b5b("0x2")](0, split * 2) == _0x4b5b("0x3")) {
    if (checkpass[_0x4b5b("0x2")](7, 9) == "{n") {
      if (checkpass[_0x4b5b("0x2")](split * 2, split * 2 * 2) == _0x4b5b("0x4")) {
        if (checkpass[_0x4b5b("0x2")](3, 6) == "oCT") {
          if (checkpass[_0x4b5b("0x2")](split * 3 * 2, split * 4 * 2) == _0x4b5b("0x5")) {
            if (checkpass.substring(6, 11) == "F{not") {
              if (checkpass[_0x4b5b("0x2")](split * 2 * 2, split * 3 * 2) == _0x4b5b("0x6")) {
                if (checkpass[_0x4b5b("0x2")](12, 16) == _0x4b5b("0x7")) {
                  alert(_0x4b5b("0x8"));
                }
              }
            }
          }
        }
      }
    }
  } else {
    alert(_0x4b5b("0x9"));
  }
}
```
Without understanding the code, we find `4` interesting strings which looks like the flag 
>var _0x5a46 = ["37115}", "_again_3", "this", "Password Verified", "Incorrect password", "getElementById", "value", "substring", "picoCTF{", "not_this"];

Its actually quite obvious, reorder them to get the flag
>picoCTF{not_this_again_337115}

However, just for the purpose of `CTF`,  we should try and understand the `js` script
Lets use the `console` in browser to understand what the `verify` function is doing
```console
>_0x4b5b("0x2")
'substring'
>_0x4b5b("0x3")
'picoCTF{'
>_0x4b5b("0x4")
'not_this'
>_0x4b5b("0x5")
'37115}'
>_0x4b5b("0x6")
'_again_3'
>_0x4b5b("0x7")
'this'
>_0x4b5b("0x8")
'Password Verified'
```
Replace them
```js
function verify() {
  checkpass = document[_0x4b5b("0x0")]("pass")[_0x4b5b("0x1")];
  split = 4;
  if (checkpass['substring'](0, split * 2) == 'picoCTF{') {
    if (checkpass['substring'](7, 9) == "{n") {
      if (checkpass['substring'](split * 2, split * 2 * 2) == 'not_this') {
        if (checkpass['substring'](3, 6) == "oCT") {
          if (checkpass['substring'](split * 3 * 2, split * 4 * 2) == '37115}') {
            if (checkpass.substring(6, 11) == "F{not") {
              if (checkpass['substring'](split * 2 * 2, split * 3 * 2) == '_again_3') {
                if (checkpass['substring'](12, 16) == 'this') {
                  alert('Password Verified');
                }
              }
            }
          }
        }
      }
    }
  } else {
    alert(_0x4b5b("0x9"));
  }
}
```
Reorder the strings base on these conditions
+ split = 4
+ checkpass['substring'](0, split * 2) == 'picoCTF{'
+ checkpass['substring'](7, 9) == "{n"
+ checkpass['substring'](split * 2, split * 2 * 2) == 'not_this'
+ checkpass['substring'](3, 6) == "oCT"
+ checkpass['substring'](split * 3 * 2, split * 4 * 2) == '37115}'
+ checkpass.substring(6, 11) == "F{not"
+ checkpass['substring'](split * 2 * 2, split * 3 * 2) == '_again_3'
+ checkpass['substring'](12, 16) == 'this'


>picoCTF{not_this_again_337115}
