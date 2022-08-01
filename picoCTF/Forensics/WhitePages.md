# **_WhitePages_**
## Description
> I stopped using YellowPages and moved onto WhitePages... but the page they gave me is all blank!

## Solution
Read the file, it contains lots of spaces
```console
❯ cat whitepages.txt
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                %
```
Lets read the hex value
```console
❯ xxd whitepages.txt| head -10
00000000: e280 83e2 8083 e280 83e2 8083 20e2 8083  ............ ...
00000010: 20e2 8083 e280 83e2 8083 e280 83e2 8083   ...............
00000020: 20e2 8083 e280 8320 e280 83e2 8083 e280   ...... ........
00000030: 83e2 8083 20e2 8083 e280 8320 e280 8320  .... ...... ...
00000040: 2020 e280 83e2 8083 e280 83e2 8083 e280    ..............
00000050: 8320 20e2 8083 20e2 8083 e280 8320 e280  .  ... ...... ..
00000060: 8320 20e2 8083 e280 83e2 8083 2020 e280  .  .........  ..
00000070: 8320 20e2 8083 2020 2020 e280 8320 e280  .  ...    ... ..
00000080: 83e2 8083 e280 83e2 8083 2020 e280 8320  ..........  ...
00000090: e280 8320 e280 8320 e280 83e2 8083 e280  ... ... ........
```
Notice there are 2 kinds of spaces `0xe2 0x80 0x83` and `0x20`
Treat it as binary where `0xe2 0x80 0x83` is `0` and `0x20` is `1`
```py
# script.py
from pwn import *

# Read data from file
data=bytearray(read('whitepages.txt'))
# Replace '\xe2\x80\x83' with 0
data=data.replace(b'\xe2\x80\x83',b'0')
# Replace '\x20' with 1
data=data.replace(b'\x20',b'1')
# Decode bytearray to bytes
data=data.decode()
# Convert bytes into characters
print(unbits(data).decode())
```
```console
❯ python3 script.py

        picoCTF

        SEE PUBLIC RECORDS & BACKGROUND REPORT
        5000 Forbes Ave, Pittsburgh, PA 15213
        picoCTF{not_all_spaces_are_created_equal_c54f27cd05c2189f8147cc6f5deb2e56}
```


