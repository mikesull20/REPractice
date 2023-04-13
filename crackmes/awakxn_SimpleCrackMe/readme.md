# Analyzing CrackMe by awakxn

## Table of Contents
* [Introduction](#introduction)
* [Running the program](#Running-the-program)
* [Ghidra Analysis](#Ghidra-Analysis)
    - [Final Run](#Final-Run)
* [Conclusion](#Conclusion)
## Introduction

Author: Awakxn
Platform: Windows
Difficulty: 1.5
Quality: 4.0
Program Name: rev50_linux64-bit

Description: very very simple crackme!

Let's hope so...

## Running the program

```
mikesull@MSI:~/CrackMe/Awakxn_SimpleCrackMe/rev50$ ./rev50_linux64-bit
USAGE: ./rev50_linux64-bit <password>
try again!
mikesull@MSI:~/CrackMe/Awakxn_SimpleCrackMe/rev50$
```

Looking at this here, it seems we need to enter a password as a command line argument.
As much fun as it sound to brute force this, instead we will use Ghidra to open up the
executable and view the binary.

## Ghidra-Analysis

Once I opened the binary in Ghidra and found the main function, here is what we have for
what Ghidra has decompiled for us:

```
undefined8 main(int param_1,undefined8 *param_2)

{
  size_t sVar1;
  
  if (param_1 == 2) {
    sVar1 = strlen((char *)param_2[1]);
    if (sVar1 == 10) {
      if (*(char *)(param_2[1] + 4) == '@') {
        puts("Nice Job!!");
        printf("flag{%s}\n",param_2[1]);
      }
      else {
        usage(*param_2);
      }
    }
    else {
      usage(*param_2);
    }
  }
  else {
    usage(*param_2);
  }
  return 0;
}
```

We can clean this up a little using what we know about C. Main always takes an
int argc, char* argv[], so we can replace those terms. Now let's take a look
at the clearer to read version. Comments are added for understanding.

```
int main(int argc,char **argv)

{
  size_t arg_length;
  
  if (argc == 2) {  // Check for 2 arguments ("./exe arg1" would be valid)
    arg_length = strlen(argv[1]); // Get length of the argument1 
    if (arg_length == 10) {     // If arg length == 10, allow into the if statement
      // If the 5th character is a '@', allow the user into the loop and reveal the flag.
      if (argv[1][4] == '@') {
        puts("Nice Job!!");
        printf("flag{%s}\n",argv[1]);
      }
      else { // If the incorrect password is entered just print out the usage
        usage(*argv);
      }
    }
    else {
      usage(*argv);
    }
  }
  else {
    usage(*argv);
  }
  return 0;
}
```

It looks like what we need to do is clear. We need to enter in a 10 character
string that has an '@' as the 5th character.

## Final-Run

Using the string 'aaaa@aaaaa', let's see if that works as a valid password to
the program.

```
mikesull@MSI:~/CrackMe/Awakxn_SimpleCrackMe/rev50$ ./rev50_linux64-bit aaaa@aaaaa
Nice Job!!
flag{aaaa@aaaaa}
mikesull@MSI:~/CrackMe/Awakxn_SimpleCrackMe/rev50$
```
Huzzah it worked.

## Conclusion

This was a wonderful place to begin for CrackMes. I think that what was most
important for me though was that it gave me a solid place to begin learning
how to use Ghidra. I struggled before this CrackMe to understand what exactly
I should be looking for in Ghidra, but now I am starting to see it.