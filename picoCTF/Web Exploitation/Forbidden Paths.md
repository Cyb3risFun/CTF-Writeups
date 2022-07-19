# **_Forbidden Paths_**
## Description
Can you get the flag?
Here's the website.
We know that the website files live in `/usr/share/nginx/html/` and the flag is at `/flag.txt` but the website is filtering absolute file paths. Can you get past the filter to read the flag?
## Solution
There is a search box in the website, type in the filename and it will read it, the description says that it is filtering `absolute paths`, so all we need it pass in a `relative path`
The webserver is hosted at `/usr/share/nginx/html/` and the flag is at `/` directory, which means four `../`
Search `../../../../flag.txt` to read the flag
>picoCTF{7h3_p47h_70_5ucc355_e5fe3d4d}