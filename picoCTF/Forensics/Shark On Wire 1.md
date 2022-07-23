# **_Shark On Wire 1_**
## Description
> We found this packet capture. Recover the flag

## Solution
From the protocol hierarchy statistic we know that there are lots of `udp` and `arp` packets 
```console
❯ tshark -r capture.pcap -z io,phs -q

===================================================================
Protocol Hierarchy Statistics
Filter:

eth                                      frames:2317 bytes:202359
  ip                                     frames:1230 bytes:113479
    udp                                  frames:1140 bytes:107983
      ssdp                               frames:128 bytes:27137
      nbdgm                              frames:7 bytes:1731
        smb                              frames:7 bytes:1731
          mailslot                       frames:7 bytes:1731
            browser                      frames:7 bytes:1731
      llmnr                              frames:205 bytes:13924
      data                               frames:749 bytes:60996
      _ws.malformed                      frames:4 bytes:240
      mdns                               frames:47 bytes:3955
    tcp                                  frames:22 bytes:1408
    igmp                                 frames:68 bytes:4088
  ipv6                                   frames:347 bytes:44534
    udp                                  frames:280 bytes:38424
      llmnr                              frames:205 bytes:18024
      mdns                               frames:47 bytes:4895
      ssdp                               frames:7 bytes:1099
      data                               frames:21 bytes:14406
    icmpv6                               frames:67 bytes:6110
  arp                                    frames:738 bytes:44226
  lldp                                   frames:2 bytes:120
===================================================================
```
Lets write a script to follow the udp stream and see if it contains the flag
```sh
#! /bin/bash
# script.sh
END=$(tshark -r capture.pcap -T fields -e udp.stream | sort -n | tail -1)
for ((i=0;i<=END;i++))
do
    flag=$(tshark -r capture.pcap -T fields -e data.text -o data.show_as_text:TRUE udp.stream==$i| tr -d '\n') 
    if  echo $flag | grep -q picoCTF{; then
        echo stream $i
        echo $flag
    fi
done
```
Here it is
```
❯ ./script.sh
stream 6
picoCTF{StaT31355_636f6e6e}
stream 7
picoCTF{N0t_a_fLag}
```