# Vault-Door-4
## Description
> This vault uses ASCII encoding for the password. The source code for this vault is here: VaultDoor4.java

## Solution
Anaylze the `checkPassword` function
```java
public boolean checkPassword(String password) {
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
            0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
            0142, 0131, 0164, 063 , 0163, 0137, 0143, 061 ,
            '9' , '4' , 'f' , '7' , '4' , '5' , '8' , 'e' ,
        };
        for (int i=0; i<32; i++) {
            if (passBytes[i] != myBytes[i]) {
                return false;
            }
        }
        return true;
    }
```
Its obvious that the first two lines are `ascii` codes, and the last line is just characters, thrid line hold me up for a while, but then I noticed it might be `octal` since we have `decimal` and `hex` in the first two lines
```py
# Script.py

arr=[106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
     0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
     0o142, 0o131, 0o164, 0o63 , 0o163, 0o137, 0o143, 0o61 ,
    '9' , '4' , 'f' , '7' , '4' , '5' , '8' , 'e']

print('picoCTF{',end='')
for c in arr:
    if not isinstance(c,str):
        c=chr(c)
    print(c,end='')
print('}')
```
> picoCTF{jU5t_4_bUnCh_0f_bYt3s_c194f7458e}