---
title: "Arduino - Loading Strings from Flash Memory"
date: 2014-12-22T06:42:00
tags: [arduino, C, Christmas, progmem]
---
I got out my [Arduino](http://www.arduino.cc/) board for the first time in a while for a Christmas themed project. More on that in another post. This post is about loading strings from the Arduino's flash memory, aka program memory, aka [PROGMEM](http://www.arduino.cc/en/Reference/PROGMEM).

There is an example in the PROGMEM link above, but it's kinda ugly. Here's the main points from their example code:

```
// set up strings

prog_char string_0[] PROGMEM = "String 0";   // "String 0" etc are strings to store - change to suit.
prog_char string_1[] PROGMEM = "String 1";

// Then set up a table to refer to your strings.
PROGMEM const char *string_table[] = 	   // change "string_table" name to suit
{   
  string_0,
  string_1
};

// loading string
strcpy_P(buffer, (char*)pgm_read_word(&(string_table[i]))); // Necessary casts and dereferencing, just copy.
```

I don’t like the fact that they create global variables called string_N then load them in an array of a different type and reference the variables by array index later. It all seems rather unnecessary.

It seemed like there should be a more straight-forward way of doing things and after reading the header file and playing around for a few minutes, here’s what I came up with:

```
// create strings in flash memory
const prog_char STRING_WISH[] PROGMEM  = "Have";
const prog_char STRING_YOU[] PROGMEM   = "  a";
const prog_char STRING_MERRY[] PROGMEM = "Merry";
const prog_char STRING_XMAS[] PROGMEM  = "Xmas!";

// example reading strings
char firstLine[6];
char secondLine[6];
char thirdLine[6];
char fourthLine[6];

strcpy_P(firstLine, STRING_WISH);
strcpy_P(secondLine, STRING_YOU);
strcpy_P(thirdLine, STRING_MERRY);
strcpy_P(fourthLine, STRING_XMAS);
```

Unfortunately global variables are still required. At least I’ve given them sensible names (STRING_WISH was initially the word ‘wish’ but I later decided ‘have’ sounded better). Loading the strings from program memory into RAM doesn’t require any manipulation of the arguments. No need to remember “magic” array indices or for type casting or unnecessary calls to `pgm_read_word`.