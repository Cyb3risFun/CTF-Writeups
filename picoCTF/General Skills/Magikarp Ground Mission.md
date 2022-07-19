# Magikarp Ground Mission
## Description
Do you know how to move between directories and read files in the shell? Start the container, `ssh` to it, and then `ls` once connected to begin. Login via `ssh` as `ctf-player` with the password, `481e7b14`
## Solution
After starting the instance, connect to the target through the command provided, and enter the password `481e7b14`

![image](https://user-images.githubusercontent.com/70738420/178290772-07981fdc-3b4a-4099-9a85-1ff862f1cf60.png)

`ls` to list files, it appears that the flag is divided into 3 parts

![image](https://user-images.githubusercontent.com/70738420/178291121-e37f9953-6e5d-4fd1-98c7-653195125a0a.png)

Read `1of3.flag.txt`, we get `picoCTF{xxsh_`

![image](https://user-images.githubusercontent.com/70738420/178291447-62228d58-821d-4553-8dcd-d29181ab12ca.png)

Read `instructions-to-2of3.txt`, it asks us to go to the root `/` directory

![image](https://user-images.githubusercontent.com/70738420/178291549-8e190925-d343-445f-ac30-6e9214d8c258.png)

Go to the root `/` directory and list files  

![image](https://user-images.githubusercontent.com/70738420/178292121-d906027a-b990-487b-b1d2-f29b1d0c8167.png)

Read `2of3.flag.txt`, we get `0ut_0f_\/\/4t3r_`

![image](https://user-images.githubusercontent.com/70738420/178292936-90c026c0-b3a4-4b1b-a528-5adccbd6765d.png)

Read `instructions-to-3of3.txt`, it ask us to go to the home `~` directory

![image](https://user-images.githubusercontent.com/70738420/178293388-d4908216-3887-4578-9afd-d684eded0a72.png)

Go to `~` and get the last part of the flag `1118a9a4}`

![image](https://user-images.githubusercontent.com/70738420/178293710-b59a0825-66f1-4679-bd14-8f444c0581c6.png)

