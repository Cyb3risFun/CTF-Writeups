# _It is my Birthday_
## Description
I sent out 2 invitations to all of my friends for my birthday! I'll know if they get stolen because the two invites look similar, and they even have the same md5 hash, but they are slightly different! You wouldn't believe how long it took me to find a collision. Anyway, see if you're invited by submitting 2 PDFs to my website. `http://mercury.picoctf.net:20277/`
## Solution
The description says that its looking with two pdfs with same `md5` hash, the first thing that came in mind is the md5 collision, so I looked online for the md5 collision file, found it in this [page](https://www.mscs.dal.ca/~selinger/md5collision/), download the files, change extension to `pdf`, and upload it, then I got the flag
```php
<?php

if (isset($_POST["submit"])) {
    $type1 = $_FILES["file1"]["type"];
    $type2 = $_FILES["file2"]["type"];
    $size1 = $_FILES["file1"]["size"];
    $size2 = $_FILES["file2"]["size"];
    $SIZE_LIMIT = 18 * 1024;

    if (($size1 < $SIZE_LIMIT) && ($size2 < $SIZE_LIMIT)) {
        if (($type1 == "application/pdf") && ($type2 == "application/pdf")) {
            $contents1 = file_get_contents($_FILES["file1"]["tmp_name"]);
            $contents2 = file_get_contents($_FILES["file2"]["tmp_name"]);

            if ($contents1 != $contents2) {
                if (md5_file($_FILES["file1"]["tmp_name"]) == md5_file($_FILES["file2"]["tmp_name"])) {
                    highlight_file("index.php");
                    die();
                } else {
                    echo "MD5 hashes do not match!";
                    die();
                }
            } else {
                echo "Files are not different!";
                die();
            }
        } else {
            echo "Not a PDF!";
            die();
        }
    } else {
        echo "File too large!";
        die();
    }
}

// FLAG: picoCTF{c0ngr4ts_u_r_1nv1t3d_da36cc1b}
```
However when I look at the hint
>How may a PHP site check the rules in the description?

Seems like its looking for a php comparison exploit, so I searched online and found this [page](https://stackoverflow.com/questions/22140204/why-md5240610708-is-equal-to-md5qnkcdzo), put the strings in pdf and upload them to get the flag
```console
❯ echo -n "240610708">1.pdf
❯ echo -n "QNKCDZO">2.pdf
```

