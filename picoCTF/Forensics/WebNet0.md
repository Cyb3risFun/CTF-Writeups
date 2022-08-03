# **_WebNet0_**
## Description
> We found this packet capture and key. Recover the flag.

## Solution
A Google search on TLS decryption leads to this [page](https://wiki.wireshark.org/TLS)
> Go to Edit -> Preferences. Open the Protocols tree and select TLS. Alternatively, select a TLS packet in the packet list, right-click on the TLS layer in the packet details view and open the Protocol preferences menu.

Decrypt TLS and search for the flag, luckly there isn't much packets, so you can do it manually or use the search box

![image](https://user-images.githubusercontent.com/70738420/182520119-9c436cc8-f276-475d-af9c-2b4a2e33e91e.png)

Open the packet and find the flag in the `Pico-Flag` header
> picoCTF{nongshim.shrimp.crackers}
