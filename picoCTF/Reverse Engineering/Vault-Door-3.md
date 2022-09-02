# **_Vault-Door-3_**
## Description
> This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java

## Solution
Analyze the code, the `checkPassword` function reorders the input then check if it matchs `jU5t_a_sna_3lpm18gb41_u_4_mfr340`
```java
public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm18gb41_u_4_mfr340");
    }
```
Write a script to reverse the order
```py
# Script.py

s="jU5t_a_sna_3lpm18gb41_u_4_mfr340"
flag=['' for i in range(32)]

for i in range(8):
    flag[i]=s[i]

for i in range(8,16):
    flag[23-i]=s[i]

for i in range(16,32,2):
    flag[46-i]=s[i]

for i in range(31,16,-2):
    flag[i]=s[i]

print('picoCTF{'+''.join(flag)+'}')
```
```console
❯ python3 script.py
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_1fb380}
```
