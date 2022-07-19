# **_Java Script Kiddie_**
## Description
>The image link appears broken... `https://jupiter.challenges.picoctf.org/problem/58112` or `http://jupiter.challenges.picoctf.org:58112`
## Solution
Analyze the `js` code, notice how it shifts the `png` bytes with the key 
>Consider every 16 bytes as a seperate list, each index of key will shift the same index in the array by the key digit
Ex:
Key='123456789'
//key[0]=1, index 0 shift by 1
a1[0]=a2[0] & a2[0]=a3[0] ...
//key[1]=2, index 1 shift by 2
a1[1]=a3[1] & a2[1]=a4[1] & a3[1]=a5[1] ...
//key[2]=3, index 2 shift by 3
a1[2]=a4[2] & a2[2]=a5[2] & a3[2]=a6[2] & a4[2]=a7[2]
```html
<script>
			var bytes = [];
			$.get("bytes", function(resp) {
				bytes = Array.from(resp.split(" "), x => Number(x));
			});

			function assemble_png(u_in){
				var LEN = 16;
				var key = "0000000000000000";
				var shifter;
				if(u_in.length == LEN){
					key = u_in;
				}
				var result = [];
				for(var i = 0; i < LEN; i++){
					shifter = key.charCodeAt(i) - 48;
					for(var j = 0; j < (bytes.length / LEN); j ++){
						result[(j * LEN) + i] = bytes[(((j + shifter) * LEN) % bytes.length) + i]
					}
				}
				while(result[result.length-1] == 0){
					result = result.slice(0,result.length-1);
				}
				document.getElementById("Area").src = "data:image/png;base64," + btoa(String.fromCharCode.apply(null, new Uint8Array(result)));
				return false;
			}
		</script>
```
Original bytes are grabed from `https://jupiter.challenges.picoctf.org/problem/58112/bytes`
```console
❯ wget https://jupiter.challenges.picoctf.org/problem/58112/bytes
--2022-07-17 23:42:23--  https://jupiter.challenges.picoctf.org/problem/58112/bytes
Resolving jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)... 3.131.60.8
Connecting to jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)|3.131.60.8|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2350 (2.3K)
Saving to: ‘bytes’

bytes                  100%[============================>]   2.29K  --.-KB/s    in 0s

2022-07-17 23:42:24 (26.7 MB/s) - ‘bytes’ saved [2350/2350]
```
Convert the `js` script into `python` script
```py
# script.py

# read bytes and convert into list
f=open('bytes','rb')
bytes_array=list(map(int,f.read().split()))
f.close()
# key used for shifting
key = "0000000000000000"
length = 16
# create array with same length of bytes list
result = [0 for b in bytes_array]
# shift bytes
# loop through keys
for i in range(length):
    shifter=ord(key[i])-48
    # shift bytes base on key
    for j in range(round(len(result)/length)):
        result[(j*length)+i]=bytes_array[(((j+shifter)*length)%len(result))+i]
while(result[-1]==0):
    result=result[0:-1]
# print result
for i in range(len(result)):
    if(i%length==0):
        print()
    # convert to hex 
    print("{:02x}".format(result[i]),end=' ')
```
Run the script and check the hex value
```console
❯ python3 script.py

80 fc b6 73 b1 d3 8e fc bd f8 82 5d 9a 00 44 5a 
83 ff cc aa ef a7 12 33 e9 2b 00 1a d2 48 5f 78 
e3 07 c3 7e cf fe 73 35 8d d9 00 0b 76 c0 6e 00 
00 aa f8 49 67 4e 0a ae d0 e9 9c bb b9 41 e4 00 
89 80 e4 47 9f 0a 6f 0a 1d 60 47 ee 8d 56 5b 52 
00 d6 25 72 07 00 ee 72 85 00 8c 00 26 24 90 6c 
a4 8d 3f 02 45 49 0f 41 44 00 f9 0d 00 40 6f dc 
30 00 37 ff 0d 0c 44 29 42 78 bc 00 49 1b ad 48 
bd 50 00 94 00 40 1a 7b 00 20 2c ed 00 fc 24 13 
34 00 4e e3 62 58 01 b9 01 80 b6 b1 9b 2c 84 a2 
44 00 01 ef af f8 44 5b 54 12 df df 6f 53 1a bc 
f1 0c 00 c5 39 59 74 60 df 60 a1 2d 85 7f 7d 3f 
50 81 45 3b f1 9d 00 69 39 17 1e f1 3e e5 80 5b 
27 98 7d 92 d8 5b 05 d9 10 30 9f 04 c6 17 6c b2 
c7 0e 06 af 33 9a e3 2d 38 8c dd 00 e6 e4 63 ef 
84 c6 85 48 f3 5d 03 56 5e f6 9c 99 7b 01 cc c8 
e9 8f 7f 40 a4 cb 24 18 02 a9 79 7a 9f 28 04 19 
40 00 f1 09 5e dc fe dd 7a 08 16 e3 8c dd f8 fa 
8d 42 4e 7e be 49 f8 69 05 0e 1a 13 77 df 67 a5 
45 b1 44 3d c3 ef 73 c7 7e 3d 29 f2 af 55 d3 0b 
05 fa 5d 4f c2 4e f5 df ff bd 00 80 09 96 b2 00 
70 f7 d2 15 24 00 02 fc 90 3b 65 a4 b9 5e e8 3b 
96 ff bb 01 c6 ab b6 e4 93 49 95 2f 5c 85 93 fe 
ad f2 27 fe df d6 c4 87 f8 22 92 ce 3f 7f 7f 16 
bf 5c 58 45 17 8e a7 ed f8 17 d7 94 a6 3b f3 f8 
ad d2 a9 fe d1 9d ae c0 20 e4 29 c0 f5 2f cf 78 
8b 1c e0 f9 1d 37 dd 6d e2 15 81 4b 29 71 c0 93 
2d 90 37 e4 7e fa 7f c5 b8 9b fb 13 dc 0b f1 ab 
e5 d5 4f 87 5d 31 5e 90 26 fa 79 71 3a 72 4d 6f 
9d 92 f2 af ec b9 3c 43 ad 67 e9 ea 3c f8 1b f2 
73 df cf da cb 73 2f fc f1 98 18 a5 73 7e 30 4c 
68 7e 2a e1 e2 d3 39 fc ef 15 c3 cd 6b ff db 84 
94 51 ab 35 4f 5b 1b ae eb 7c d5 47 dd f3 d4 26 
e0 7c 36 4d f8 fc 58 a3 2c bf 6d 3f bd e7 fb bd 
f2 8d f6 f9 0f 00 02 e6 07 f4 a1 1f 2a b6 db 0f 
dd a4 fc cf 35 5f 63 3c be e8 4e ff c5 10 a9 fc 
64 a4 13 9e 20 bd 7e 8c 91 9e 74 f5 44 5e 95 6f 
fc 4a 87 bd 53 4a 47 da 63 dc d0 57 18 e4 0b 6f 
f5 01 00 62 83 2e 16 5e 47 f4 16 93 15 53 9b fc 
f3 5a 18 3b 49 f7 df 7f f2 b7 fb 7c 1c f5 de c7 
f8 7a cc e6 4f db 93 0b e1 ca ef 18 84 37 59 dd 
8f 97 89 3f 96 4f d3 08 10 04 3c 3f 63 41 00 02 
```
Notice that in the `js` script, the result of the shifted bytes is a `png` file
> document.getElementById("Area").src = "data:image/png;base64," + btoa(String.fromCharCode.apply(null, new Uint8Array(result)));

