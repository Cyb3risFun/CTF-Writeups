# _Don't use client side_
## Description
Can you break into this super secure portal? `https://jupiter.challenges.picoctf.org/problem/37821/`
## Solution
Go to the webpage, it asks for credentials
I took a look at the page source, found an interesting `js` code
```js
<script type="text/javascript">
  function verify() {
    checkpass = document.getElementById("pass").value;
    split = 4;
    if (checkpass.substring(0, split) == 'pico') {
      if (checkpass.substring(split*6, split*7) == 'a3c8') {
        if (checkpass.substring(split, split*2) == 'CTF{') {
         if (checkpass.substring(split*4, split*5) == 'ts_p') {
          if (checkpass.substring(split*3, split*4) == 'lien') {
            if (checkpass.substring(split*5, split*6) == 'lz_1') {
              if (checkpass.substring(split*2, split*3) == 'no_c') {
                if (checkpass.substring(split*7, split*8) == '9}') {
                  alert("Password Verified")
                  }
                }
              }
      
            }
          }
        }
      }
    }
    else {
      alert("Incorrect password");
    }
    
  }
</script>
```
Rearrange the `js` code base on the substring index to get the flag `picoCTF{no_clients_plz_1a3c89}`
```js
checkpass.substring(0, split) == 'pico'
checkpass.substring(split, split*2) == 'CTF{'
checkpass.substring(split*2, split*3) == 'no_c'
checkpass.substring(split*3, split*4) == 'lien'
checkpass.substring(split*4, split*5) == 'ts_p'
checkpass.substring(split*5, split*6) == 'lz_1'
checkpass.substring(split*6, split*7) == 'a3c8'
checkpass.substring(split*7, split*8) == '9}'
```
