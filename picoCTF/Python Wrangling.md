# Python Wrangling
## Description
Python scripts are invoked kind of like programs in the Terminal... Can you run this Python script using this password to get the flag?
## Solution
Easy One! 
Read the python script, notice how the script works.

![image](https://user-images.githubusercontent.com/70738420/178275748-a8f8c0c5-8fe2-4f41-bf2b-6e28551d91cb.png)

Read the `pw.txt` file to get the password for decryption

![image](https://user-images.githubusercontent.com/70738420/178276443-42ceeb69-3ac0-44e0-9004-c6a501464cb5.png)

Run `python3 ende.py -d flag.txt.en`, then enter the password which we retrieved in the previous step to decrypt the file and get the Flag

![image](https://user-images.githubusercontent.com/70738420/178276871-7854736a-4e22-4eba-bc02-1bc4725221ca.png)