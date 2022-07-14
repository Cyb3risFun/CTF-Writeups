# _Local Authority＿
## Description
Can you get the flag?
Go to this website and see what you can discover.
## Solution
Go to the page and there is a login form, looked at the `html` file but didn't find anything helpful, so I tried login as random user, but the website replies
>Log In Failed

Monitored the website with chrome and noticed a file `secure.js`, the filename shows that it's used for validation
Take a look at the `js` file, it seems really straightforward, with the username and password in the js file
```js
function checkPassword(username, password)
{
  if( username === 'admin' && password === 'strongPassword098765' )
  {
    return true;
  }
  else
  {
    return false;
  }
}
```
Login with the given username and password, the website responds with the flag
>picoCTF{j5_15_7r4n5p4r3n7_05df90c8}

