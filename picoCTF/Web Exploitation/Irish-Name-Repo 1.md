# **_Irish-Name-Repo 1_**
## Description
>There is a website running at `https://jupiter.challenges.picoctf.org/problem/33850/`  or `http://jupiter.challenges.picoctf.org:33850`. Do you think you can log us in? Try to see if you can login!

## Solution
Visit the page and browse around, there is a admin login page at `https://jupiter.challenges.picoctf.org/problem/33850/login.html`
Lets try to login as admin through `SQL Injection`, put `admin';` as username to login as admin, `;` ends the sql statement therefore ignores the password check
Login and get the flag
>picoCTF{s0m3_SQL_f8adf3fb}