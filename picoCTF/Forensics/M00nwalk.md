# **_M00nwalk_**
## Description
> Decode this message from the moon.

## Solution
First thing that came to mind is to visualize the audio, however nothing helpful is returned
I had to check hint 1
> How did pictures from the moon landing get sent back to Earth?

Research on google leads to [SSTV](https://www.wikiwand.com/en/Slow-scan_television)
This [page](https://ourcodeworld.com/articles/read/956/how-to-convert-decode-a-slow-scan-television-transmissions-sstv-audio-file-to-images-using-qsstv-in-ubuntu-18-04) explains how to transmit the audio back to png file, I found this link in [Dvd848](https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/m00nwalk.md)'s writeup

Follow the steps and start revieving the audio input, make sure you check `Auto Slant` or it will fail.

![image](https://user-images.githubusercontent.com/70738420/182037895-26e5d65a-0f3d-4785-b48a-93c2206f51e1.png)

The flag is in the png file
> picoCTF{beep_boop_im_in_space}
