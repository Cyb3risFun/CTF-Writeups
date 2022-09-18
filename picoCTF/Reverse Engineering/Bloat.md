# **_Bloat_**
## Description
> Can you get the flag?
Run this Python program in the same directory as this encrypted flag.

## Solution
Anaylse the code, it verifies the input with the function below, then decodes the flag if it matches
```py
def arg133(arg432):
  if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]:
    return True
  else:
    print(a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
a[81]+a[68]+a[66]+a[83])
    sys.exit(0)
    return False
```
Identify the expected input
```console
>>> a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
...             "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
>>> a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]
'happychance'
```
Run the python program with `happychance` as input
```console
❯ python3 bloat.flag.py
Please enter correct password for flag: happychance
Welcome back... your flag, user:
picoCTF{d30bfu5c4710n_f7w_161a4f09}
```