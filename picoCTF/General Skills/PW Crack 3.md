# _PW Crack 3_
## Description
Can you crack the password to get the flag?
Download the password checker here and you'll need the encrypted flag and the hash in the same directory too.
There are 7 potential passwords with 1 being correct. You can find these by examining the password checker script.
## Solution
Anaylze `level3.py`, the script prompts for a password, calculate the `md5` hash for it and compares it with the hash in `level3.hash.bin` 

![image](https://user-images.githubusercontent.com/70738420/178349068-a8d17f72-a5da-433a-80e3-b704412ee103.png)

Hashes are non-revertable, therefore I wrote a script to loop through the 7 possible passwords, calculate its md5 hash and look for the one which matches

![image](https://user-images.githubusercontent.com/70738420/178350755-ef82988f-89de-4ec4-9693-b5a8dccad7a1.png)

The script states the correct password is `dba8`

![image](https://user-images.githubusercontent.com/70738420/178350834-c2116909-7ec9-4438-9302-64769b71aa79.png)

Enter the password and retrieve the flag 

![image](https://user-images.githubusercontent.com/70738420/178350978-2f224e55-9b30-48ad-8729-8a3e9f2b0793.png)