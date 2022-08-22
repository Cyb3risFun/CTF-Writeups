# **_Safe Opener_**
## Description
> Can you open this safe?
I forgot the key to my safe but this program is supposed to help me with retrieving the lost key. Can you help me unlock my safe?
Put the password you recover into the picoCTF flag format like:
picoCTF{password}

## Solution
Analyse the code, there is a base64 encoded string
> cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz

Decrypt it to get the flag
```console
❯ echo 'cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz' | base64 -d
pl3as3_l3t_m3_1nt0_th3_saf3
```
> picoCTF{pl3as3_l3t_m3_1nt0_th3_saf3}