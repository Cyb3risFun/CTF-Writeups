# Static ain't always noise
## Description
Can you look at the data in this binary: static? This BASH script might help!
## Solution
Notice that `static` is an executable file 

![image](https://user-images.githubusercontent.com/70738420/178284468-c6b3023d-0a99-4477-b97b-4aff81a241d5.png)

Run `static`, nothing interesting

![image](https://user-images.githubusercontent.com/70738420/178284706-b1f13499-8c78-4d9e-8d8b-d5cfe0a3ae82.png)

Take a look at `ltdis.sh`, its a disassembly script using `objdump`

![image](https://user-images.githubusercontent.com/70738420/178284916-ac9d0afe-3a84-48fd-9546-4934a392814e.png)

Disassemble `static` executable with `ltdis.sh`, output will be saved in `static.ltdis.strings.txt`

![image](https://user-images.githubusercontent.com/70738420/178285532-f7f7c69d-7ec0-4f56-a0e5-aaad5aa9d9c8.png)

Read the outout file and look for the flag

![image](https://user-images.githubusercontent.com/70738420/178286129-81d754a6-d2cf-453c-b02f-c755514fefd0.png)

## Alternative Solution
Another way of retrieving the flag is to get printable characters of the executable with `strings` command

![image](https://user-images.githubusercontent.com/70738420/178286444-d0adc128-3f48-4e25-84db-20ef21868e29.png)