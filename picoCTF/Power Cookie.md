# **_Power Cookie_**
## Description
Can you get the flag?
Go to this website and see what you can discover.
## Solution
There is a button `Continue as guest` on the website, click on it and it replies
>We apologize, but we have no guest services at the moment.

The privilege should be controlled by the cookie `isAdmin`, change the value to `1` to continue as `admin` and get the flag
>picoCTF{gr4d3_A_c00k13_65fd1e1a}