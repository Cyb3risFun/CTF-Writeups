# _Logon_
## Description
The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at?` https://jupiter.challenges.picoctf.org/problem/13594/`
## Solution
Login as `Joe` but it requires a password

![image](https://user-images.githubusercontent.com/70738420/178882825-6c0cb0c1-1ec3-4cd5-af84-121b4536f197.png)

Try login as random user, turns out login as other users doesn't require a password, there is no flag though

![image](https://user-images.githubusercontent.com/70738420/178883349-500dc098-151c-42cf-9f4d-a85b1b290df5.png)

Take a look at the cookies, notice that there is a cookie name `admin` and it's value is `False`, change the value to `True` and get the flag

![image](https://user-images.githubusercontent.com/70738420/178883748-b9743caa-cac4-4fb2-ba67-a58115255d15.png)