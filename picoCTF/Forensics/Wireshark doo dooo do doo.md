# **_Wireshark doo dooo do doo_**
## Description
> Can you find the flag? shark1.pcapng.

## Solution
Browse through the `pcap` file, notice it's mostly `http` packets with some `tcp` packets
Lets filter the packets and only show `http` packet
> filter: http

Browse through `http` packets, notice that packet 827 contains plain text

![image](https://user-images.githubusercontent.com/70738420/179882465-60138a43-d094-44cf-950d-44e1db245468.png)

Open the packet, the text data seems `Rot13` encrypted

![image](https://user-images.githubusercontent.com/70738420/179882641-2961a013-8b72-4173-a618-11cec9333532.png)

Decrypt the text
> I used `rot13` from `bsdgames` 

```
❯ echo "Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}" | rot13
The flag is picoCTF{p33kab00_1_s33_u_deadbeef}
```