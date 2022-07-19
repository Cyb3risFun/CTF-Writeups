# _Login_
## Description
My dog-sitter's brother made this website but I can't get in; can you help?
`login.mars.picoctf.net`
## Solution
Go to the website and there is a login form, tried to login with random username and password, doesn't seem helpful
So I took a look at the page source, there is a `index.js` file; After beautify with an online tool, it looks like this is used for validation 
```js
(async () => {
    await new Promise((e => window.addEventListener("load", e))), document.querySelector("form").addEventListener("submit", (e => {
        e.preventDefault();
        const r = {
                u: "input[name=username]",
                p: "input[name=password]"
            },
            t = {};
        for (const e in r) t[e] = btoa(document.querySelector(r[e]).value).replace(/=/g, "");
        return "YWRtaW4" !== t.u ? alert("Incorrect Username") : "cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ" !== t.p ? alert("Incorrect Password") : void alert(`Correct Password! Your flag is ${atob(t.p)}.`)
    }))
})();
```
There is an interesting string in the `js` file
>cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ

`base64` decode it and get the string
```console
❯ echo "cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ" | base64 -d
picoCTF{53rv3r_53rv3r_53rv3r_53rv3r_53rv3r}
```