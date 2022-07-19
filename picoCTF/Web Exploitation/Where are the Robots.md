# _Where are the Robots_
## Description
Can you find the robots? `https://jupiter.challenges.picoctf.org/problem/36474/`
## Solution
Big hint in the challenge name, go to `robots.txt`, showed a html page
```
User-agent: *
Disallow: /477ce.html
```
Go to `/477ce.html` and get the flag
```
Guess you found the robots
picoCTF{ca1cu1at1ng_Mach1n3s_477ce}
```