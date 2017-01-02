LibNMS
======

This project is a compliment to my [no-more-secrets project](https://github.com/bartobri/no-more-secrets)
which provides a comamnd line tool for reproducing the famouse
"decrypting text" effect shown in the 1992 movie [Sneakers](https://www.youtube.com/watch?v=F5bAa6gFvLs&t=35).

This project provides a dynamically linkable library that provides a
simple interface for reproducing this effect in other projects.

This library relies on ANSI/VT100 escape sequences. Most modern terminal
programs support these sequences. If yours does not, you can install and
run this through an alternate terminal program like xTerm.

Effect example:

SCREEN SHOT HERE

Table of Contents
-----------------

1. [Download and Install](#download-and-install)
2. [Usage](#usage)
3. [License](#license)

Download and Install
--------------------

In order to download and build this library, you will need to have `git`,
`gcc`, and `make` installed. Install them from your package manager if not
already installed.

```
$ which make
/usr/bin/make

$ which gcc
/usr/bin/gcc

$ which git
/usr/bin/git
```

Download and Install:

```
$ git clone https://github.com/bartobri/libnms.git
$ cd libnms
$ make
$ sudo make install
```

Uninstall:

```
$ sudo make uninstall
```

Usage
-----

**Synopsys**
```
include "libnms.h"

int main(void) {

    // Apply the effect to the string "SETEC ASTRONOMY"
    nms_exec("setec astronomy");
    
    // Make the foreground color of the decrypted characters "red"
    nms_set_foreground_color("red");
    nms_exec("setec astronomy");
    
    // Initiate decryption sequence without requiring a key press
    nms_set_auto_decrypt(1);
    nms_exec("setec astronomy");
    
    // Require the user to choose 1, 2, or 3 before returning execution to main()
    nms_set_return_opts("123");
    char c = nms_exec("Choose: [1] apples [2] oranges [3] pears");
    printf("User chose %c", c);
    
    // Set the cursor position to 0/2 (x/y) when getting user selection
    nms_set_input_position(0, 2);
    nms_set_return_opts("123");
    char c = nms_exec("Choose: [1] apples [2] oranges [3] pears");
    printf("User chose %c", c);
    
    return 0;
}
```

**Functions**

`char nms_exec(char *)`

The nms_exec() function accepts a character pointer to a string and performs
the decryption effect on it. By default the effect will "type" the character
on to the screen and then wait for the user to press a key to initiate the
decryption sequence. Once decryption has been completed, it will again wait
for the user to press a key before returning execution to the calling function.
The last character pressed is returned.

`void nms_set_foreground_color(char *)`

The nms_set_foreground_color() function accepts a character pointer to a
string indicating the desired foreground color of the decrypted characters.
Valid options are "white", "yellow", "black", "magenta", "blue", "green",
"red" and "cyan". Blue is the default. No value is returned.

`void nms_set_auto_decrypt(int)`

The nms_set_auto_decrypt() function accepts an integer and, if evaluated
as true, sets a flag that nms_exec() uses to initiate the decryption sequence
without the need for the user to press a key. If the integer argument
is evaluated as false, it unsets the flag.

`void nms_set_return_opts(char *)`

The nms_set_return_opts() function accepts a character pointer to a string
containing the character options that the user much press in order to
return execution to the calling function. This is intended to be used for
cases where the decrypted characters reveal a menu with a set of
selections that must be chosen from. Note that nms_exec() returns the
selection to the calling function.

`void nms_set_input_position(int, int)`

The nms_set_input_position() sets the x/y coordinates for the cursor position
when nms_exec() pauses for user input after the decryption sequence. This is
meant to be used in conjunction with the nms_set_return_opts() to move the
cursor to an appropriate place for input when the decryption sequence reveals
a menu.

License
-------

This program is free software; you can redistribute it and/or modify it under the terms of the the
MIT License (MIT). See [LICENSE](LICENSE) for more details.
