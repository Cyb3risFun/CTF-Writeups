# **_Shark on wire 2_**
## Description
> We found this packet capture. Recover the flag that was pilfered from the network.

## Solution
Statistics
```console
❯ tshark -r capture.pcap -z io,phs -q

===================================================================
Protocol Hierarchy Statistics
Filter:

eth                                      frames:1326 bytes:91078
  ipv6                                   frames:22 bytes:3067
    udp                                  frames:15 bytes:2457
      mdns                               frames:13 bytes:2267
      llmnr                              frames:2 bytes:190
    icmpv6                               frames:7 bytes:610
  ip                                     frames:743 bytes:54351
    udp                                  frames:599 bytes:45573
      mdns                               frames:13 bytes:2007
      _ws.malformed                      frames:6 bytes:360
      data                               frames:537 bytes:34356
      ssdp                               frames:40 bytes:8640
      llmnr                              frames:2 bytes:150
      ntp                                frames:1 bytes:60
        _ws.malformed                    frames:1 bytes:60
    tcp                                  frames:138 bytes:8418
    igmp                                 frames:6 bytes:360
  arp                                    frames:560 bytes:33600
  lldp                                   frames:1 bytes:60
===================================================================
```
Follow all udp streams with the script
```sh
#! /bin/bash
# script.sh
END=$(tshark -r capture.pcap -T fields -e udp.stream | sort -n | tail -1)
for ((i=0;i<=END;i++))
do
    data=$(tshark -r capture.pcap -T fields -e data.text -o data.show_as_text:TRUE udp.stream==$i| tr -d '\n') 
    echo stream $i
    echo $data
done
```
``` console 
❯ ./script.sh
stream 6
kfdsalkfsalkico{N0t_a_fLag}
stream 7
icoCTF{StaT31355e
stream 8
fjdbanlkfdnfjdbanlkfdnfjdbanlkfdnfjdbanlkfdnfjdbanlkfdnfjdbanlkfdnfjdbanlkfdnfjdbanlkfdnfjdbanlkfdn
stream 9
picoCTF Sure is fun!picoCTF Sure is fun!picoCTF Sure is fun!picoCTF Sure is fun!picoCTF Sure is fun!picoCTF Sure is fun!picoCTF Sure is fun!picoCTF Sure is fun!picoCTF Sure is fun!aaaaa
stream 10
I really want to find some picoCTF flagsI really want to find some picoCTF flagsI really want to find some picoCTF flagsI really want to find some picoCTF flagsI really want to find some picoCTF flagsI really want to find some picoCTF flagsI really want to find some picoCTF flagsI really want to find some picoCTF flagsI really want to find some picoCTF flags
.
.
.
stream 32
start
.
.
.
stream 60
end
.
.
.
```
At first I thought stream 7 was the flag, and tried digging the second half
But turns out its not
Then I noticed the start and end stream 
```console
❯ tshark -r capture.pcap data contains start or data contains end
 1104 991.587437    10.0.0.66 → 10.0.0.1     UDP 60 5000 → 22 Len=5
 1303 1171.059146    10.0.0.80 → 10.0.0.1     UDP 60 5000 → 22 Len=3
```
Both packets has a srcport of 5000 and dstport of 22
```console
❯ tshark -r capture.pcap udp.srcport==5000 and udp.dstport==22
 1104 991.587437    10.0.0.66 → 10.0.0.1     UDP 60 5000 → 22 Len=5
 1303 1171.059146    10.0.0.80 → 10.0.0.1     UDP 60 5000 → 22 Len=3
```
Lets apply the filters seperately
```console
❯ tshark -r capture.pcap udp.srcport==5000
   11  31.615731     10.0.0.6 → 10.0.0.11    UDP 60 5000 → 9999 Len=4[Malformed Packet]
   19  40.041556     10.0.0.2 → 10.0.0.1     UDP 60 5000 → 8888 Len=4[Malformed Packet]
   21  42.137115     10.0.0.2 → 10.0.0.13    UDP 60 5000 → 8888 Len=12
   23  44.228732     10.0.0.2 → 10.0.0.1     UDP 60 5000 → 8888 Len=1
   25  46.324952     10.0.0.2 → 10.0.0.12    UDP 60 5000 → 8888 Len=1
   32  48.424679     10.0.0.2 → 10.0.0.13    UDP 60 5000 → 8888 Len=1
   39  50.516983     10.0.0.2 → 10.0.0.12    UDP 60 5000 → 8888 Len=1
   42  52.616679     10.0.0.2 → 10.0.0.13    UDP 60 5000 → 8888 Len=1
   44  54.733054     10.0.0.2 → 10.0.0.12    UDP 60 5000 → 8888 Len=1
   46  56.840816     10.0.0.2 → 10.0.0.13    UDP 60 5000 → 8888 Len=1
   48  58.945018     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
   50  61.045583     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
   52  63.149043     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
   54  65.242834     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
   56  67.336637     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
   58  69.432626     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
   60  71.528826     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
   62  73.616850     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
   67  75.708789     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
   71  77.801159     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
   73  79.893171     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
   75  81.988688     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
   77  84.081927     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
   79  86.176981     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
   81  88.268727     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
   83  90.364793     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
   85  92.456675     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
   87  94.553369     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
   89  96.657016     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
   91  98.745157     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
   93 100.837284     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
   98 102.924617     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
  100 105.008809     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
  102 107.101165     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
  104 109.201409     10.0.0.2 → 10.0.0.7     UDP 60 5000 → 8990 Len=11
  107 111.303767     10.0.0.2 → 10.0.0.29    UDP 62 5000 → 8990 Len=20
  114 113.397050     10.0.0.8 → 10.0.0.29    UDP 82 5000 → 8990 Len=40
```
```console
❯ tshark -r capture.pcap udp.dstport==22
 1104 991.587437    10.0.0.66 → 10.0.0.1     UDP 60 5000 → 22 Len=5
 1106 993.672341    10.0.0.66 → 10.0.0.1     UDP 60 5112 → 22 Len=5
 1118 1006.227400    10.0.0.66 → 10.0.0.1     UDP 60 5105 → 22 Len=5
 1122 1008.323546    10.0.0.66 → 10.0.0.1     UDP 60 5099 → 22 Len=5
 1124 1010.428768    10.0.0.66 → 10.0.0.1     UDP 60 5111 → 22 Len=5
 1129 1012.535515    10.0.0.66 → 10.0.0.1     UDP 60 5067 → 22 Len=5
 1131 1014.627130    10.0.0.66 → 10.0.0.1     UDP 60 5084 → 22 Len=5
 1133 1016.719657    10.0.0.66 → 10.0.0.1     UDP 60 5070 → 22 Len=5
 1135 1018.807279    10.0.0.66 → 10.0.0.1     UDP 60 5123 → 22 Len=5
 1137 1020.899193    10.0.0.66 → 10.0.0.1     UDP 60 5112 → 22 Len=5
 1139 1022.991480    10.0.0.66 → 10.0.0.1     UDP 60 5049 → 22 Len=5
 1141 1025.083748    10.0.0.66 → 10.0.0.1     UDP 60 5076 → 22 Len=5
 1143 1027.167730    10.0.0.66 → 10.0.0.1     UDP 60 5076 → 22 Len=5
 1145 1029.255106    10.0.0.66 → 10.0.0.1     UDP 60 5102 → 22 Len=5
 1147 1031.334799    10.0.0.66 → 10.0.0.1     UDP 60 5051 → 22 Len=5
 1162 1043.850969    10.0.0.66 → 10.0.0.1     UDP 60 5114 → 22 Len=5
 1164 1045.934960    10.0.0.66 → 10.0.0.1     UDP 60 5051 → 22 Len=5
 1166 1048.019181    10.0.0.66 → 10.0.0.1     UDP 60 5100 → 22 Len=5
 1172 1054.255069    10.0.0.66 → 10.0.0.1     UDP 60 5095 → 22 Len=5
 1178 1060.507360    10.0.0.66 → 10.0.0.1     UDP 60 5100 → 22 Len=5
 1180 1062.619741    10.0.0.66 → 10.0.0.1     UDP 60 5097 → 22 Len=5
 1187 1066.779955    10.0.0.66 → 10.0.0.1     UDP 60 5116 → 22 Len=5
 1189 1068.867478    10.0.0.66 → 10.0.0.1     UDP 60 5097 → 22 Len=5
 1192 1070.959143    10.0.0.66 → 10.0.0.1     UDP 60 5095 → 22 Len=5
 1196 1073.043525    10.0.0.66 → 10.0.0.1     UDP 60 5118 → 22 Len=5
 1199 1075.127069    10.0.0.66 → 10.0.0.1     UDP 60 5049 → 22 Len=5
 1267 1139.786992    10.0.0.66 → 10.0.0.1     UDP 60 5097 → 22 Len=5
 1272 1141.870974    10.0.0.66 → 10.0.0.1     UDP 60 5095 → 22 Len=5
 1274 1143.955404    10.0.0.66 → 10.0.0.1     UDP 60 5115 → 22 Len=5
 1276 1146.043247    10.0.0.66 → 10.0.0.1     UDP 60 5116 → 22 Len=5
 1284 1154.383039    10.0.0.66 → 10.0.0.1     UDP 60 5051 → 22 Len=5
 1286 1156.475039    10.0.0.66 → 10.0.0.1     UDP 60 5103 → 22 Len=5
 1296 1166.882937    10.0.0.66 → 10.0.0.1     UDP 60 5048 → 22 Len=5
 1301 1168.975486    10.0.0.66 → 10.0.0.1     UDP 60 5125 → 22 Len=5
 1303 1171.059146    10.0.0.80 → 10.0.0.1     UDP 60 5000 → 22 Len=3
 ```
Filter scrport 5000 lists a bunch of packets from different hosts to different destinations
Whereas dstport 22 are mostly packets from 10.0.0.66 to 10.0.0.1, this is more likely the flag
Also notice that `112` is `p` in ascii and `102` is `i`, so the port is the flag instead of the data
```py
# Script.py
from pwn import *

os=process('sh',level='error')
os.sendline(b' tshark -r capture.pcap -T fields -e udp.srcport udp.dstport==22')
ports=os.recvlines(timeout=1)
flag=''
for p in ports[1:-1]:
    flag+=chr(int(p.decode())-5000)
print(flag)
```
```console
❯ python3 script.py
picoCTF{p1LLf3r3d_data_v1a_st3g0}
```