Reading the info of `png` from [wiki](https://www.wikiwand.com/en/Portable_Network_Graphics)

![image](https://user-images.githubusercontent.com/70738420/179458576-e77e472e-0795-42db-a41a-91d802f1bbb8.png)

We know that the first 16 bytes would be 8 bytes of `Magic Header`: `89 50 4E 47 0D 0A 1A 0A` and 8 bytes of the `IHDR Chunk`:`00 00 00 0D 49 48 44 52`, calcutate the shifts and we get the key `4894748485167104`
Change the key value in the script and modify the script to save the output into a `png` file
```py
# script.py

# read bytes and convert into list
f=open('bytes','rb')

bytes_array=list(map(int,f.read().split()))
f.close()
# key used for shifting
key = "4894748485167104"
length = 16
# create array with same length of bytes list
result = [0 for b in bytes_array]
# shift bytes
# loop through keys
for i in range(length):
    shifter=ord(key[i])-48
    # shift bytes base on key
    for j in range(round(len(result)/length)):
        result[(j*length)+i]=bytes_array[(((j+shifter)*length)%len(result))+i]
while(result[-1]==0):
    result=result[0:-1]
# save result
f=open('flag.png','wb')
s=""
for i in range(len(result)):
    # convert to hex
    s+=("{:02x}".format(result[i]))
# Write bytes to png
b=bytearray.fromhex(s)
f.write(b)
f.close()
```
Submit the key to the webserver, we know that the output is a `qr code`, read the output with `zbarimg` and get the flag
```console
❯ zbarimg flag.png
Connection Error (Failed to connect to socket /run/dbus/system_bus_socket: No such file or directory)
Connection Null
QR-Code:picoCTF{7b15cfb95f05286e37a22dda25935e1e}
scanned 1 barcode symbols from 1 images in 0 seconds
```
