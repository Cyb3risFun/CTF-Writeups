# **_Surfing the Waves_**
## Description
> While you're going through the FBI's servers, you stumble across their incredible taste in music. One main.wav you found is particularly interesting, see if you can find the flag!

## Solution
Visualized the signal but didn't find anything useful
Import `scipy.io` to gather signal from `wav` file
```py
>>> from scipy.io import wavfile
>>> data=wavfile.read('main.wav')
>>> print(data)
(2736, array([2005, 2507, 2008, ..., 5508, 4506, 7506], dtype=int16))
```
Printing the array gives us a long list of numbers, remove duplicates 
```py
>>> import numpy as np
>>> np.unique(data[1])
array([1000, 1001, 1002, 1003, 1004, 1005, 1006, 1007, 1008, 1009, 1500,
       1501, 1502, 1503, 1504, 1505, 1506, 1507, 1508, 1509, 2000, 2001,
       2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2500, 2501, 2502,
       2503, 2504, 2505, 2506, 2507, 2508, 2509, 3000, 3001, 3002, 3003,
       3004, 3005, 3006, 3007, 3008, 3009, 3500, 3501, 3502, 3503, 3504,
       3505, 3506, 3507, 3508, 3509, 4000, 4001, 4002, 4003, 4004, 4005,
       4006, 4007, 4008, 4009, 4500, 4501, 4502, 4503, 4504, 4505, 4506,
       4507, 4508, 4509, 5000, 5001, 5002, 5003, 5004, 5005, 5006, 5007,
       5008, 5009, 5500, 5501, 5502, 5503, 5504, 5505, 5506, 5507, 5508,
       5509, 6000, 6001, 6002, 6003, 6004, 6005, 6006, 6007, 6008, 6009,
       6501, 6502, 6503, 6504, 6505, 6507, 6508, 6509, 7000, 7001, 7002,
       7003, 7004, 7005, 7006, 7007, 7008, 7009, 7500, 7501, 7502, 7503,
       7504, 7505, 7506, 7507, 7508, 7509, 8000, 8001, 8002, 8003, 8005,
       8006, 8007, 8008, 8009, 8500, 8501, 8502, 8503, 8504, 8505, 8506,
       8507, 8508, 8509], dtype=int16)
```
The numbers range from `1000` to `8509`, notice the pattern
There are 16 levels, 1000, 1500, 2000, 2500, etc...
16 levels reminds me of `hex`
Remove the last two digits by dividing them with `100`
```py
>>> np.unique([d//100 for d in data[1]])
array([10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85])
```
These numbers have a common factor of 5, since these numbers represents hex and starts with 10, we should minus 2 after the division
```py
>>> np.unique([d//100//5-2 for d in data[1]])
array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15])
```
Convert the signal data into hex and `unhex` it to `ascii` 
```py
>>> h=[string.hexdigits[d//100//5-2] for d in data[1]]
>>> from pwn import *
>>> unhex(''.join(h))
b'#!/usr/bin/env python3\nimport numpy as np\nfrom scipy.io.wavfile import write\nfrom binascii import hexlify\nfrom random import random\n\nwith open(\'generate_wav.py\', \'rb\') as f:\n\tcontent = f.read()\n\tf.close()\n\n# Convert this program into an array of hex values\nhex_stuff = (list(hexlify(content).decode("utf-8")))\n\n# Loop through the each character, and convert the hex a-f characters to 10-15\nfor i in range(len(hex_stuff)):\n\tif hex_stuff[i] == \'a\':\n\t\thex_stuff[i] = 10\n\telif hex_stuff[i] == \'b\':\n\t\thex_stuff[i] = 11\n\telif hex_stuff[i] == \'c\':\n\t\thex_stuff[i] = 12\n\telif hex_stuff[i] == \'d\':\n\t\thex_stuff[i] = 13\n\telif hex_stuff[i] == \'e\':\n\t\thex_stuff[i] = 14\n\telif hex_stuff[i] == \'f\':\n\t\thex_stuff[i] = 15\n\n\t# To make the program actually audible, 100 hertz is added from the beginning, then the number is multiplied by\n\t# 500 hertz\n\t# Plus a cheeky random amount of noise\n\thex_stuff[i] = 1000 + int(hex_stuff[i]) * 500 + (10 * random())\n\n\ndef sound_generation(name, rand_hex):\n\t# The hex array is converted to a 16 bit integer array\n\tscaled = np.int16(np.array(hex_stuff))\n\t# Sci Pi then writes the numpy array into a wav file\n\twrite(name, len(hex_stuff), scaled)\n\trandomness = rand_hex\n\n\n# Pump up the music!\n# print("Generating main.wav...")\n# sound_generation(\'main.wav\')\n# print("Generation complete!")\n\n# Your ears have been blessed\n# picoCTF{mU21C_1s_1337_b58b4519}'
```
The flag is in the python script we decoded
> picoCTF{mU21C_1s_1337_b58b4519}


```py
# Script.py
from scipy.io import wavfile
from pwn import *
import string

data=wavfile.read('main.wav')
h=[string.hexdigits[d//100//5-2] for d in data[1]]
print(unhex(''.join(h)).decode())
```
```py
❯ python3 script.py
#!/usr/bin/env python3
import numpy as np
from scipy.io.wavfile import write
from binascii import hexlify
from random import random

with open('generate_wav.py', 'rb') as f:
    content = f.read()
    f.close()

# Convert this program into an array of hex values
hex_stuff = (list(hexlify(content).decode("utf-8")))

# Loop through the each character, and convert the hex a-f characters to 10-15
for i in range(len(hex_stuff)):
    if hex_stuff[i] == 'a':
        hex_stuff[i] = 10
    elif hex_stuff[i] == 'b':
        hex_stuff[i] = 11
    elif hex_stuff[i] == 'c':
        hex_stuff[i] = 12
    elif hex_stuff[i] == 'd':
        hex_stuff[i] = 13
    elif hex_stuff[i] == 'e':
        hex_stuff[i] = 14
    elif hex_stuff[i] == 'f':
        hex_stuff[i] = 15

    # To make the program actually audible, 100 hertz is added from the beginning, then the number is multiplied by
    # 500 hertz
    # Plus a cheeky random amount of noise
    hex_stuff[i] = 1000 + int(hex_stuff[i]) * 500 + (10 * random())


def sound_generation(name, rand_hex):
    # The hex array is converted to a 16 bit integer array
    scaled = np.int16(np.array(hex_stuff))
    # Sci Pi then writes the numpy array into a wav file
    write(name, len(hex_stuff), scaled)
    randomness = rand_hex


# Pump up the music!
# print("Generating main.wav...")
# sound_generation('main.wav')
# print("Generation complete!")

# Your ears have been blessed
# picoCTF{mU21C_1s_1337_b58b4519}
```
