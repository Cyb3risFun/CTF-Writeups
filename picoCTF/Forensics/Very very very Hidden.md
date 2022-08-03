# **_Very very very Hidden_**
## Description
> Finding a flag may take many steps, but if you look diligently it won't be long until you find the light at the end of the tunnel. Just remember, sometimes you find the hidden treasure, but sometimes you find only a hidden map to the treasure. try_me.pcap

## Solution
Protocol Hierarchy Statistics
```console
❯ tshark -r try_me.pcap -z io,phs -q

===================================================================
Protocol Hierarchy Statistics
Filter:

eth                                      frames:9760 bytes:9512178
  ip                                     frames:9718 bytes:9508890
    tcp                                  frames:7869 bytes:8561507
      tls                                frames:1833 bytes:1548707
        tcp.segments                     frames:674 bytes:850148
          tls                            frames:485 bytes:698665
      http                               frames:10 bytes:5276
        data-text-lines                  frames:2 bytes:836
        png                              frames:2 bytes:1152
          tcp.segments                   frames:2 bytes:1152
      data                               frames:23 bytes:1487
      _ws.malformed                      frames:1 bytes:55
    udp                                  frames:1849 bytes:947383
      quic                               frames:1693 bytes:931763
        quic                             frames:16 bytes:7859
          quic                           frames:4 bytes:5568
      dns                                frames:107 bytes:11300
      mdns                               frames:20 bytes:1424
      nbns                               frames:15 bytes:1380
      llmnr                              frames:10 bytes:652
      ssdp                               frames:4 bytes:864
  ipv6                                   frames:30 bytes:2676
    udp                                  frames:30 bytes:2676
      mdns                               frames:20 bytes:1824
      llmnr                              frames:10 bytes:852
  arp                                    frames:12 bytes:612
===================================================================
```
Notice that there are `png` files under `http` protocol, lets export it
```console
❯ tshark -r try_me.pcap --export-objects "http,./" -q
❯ ls
%2f  duck.png  evil_duck.png  favicon.ico  NothingSus  try_me.pcap
```
Anaylzed the files exported, we know that the flag might be hidden in `evil_duck.png`, since it's size is double as `duck.png`
```console
❯ ls -l *png
-rw-r--r-- 1 user user 1284036 Aug  2 13:09 duck.png
-rw-r--r-- 1 user user 2497784 Aug  2 13:09 evil_duck.png
```
But `steghide` and `binwalk` wasn't able to extract useful files from it
Lets anaylse the http packets again
```console
❯ tshark -r try_me.pcap -T fields -e http.response_for.uri  http

http://ec2-54-147-39-126.compute-1.amazonaws.com/NothingSus/

http://ec2-54-147-39-126.compute-1.amazonaws.com/favicon.ico

http://ec2-54-147-39-126.compute-1.amazonaws.com/NothingSus/duck.png

http://ec2-54-147-39-126.compute-1.amazonaws.com/NothingSus/evil_duck.png

http://powershell.org/
```
We got the all files from the list, except the link to powershell
A Google research on `Powershell Steganography` leads to this [tool](https://github.com/peewpw/Invoke-PSImage)
And the extract [tool](https://github.com/imurasheen/Extract-PSImage)
Follow the instructions to extract file from `evil_duck.png`
>PS> Import-Module .\Extract-Invoke-PSImage.ps1
PS> Extract-Invoke-PSImage -Image [path to the PNG image] -Out [path to the .ps1 file]

You might need to bypass the powershell execution policy to run the tool
> powershell -ep bypass

```ps
PS > Extract-Invoke-PSImage -image evil_duck.png -Out evil_duck.ps1
[Oneliner to extract embedded payload]
sal a New-Object;Add-Type -AssemblyName "System.Drawing";$g=a System.Drawing.Bitmap("C:\Users\arman\Desktop\evil_duck.png");$o=a Byte[] 1490837;(0..811)|%{foreach($x in(0..1222)){$p=$g.GetPixel($x,$_);$o[$_*1223+$x]=([math]::Floor(($p.B-band15)*16)-bor($p.G-band15))}};$g.Dispose();[System.Text.Encoding]::ASCII.GetString($o[0..1490831])|Out-File $Out
[First 50 characters of extracted payload]
$out = "flag.txt"
$enc = [system.Text.Encoding]::
```
The extracted script will contain a bunch of garbage, stoping it from executing, remove them and run
```ps
$out = "flag.txt"
$enc = [system.Text.Encoding]::UTF8
$string1 = "HEYWherE(IS_tNE)50uP?^DId_YOu(]E@t*mY_3RD()B2g3l?"
$string2 = "8,:8+14>Fx0l+$*KjVD>[o*.;+1|*[n&2G^201l&,Mv+_'T_B"

$data1 = $enc.GetBytes($string1)
$bytes = $enc.GetBytes($string2)

for($i=0; $i -lt $bytes.count ; $i++)
{
    $bytes[$i] = $bytes[$i] -bxor $data1[$i]
}
[System.IO.File]::WriteAllBytes("$out", $bytes)
```

```console
PS > .\evil_duck.ps1
PS > type .\flag.txt
picoCTF{n1c3_job_f1nd1ng_th3_s3cr3t_in_the_im@g3}
```


