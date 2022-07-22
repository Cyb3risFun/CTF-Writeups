# **_Lookey Here_**
## Description
> Attackers have hidden information in a very large mass of data in the past, maybe they are still doing it.
Download the data here.

## Solution
The description gave a hint that the flag is hidden in the large text file
Use `grep` to find the flag
```console
❯ cat anthem.flag.txt | grep pico
      we think that the men of picoCTF{gr3p_15_@w3s0m3_58f5c024}
```