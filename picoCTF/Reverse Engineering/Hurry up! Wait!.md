# **_Hurry up! Wait!_**
## Description
> svchost.exe

## Solution
Open with Ghidra and look around we can find this function below
```c
void FUN_0010298a(void)

{
  ada__calendar__delays__delay_for(1000000000000000);
  FUN_00102616();
  FUN_001024aa();
  FUN_00102372();
  FUN_001025e2();
  FUN_00102852();
  FUN_00102886();
  FUN_001028ba();
  FUN_00102922();
  FUN_001023a6();
  FUN_00102136();
  FUN_00102206();
  FUN_0010230a();
  FUN_00102206();
  FUN_0010257a();
  FUN_001028ee();
  FUN_0010240e();
  FUN_001026e6();
  FUN_00102782();
  FUN_001028ee();
  FUN_0010226e();
  FUN_00102136();
  FUN_0010226e();
  FUN_0010233e();
  FUN_001023da();
  FUN_0010230a();
  FUN_001021d2();
  FUN_00102956();
  return;
}
```
The whole function prints the flag 1 character at once; The first function call adds a delay to this program so the flag can't be grabbed by just executing the program
We can bypass the delay function in GDB, but I'm missing a shared object file which blocks me from executing the program, so I'll manully follow the functions and grab the flag
I'll show the process grabbing the first character
Follow the first function
```c
void FUN_00102616(void)

{
  ada__text_io__put__4(&DAT_00102cd8,&DAT_00102cb8);
  return;
}

```
Follow the parameter 
```assembly
DAT_00102cd8                                    XREF[3]:     FUN_00102616:0010261f(*), 
                                                                                          FUN_00102616:0010262d(*), 
                                                                                          FUN_00102616:00102636(*)  
        00102cd8 70              ??         70h    p

```
We get `p` for the first character, the rest of the flag can be grabbed with the same process

> picoCTF{d15a5m_ftw_717bea4}