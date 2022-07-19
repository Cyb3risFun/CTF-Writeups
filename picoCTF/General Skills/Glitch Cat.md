# _Glitch Cat_
## Description
Our flag printing service has started glitching!
`$ nc saturn.picoctf.net 53933`
## Solution
Run `$ nc saturn.picoctf.net 53933`, seems like its having a problem converting the hexadecimal value into characters

![image](https://user-images.githubusercontent.com/70738420/178329860-a2a1b7e8-fcbf-43ee-9e0e-850296f7b634.png)

Copy the output from the previous step and run it in a `python shell` to get the flag

![image](https://user-images.githubusercontent.com/70738420/178330650-9bee5f8e-14ab-412d-9ddf-604c95c1c80a.png)
