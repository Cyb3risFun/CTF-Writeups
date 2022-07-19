# _PW Crack 5_
## Description
Can you crack the password to get the flag?
Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. Here's a dictionary with all possible passwords based on the password conventions we've seen so far.
## Solution
Analyze `level5.py`,  it's also similar to level 4, again edited the script to read lines from `dictionary.txt`
Had to `strip` each lines since spaces effects the `md5` hash as well

![image](https://user-images.githubusercontent.com/70738420/178356075-f557d5f4-56d2-4645-a612-ddb61f477502.png)

Run the Script and get the password `9581`

![image](https://user-images.githubusercontent.com/70738420/178356466-5cf80615-718a-43cd-8b42-afc8cecde730.png)

Enter the password and retrieve the flag

![image](https://user-images.githubusercontent.com/70738420/178356540-f48c4307-00d1-4389-9419-3db43e79cc8c.png)