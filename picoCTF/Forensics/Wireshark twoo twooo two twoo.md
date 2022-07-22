# **_Wireshark twoo twooo two twoo_**
## Description
> Can you find the flag? shark2.pcapng.

## Solution
Anaylze the `pcap` file 

![image](https://user-images.githubusercontent.com/70738420/180365719-6adfafd9-d6f1-4701-8031-074a06391646.png)

Notice its mostly `DNS` and `HTTP` packets
Lets set a filter to only show the `31.3%` DNS packets
Browse through the packets, seems like its looking for subdomains of `reddshrimpandherring.com`, lets visit the server
```console
❯ curl reddshrimpandherring.com
<html>
        <head>
                <script>
                        var forwardingUrl = "/page/bouncy.php?&bpae=GbhWtzcnokx%2F9pthSTymQvgnA%2FjtMeHEkVLzKakjQRN8bSN4DLTrsKXcsck3CN%2F%2FUB9hbtnt4PRy%2BtGpZpUC2iZiRlJxL5PU7QYB%2FbP3A93hott0pVc%2FfXos9xwgPa6E0sFgcSjDerwt6GyGMFLA%2Bv5eHNmXJMve%2FQfvZzCM05vShmKjGIRfeNkIkF9StB%2FLFo7REb9YH6VY2P41Nn7Ktw9kGXRNdSPhrdNRYCGgOAysvBqIdsMjmDmxLevrpAuQkMzfTpiZzF%2Ffd9jNgWEn4z8JrtoB9Ei7XyB00r7LFUce44oD%2Byv44afg%2FbhTd0OMzr%2BBDui2IUy40hvQFxc8ZtnTcInyZxjnMOhCjHLMjilFDMLlfsoaEUF1s6KsdwcEjZsxRLt9Sdr5VFBOTeuvwP5Mtn%2FQSA%3D%3D&redirectType=js";
                        var destinationUrl = "/page/bouncy.php?&bpae=GbhWtzcnokx%2F9pthSTymQvgnA%2FjtMeHEkVLzKakjQRN8bSN4DLTrsKXcsck3CN%2F%2FUB9hbtnt4PRy%2BtGpZpUC2iZiRlJxL5PU7QYB%2FbP3A93hott0pVc%2FfXos9xwgPa6E0sFgcSjDerwt6GyGMFLA%2Bv5eHNmXJMve%2FQfvZzCM05vShmKjGIRfeNkIkF9StB%2FLFo7REb9YH6VY2P41Nn7Ktw9kGXRNdSPhrdNRYCGgOAysvBqIdsMjmDmxLevrpAuQkMzfTpiZzF%2Ffd9jNgWEn4z8JrtoB9Ei7XyB00r7LFUce44oD%2Byv44afg%2FbhTd0OMzr%2BBDui2IUy40hvQFxc8ZtnTcInyZxjnMOhCjHLMjilFDMLlfsoaEUF1s6KsdwcEjZsxRLt9Sdr5VFBOTeuvwP5Mtn%2FQSA%3D%3D&redirectType=meta";
                        var addDetection = true;
                        if (addDetection) {
                                var inIframe = window.self !== window.top;
                                forwardingUrl += "&inIframe=" + inIframe;
                                var inPopUp = (window.opener !== undefined && window.opener !== null && window.opener !== window);
                                forwardingUrl += "&inPopUp=" + inPopUp;
                        }
                        window.location.replace(forwardingUrl);
                </script>
                <noscript>
                        <meta http-equiv="refresh" content="1;url=/page/bouncy.php?&bpae=GbhWtzcnokx%2F9pthSTymQvgnA%2FjtMeHEkVLzKakjQRN8bSN4DLTrsKXcsck3CN%2F%2FUB9hbtnt4PRy%2BtGpZpUC2iZiRlJxL5PU7QYB%2FbP3A93hott0pVc%2FfXos9xwgPa6E0sFgcSjDerwt6GyGMFLA%2Bv5eHNmXJMve%2FQfvZzCM05vShmKjGIRfeNkIkF9StB%2FLFo7REb9YH6VY2P41Nn7Ktw9kGXRNdSPhrdNRYCGgOAysvBqIdsMjmDmxLevrpAuQkMzfTpiZzF%2Ffd9jNgWEn4z8JrtoB9Ei7XyB00r7LFUce44oD%2Byv44afg%2FbhTd0OMzr%2BBDui2IUy40hvQFxc8ZtnTcInyZxjnMOhCjHLMjilFDMLlfsoaEUF1s6KsdwcEjZsxRLt9Sdr5VFBOTeuvwP5Mtn%2FQSA%3D%3D&redirectType=meta" />
                </noscript>
        </head>
</html>%
```
Looks like we are redirected to another page, however following the path doesn't lead to anything helpful
Lets go back to the dns queries, browsing through the packets and notice that most DNS query packets are sent to `8.8.8.8`, which is Google's public dns server, and a few DNS query packets are sent to `18.217.1.57`, lets apply a filter to view the queries

![image](https://user-images.githubusercontent.com/70738420/180371387-a5d1ac6e-10c7-402a-9967-de515a3331fe.png)

From the screenshot above, we know that the host is looking for subdomains for `reddshrimpandherring.com`,  `reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com`, and `reddshrimpandherring.com.windomain.local`, apply a filter so that same subdomain names only appears once 

![image](https://user-images.githubusercontent.com/70738420/180374656-6a9c8acf-1fce-4ebd-adf9-be7b575085ea.png)

Its quite obvious that the subdomains are `base64` encoded, lets combine them and decode the flag
```console
❯ tshark -r shark2.pcapng  -T fields -e dns.qry.name dns and ip.dst== 18.217.1.57 and  dns.qry.name contains local
cGljb0NU.reddshrimpandherring.com.windomain.local
RntkbnNf.reddshrimpandherring.com.windomain.local
M3hmMWxf.reddshrimpandherring.com.windomain.local
ZnR3X2Rl.reddshrimpandherring.com.windomain.local
YWRiZWVm.reddshrimpandherring.com.windomain.local
fQ==.reddshrimpandherring.com.windomain.local
fQ==.reddshrimpandherring.com.windomain.local
❯ tshark -r shark2.pcapng  -T fields -e dns.qry.name dns and ip.dst== 18.217.1.57 and  dns.qry.name contains local | awk -F. '{ print $1}'|tr -d "\n"| sed 's/fQ==//'
cGljb0NURntkbnNfM3hmMWxfZnR3X2RlYWRiZWVmfQ==
❯ tshark -r shark2.pcapng  -T fields -e dns.qry.name dns and ip.dst== 18.217.1.57 and  dns.qry.name contains local | awk -F. '{ print $1}'|tr -d "\n"| sed 's/fQ==//' | base64 -d
picoCTF{dns_3xf1l_ftw_deadbeef}
```