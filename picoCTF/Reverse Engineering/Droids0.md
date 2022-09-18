# Droids0
## Description
> Where do droid logs go. Check out this file.

## Solution
The file given has an `.apk` extension, which means its for Android, I have never played with android hacking so I had to check the hint
> Try using an emulator or device

> https://developer.android.com/studio

It suggested to use a emulater provided in the second hint, so I downloaded the `Android Studio` and debugged the apk file, then ran the app in the emulator

![image](https://user-images.githubusercontent.com/70738420/190915547-4a08acef-6ee2-4a16-8ab8-933657cf690d.png)

Pressed the button then the text changed, but not to something helpful

![image](https://user-images.githubusercontent.com/70738420/190915651-05156796-ae85-4b1c-8709-1f07e1a098f9.png)

Notice that at the top of the screen, there is a line `where else can output go?`, if its not shown on the screen, it might be at the log
Search for the flag in the `logcat` window 
> 2022-09-18 08:42:29.788 8159-8159/com.hellocmu.picoctf I/PICO: picoCTF{a.moose.once.bit.my.sister}