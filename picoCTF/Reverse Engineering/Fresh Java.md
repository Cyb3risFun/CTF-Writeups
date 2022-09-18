# **_Fresh Java_**
## Description
> Can you get the flag?
Reverse engineer this Java program.

## Solution
Decompile the file, we get a `java` file, it compares the input with the flag character by character, in reverse order
```java
  cVar3 = objectRef.charAt(0x21);
  if (cVar3 != '}') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x20);
  if (cVar3 != 'e') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1f);
  if (cVar3 != 'b') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1e);
  if (cVar3 != '6') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1d);
  if (cVar3 != 'a') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1c);
  if (cVar3 != '2') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1b);
  if (cVar3 != '3') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1a);
  if (cVar3 != '3') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x19);
  if (cVar3 != '9') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x18);
  if (cVar3 != '_') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x17);
  if (cVar3 != 'd') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x16);
  if (cVar3 != '3') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x15);
  if (cVar3 != 'r') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x14);
  if (cVar3 != '1') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x13);
  if (cVar3 != 'u') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x12);
  if (cVar3 != 'q') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x11);
  if (cVar3 != '3') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x10);
  if (cVar3 != 'r') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xf);
  if (cVar3 != '_') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xe);
  if (cVar3 != 'g') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xd);
  if (cVar3 != 'n') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xc);
  if (cVar3 != '1') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xb);
  if (cVar3 != 'l') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(10);
  if (cVar3 != '0') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(9);
  if (cVar3 != '0') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(8);
  if (cVar3 != '7') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(7);
  if (cVar3 != '{') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(6);
  if (cVar3 != 'F') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(5);
  if (cVar3 != 'T') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(4);
  if (cVar3 != 'C') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(3);
  if (cVar3 != 'o') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(2);
  if (cVar3 != 'c') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(1);
  if (cVar3 != 'i') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0);
  if (cVar3 != 'p') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
```
Gather the characters to get the flag
> picoCTF{700l1ng_r3qu1r3d_9332a6be}