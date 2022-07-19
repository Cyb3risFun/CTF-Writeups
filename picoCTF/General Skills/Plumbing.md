# _Plumbing_
## Description
Sometimes you need to handle process data outside of a file. Can you find a way to keep the output from this program and search for the flag? Connect to `jupiter.challenges.picoctf.org` `7480`.
## Solution
Connect to the target and redirect the output with `>`

![image](https://user-images.githubusercontent.com/70738420/178361933-fa57f3a7-3e14-4197-90e5-5aa7251aca12.png)

Read the output and `grep` for the flag

![image](https://user-images.githubusercontent.com/70738420/178362133-4bb2e1db-b788-4dae-b31d-4d7ac0b3eab7.png)