# _Flag Shop_
## Description
There's a flag shop selling stuff, can you buy a flag? Source. Connect with `nc jupiter.challenges.picoctf.org 4906`.
## Solution
We will be using integer overflow method in this question
### Brief overview of integer overflow
A signed integer is saved in a `32 bit` space, ranging from `-2147483648` to `2147483647` and the largest bit will be used to determine whether this integer is `postive` or `negative`, to overflow the largest bit, the number has to be larger than the max number or smaller than the minimum number. Max number for a signed integer is `2147483647`, which is `01111111111111111111111111111111` in two's compliment, 31 bits of 1 and the largest bit as 0, stating that it is positive. When we add 1, the largest bit becomes 1, which turns the number negative, therefore integer overflow.

In this scenario, to increase our account balnce, in order to be able to afford the actual flag, we have to make the total cost negative, so when our account balance deducts the cost, it actually adds it to the account balance. The code checks if the number of flags is postive, so entering a negative number won't work, luckly each flags is cost 900 each, which means the number of flags we enter will be multiplied by 900 to calculate the total cost.

After analyzing the code, we noticed that we need at least `100000` to purchase the actual flag. 


![image](https://user-images.githubusercontent.com/70738420/178367913-d0f4b1e6-3829-400f-ba19-b320408f01cb.png)

Connect to the target and purchase enough amount of flags to earn enough money for the actual flag

![image](https://user-images.githubusercontent.com/70738420/178371403-ab61d9ca-bd13-4c30-be2a-5a2b6614986e.png)

Purchase the actual flag

![image](https://user-images.githubusercontent.com/70738420/178371489-36c39c48-c446-4ce5-a443-e35a3999f0bc.png)