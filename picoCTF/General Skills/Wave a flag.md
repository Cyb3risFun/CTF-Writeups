# Wave a Flag
## Description
Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information...
## Solution
Notice that it is an executable file

![image](https://user-images.githubusercontent.com/70738420/178277905-1a5e4bc3-6b14-44ba-81cf-ae918a7f8817.png)

Run the file

![image](https://user-images.githubusercontent.com/70738420/178278038-59cb4f79-abf9-4eb9-927b-a61655bc865a.png)

Add `-h` to see the help function, and the flag appears

![image](https://user-images.githubusercontent.com/70738420/178278252-0ac1267e-6b36-422c-b8e7-55c9c1ddc923.png)

## Alternative Solution
Another way to retrive the flag is to get the printable characters in file with `strings`, then look for the flag

![image](https://user-images.githubusercontent.com/70738420/178278920-5042f874-4968-41f7-a64e-34cd44270ef9.png)