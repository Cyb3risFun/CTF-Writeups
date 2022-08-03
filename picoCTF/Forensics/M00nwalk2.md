# **_M00nwalk2_**
## Description
> Revisit the last transmission. We think this transmission contains a hidden message. There are also some clues clue 1, clue 2, clue 3.

## Solution
`message.wav` returns the same image as [M00nwalk](https://github.com/Cyb3risFun/CTF-Writeups/blob/main/picoCTF/Forensics/M00nwalk.md)

![image](https://user-images.githubusercontent.com/70738420/182259804-4c684bca-4f00-4079-934d-11b921177571.png)

`clue1.wav` returned an image with the password `hidden_stegosaurus`

![image](https://user-images.githubusercontent.com/70738420/182260280-6c107cd0-03b2-4891-9c30-83752a0e0bd9.png)


The password reminds me of `steghide`, use the password to extract hidden files
```console
❯ steghide extract -sf message.wav
Enter passphrase:
wrote extracted data to "steganopayload12154.txt".
```
Read the extracted file and get the flag
```console
❯ cat steganopayload12154.txt
picoCTF{the_answer_lies_hidden_in_plain_sight}
```

But lets read the rest of the clues too
`clue2.wav`

![image](https://user-images.githubusercontent.com/70738420/182260809-f4e0c755-6333-43db-8104-552005eba0cf.png)

`clue3.wav`

![image](https://user-images.githubusercontent.com/70738420/182261003-59039be6-0329-4a9f-aeec-1192eed57304.png)

Clue2 doesn't seem helpful to me, but a Google search on Clue3, `Alan Eliasen th FutureBoy`, leads to this [Steganography Tools](https://futureboy.us/stegano/) page
The page mentioned the tool `steghide`, which we used to extract the hidden file from `message.wav`