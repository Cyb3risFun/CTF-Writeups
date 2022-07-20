# **_Trivial Flag Transfer Protocol_**
## Description
> Figure out how they moved the flag.

## Solution
Browse the `pcap` file, notice most packets are `tftp` protocol packets
Lets export the files, in `Wireshark`
> File>Export Objects>TFTP

```console
❯ ls
instructions.txt  picture1.bmp  picture2.bmp  picture3.bmp  plan  program.deb  tftp.pcapng
```
Lets take a look at `instructions.txt`
```console
❯ cat instructions.txt
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
```
The text is encoded, lets decode it
This looks like a `base64` encoded string
```console
❯ cat instructions.txt | base64 -d
!�@Ed@<B��CRIS��YQ�TT�TTAD�51�4RDbase64: invalid input
```
But its not, so I used a [online tool](https://www.boxentriq.com/code-breaking/cryptogram) to decode it and got the result
>tftp doesnt encrypt our traffic so we must disguise our flag transfer figure out away to hide the flag and i will check back for the plan

Turns out its `Rot13` encoded
```console
❯ cat instructions.txt | rot13
TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN
```
Decoded text mentioned the `plan` file, lets take a look
```console
❯ cat plan
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
```
Seems like its `Rot13` encoded again
```console
❯ cat plan | rot13
IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS
```
Adding space to the text
>I USED THE PROGRAM AND HID IT WITH - DUE DILIGENCE. CHECK OUT THE PHOTOS

The text mentioned the it used the `program` to hide something in the `photos` with `DUE DILIGENCE`
Lets check out the `program.deb` with `dpkg-deb`, `-I` shows the info of the `deb` package
```console
❯ dpkg-deb -I program.deb
 new Debian package, version 2.0.
 size 138310 bytes: control archive=1250 bytes.
     826 bytes,    18 lines      control
    1184 bytes,    17 lines      md5sums
 Package: steghide
 Source: steghide (0.5.1-9.1)
 Version: 0.5.1-9.1+b1
 Architecture: amd64
 Maintainer: Ola Lundqvist <opal@debian.org>
 Installed-Size: 426
 Depends: libc6 (>= 2.2.5), libgcc1 (>= 1:4.1.1), libjpeg62-turbo (>= 1:1.3.1), libmcrypt4, libmhash2, libstdc++6 (>= 4.9), zlib1g (>= 1:1.1.4)
 Section: misc
 Priority: optional
 Description: A steganography hiding tool
  Steghide is steganography program which hides bits of a data file
  in some of the least significant bits of another file in such a way
  that the existence of the data file is not visible and cannot be proven.
  .
  Steghide is designed to be portable and configurable and features hiding
  data in bmp, wav and au files, blowfish encryption, MD5 hashing of
  passphrases to blowfish keys, and pseudo-random distribution of hidden bits
```
So it uses `steghide` to hide the flag in the photos
```
❯ l pic*
-rw-r--r-- 1 user user 806K Jul 20 00:18 picture1.bmp
-rw-r--r-- 1 user user  35M Jul 20 00:18 picture2.bmp
-rw-r--r-- 1 user user 1.4M Jul 20 00:18 picture3.bmp
```
Extract with `steghide`
```console
❯ steghide extract -sf picture1.bmp
Enter passphrase:
```
It requires a passphrase, reminds me the word `DUE DILIGENCE` which is mentioned in the `plan`
```console
❯ steghide extract -sf picture1.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
❯ steghide extract -sf picture2.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
❯ steghide extract -sf picture3.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
```
However it didn't work, since the orginial text doesn't contain a space, lets remove the space and try again
```console
❯ steghide extract -sf picture1.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
❯ steghide extract -sf picture2.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
❯ steghide extract -sf picture3.bmp
Enter passphrase:
wrote extracted data to "flag.txt".
```
Extracted `flag.txt` from `picture3.bmp` 
Read `flag.txt` for the flag
```console
❯ cat flag.txt
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

