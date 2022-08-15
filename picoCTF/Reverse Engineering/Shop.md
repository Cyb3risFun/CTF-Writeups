# **_Shop_**
## Description
> Best Stuff - Cheap Stuff, Buy Buy Buy... Store Instance: source. The shop is open for business at nc mercury.picoctf.net 11371.

## Solution
Play around with the source code
```console
❯ ./source
Welcome to the market!
=====================
You have 40 coins
        Item            Price   Count
(0) Quiet Quiches       10      12
(1) Average Apple       15      8
(2) Fruitful Flag       100     1
(3) Sell an Item
(4) Exit
Choose an option:
0
How many do you want to buy?
1

You have 30 coins
        Item            Price   Count
(0) Quiet Quiches       10      11
(1) Average Apple       15      8
(2) Fruitful Flag       100     1
(3) Sell an Item
(4) Exit
Choose an option:
0
How many do you want to buy?
-20

You have 230 coins
        Item            Price   Count
(0) Quiet Quiches       10      31
(1) Average Apple       15      8
(2) Fruitful Flag       100     1
(3) Sell an Item
(4) Exit
Choose an option:
```
Notice that there is no input validation, we can enter a negative value when purchasing to gain coins
Connect to the target and purchase the flag
```console
❯ nc mercury.picoctf.net 11371
Welcome to the market!
=====================
You have 40 coins
        Item            Price   Count
(0) Quiet Quiches       10      12
(1) Average Apple       15      8
(2) Fruitful Flag       100     1
(3) Sell an Item
(4) Exit
Choose an option:
0
How many do you want to buy?
-20
You have 240 coins
        Item            Price   Count
(0) Quiet Quiches       10      32
(1) Average Apple       15      8
(2) Fruitful Flag       100     1
(3) Sell an Item
(4) Exit
Choose an option:
2
How many do you want to buy?
1
Flag is:  [112 105 99 111 67 84 70 123 98 52 100 95 98 114 111 103 114 97 109 109 101 114 95 98 56 100 55 50 55 49 102 125]
```
Flag is in Ascii format, convert it back to characters
```py
❯ python3 -q
>>> flag='112 105 99 111 67 84 70 123 98 52 100 95 98 114 111 103 114 97 109 109 101 114 95 98 56 100 55 50 55 49 102 125'.split(' ')
>>> [ print(chr(int(c)),end='') for c in flag]
picoCTF{b4d_brogrammer_b8d7271f}
