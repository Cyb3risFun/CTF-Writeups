# **_Disk, Disk, Sleuth!_**
## Description
> Use `srch_strings` from the sleuthkit and some terminal-fu to find a flag in this disk image: dds1-alpine.flag.img.gz

## Solution
Decompress the file with `gzip`
```console
 ❯ gzip -d dds1-alpine.flag.img.gz
 ```
 Run `strings` to grab printable characters from the disk image, and look for the flag
 ```console
 ❯ strings dds1-alpine.flag.img | grep picoCTF
  SAY picoCTF{f0r3ns1c4t0r_n30phyt3_267e38f6}
```