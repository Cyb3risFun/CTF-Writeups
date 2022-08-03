# **_Eavesdrop_**
## Description
> Download this packet capture and find the flag.

## Solution
Protocol Hierarchy Statistics
```console
❯ tshark -r capture.flag.pcap -z io,phs -q

===================================================================
Protocol Hierarchy Statistics
Filter:

eth                                      frames:75 bytes:6294
  ip                                     frames:59 bytes:5478
    udp                                  frames:6 bytes:1326
      dhcp                               frames:2 bytes:926
      dns                                frames:4 bytes:400
    tcp                                  frames:53 bytes:4152
      data                               frames:17 bytes:1585
      http                               frames:2 bytes:343
  arp                                    frames:16 bytes:816
===================================================================
```
Anaylzing the packets we found this stream
```console
❯ tshark -r capture.flag.pcap -T fields -e data.text -o data.show_as_text:TRUE tcp.stream==0

Hey, how do you decrypt this file again?\n

You're serious?\n

Yeah, I'm serious\n

*sigh* openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123\n

Ok, great, thanks.\n

Let's use Discord next time, it's more secure.\n

C'mon, no one knows we use this program like this!\n

Whatever.\n

Hey.\n

Yeah?\n

Could you transfer the file to me again?\n

Oh great. Ok, over 9002?\n

Yeah, listening.\n

Sent it\n

Got it.\n

You're unbelievable\n
```
The decrypt command
> openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123

And file will be transfered through port `9002`
```console
❯ tshark -r capture.flag.pcap -T fields -e tcp.stream tcp.dstport==9002 | uniq
2
❯ tshark -r capture.flag.pcap -T fields -e data tcp.stream==2
53616c7465645f5f7dc2e84ffb4797d0f6cf5ecea2475a094c6276624a3565596dbcf5db520bf6f22c40d44d985592c2
```
Save the data into `file.des3`
```console
❯ echo -n -e $(echo "53616c7465645f5f7dc2e84ffb4797d0f6cf5ecea2475a094c6276624a3565596dbcf5db520bf6f22c40d44d985592c2" | fold -w2 | sed 's/^/\\x/g'| tr -d '\n') > file.des3
```
Decrypt it and read the flag
```console
❯ openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
❯ cat file.txt
picoCTF{nc_73115_411_dd54ab67}
```
