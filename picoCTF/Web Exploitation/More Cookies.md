# _More Cookies_
## Description
I forgot Cookies can Be modified Client-side, so now I decided to encrypt them! `http://mercury.picoctf.net:56136/`

### Pre-knowledge
Please read the articles if you havent heard about `CBC Bit Flipping Attack`
https://bernardoamc.com/cbc-bitflipping-attack/
https://crypto.stackexchange.com/questions/66085/bit-flipping-attack-on-cbc-mode/66086#66086
## Solution
Since the challenge name is more `cookies`, I took a look at the cookies of the website, `auth_name` with the value `cWhBNHNkL2ZMUk5qRGRJV3RSaEVlM0c2SEJBMTRoTjZaYmIrSnpOY1V0Rm0vMk4rcVNsQW5jOWVkZEZDaVdmTFVWM0FnT1I1NzlVN1RwdEMwV09YNVFicUc5TEVWc3cwRnFhY3M2UDFwWGVFWEUrOXY1WFdpUEE3WVlVTW4vU2M=`, this may differ due to the encryption method

Its base64 decoded, I decoded it but the value doesnt seem useful
```console
❯ echo "cWhBNHNkL2ZMUk5qRGRJV3RSaEVlM0c2SEJBMTRoTjZaYmIrSnpOY1V0Rm0vMk4rcVNsQW5jOWVkZEZDaVdmTFVWM0FnT1I1NzlVN1RwdEMwV09YNVFicUc5TEVWc3cwRnFhY3M2UDFwWGVFWEUrOXY1WFdpUEE3WVlVTW4vU2M=" | base64 -d
qhA4sd/fLRNjDdIWtRhEe3G6HBA14hN6Zbb+JzNcUtFm/2N+qSlAnc9eddFCiWfLUV3AgOR579U7TptC0WOX5QbqG9LEVsw0Fqacs6P1pXeEXE+9v5XWiPA7YYUMn/Sc
```
The description gives a hint by capitalizing `C` `B` `C`, I took a look online and found `Bit Flipping Attack`, so I wrote a script for it
```python
# More_Cookies.py
import base64
import requests
from tqdm import tqdm

# Bit flipping attack
def bitFlip(pos,bit,data):

    l=bytearray(data)
    l[pos]=l[pos]^bit
    raw=bytes(l)
    return base64.b64encode(raw).decode()


ck="cWhBNHNkL2ZMUk5qRGRJV3RSaEVlM0c2SEJBMTRoTjZaYmIrSnpOY1V0Rm0vMk4rcVNsQW5jOWVkZEZDaVdmTFVWM0FnT1I1NzlVN1RwdEMwV09YNVFicUc5TEVWc3cwRnFhY3M2UDFwWGVFWEUrOXY1WFdpUEE3WVlVTW4vU2M="
# Decode cookie
ck_dec=base64.b64decode(ck)

# Bruteforce
  # Loop through all positions
for p in tqdm(range(len(ck_dec))):
    # Loop through all ascii char
    for b in tqdm(range(128),leave=False):
        # print("(",p,",",b,")")
        auth=bitFlip(p,b,ck_dec)
        # Request with cookie
        cookies={"auth_name":auth}
        r=requests.get("http://mercury.picoctf.net:56136/",cookies=cookies)
        # Check if flag is included
        if "picoCTF{" in r.text:
            print('\n',r.text[r.text.find("picoCTF{"):r.text.find("}")+1])
            break
    else:
        continue
    break
```
Revieved the flag

```console
❯ python3 more_cookies.py
 10%|████████▎                                                                      13/128 [06:23<55:50, 29.13s/it]
 picoCTF{cO0ki3s_yum_e491c430}█▉                                                  | 42/128 [00:09<00:19,  4.43it/s]
 ```
 However the script took a while, so I decided to add multiprocessing 
 ```python
 # More_Cookies_Multi.py
import base64
import requests
import sys
from p_tqdm import p_map

# Bruteforce
def loop(p):
    for b in range(128):
        auth=bitFlip(p,b,ck_dec)
        cookies={"auth_name":auth}
        r=requests.get("http://mercury.picoctf.net:56136/",cookies=cookies)
        if "picoCTF{" in r.text:
            print("\n",r.text[r.text.find("picoCTF{"):r.text.find("}")+1])
            sys.exit()

# Bit flipping attack
def bitFlip(pos,bit,data):

    l=bytearray(data)
    l[pos]=l[pos]^bit
    raw=bytes(l)
    return base64.b64encode(raw).decode()


ck="cWhBNHNkL2ZMUk5qRGRJV3RSaEVlM0c2SEJBMTRoTjZaYmIrSnpOY1V0Rm0vMk4rcVNsQW5jOWVkZEZDaVdmTFVWM0FnT1I1NzlVN1RwdEMwV09YNVFicUc5TEVWc3cwRnFhY3M2UDFwWGVFWEUrOXY1WFdpUEE3WVlVTW4vU2M="
# Decode cookie
ck_dec=base64.b64decode(ck)

# Multiprocessing
p_map(loop,range(len(ck_dec)))
```
The script ran alot faster
```console
❯ python3 More_Cookies_Multi.py
  3%|██▌                                                                          | 4/128 [00:25<08:44,  4.23s/it] picoCTF{cO0ki3s_yum_e491c430}
```

