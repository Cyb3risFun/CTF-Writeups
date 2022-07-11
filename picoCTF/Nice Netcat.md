# Nice Netcat
## Description 
There is a nice program that you can talk to by using this command in a shell: `$ nc mercury.picoctf.net 22342`, but it doesn't speak English...
## Solution
Connect to `mercury.picoctf.net` port `22342` with the command provided, we get lines of numbers

![image](https://user-images.githubusercontent.com/70738420/178280328-1167525e-e762-4e87-9050-09b0a02841e2.png)

Those numbers looks like Ascii codes, translate them with an online tool
This is the tool I used. https://www.dcode.fr/ascii-code

![image](https://user-images.githubusercontent.com/70738420/178280675-7f8e0881-6b5c-4678-bab1-3e71b2102453.png)