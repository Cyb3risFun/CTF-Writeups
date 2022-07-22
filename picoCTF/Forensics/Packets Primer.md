# **_Packets Primer_**
## Description
> Download the packet capture file and use packet analysis software to find the flag.

## Solution
Browse through the packets, and the flag will appear

![image](https://user-images.githubusercontent.com/70738420/180408089-57bc8607-6542-4c83-a2a5-0112b2a15de9.png)


```console
❯ echo "7020692063206f204320542046207b207020342063206b20332037205f2035206820342072206b205f20622039206420352033203720362035207d0a" | xxd -r -p
p i c o C T F { p 4 c k 3 7 _ 5 h 4 r k _ b 9 d 5 3 7 6 5 }
❯ echo "7020692063206f204320542046207b207020342063206b20332037205f2035206820342072206b205f20622039206420352033203720362035207d0a" | xxd -r -p | tr -d " "
picoCTF{p4ck37_5h4rk_b9d53765}
```