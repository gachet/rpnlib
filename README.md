# RPNlib

RPNlib is a **Reverse Polish Notation** calculator for ESP8266 & ESP32 microcontrollers. 
The library accepts a c-string with commands to execute and provides methods to evaluate the output.
It is meant to be embedded into third party software as a way to provide the user a simple to implement scripting language.

[![version](https://img.shields.io/badge/version-0.0.1-brightgreen.svg)](CHANGELOG.md)
[![travis](https://travis-ci.org/xoseperez/rpnlib.svg?branch=master)](https://travis-ci.org/xoseperez/rpnlib)
[![license](https://img.shields.io/github/license/xoseperez/rpnlib.svg)](LICENSE)
<br />
[![donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=xose%2eperez%40gmail%2ecom&lc=US&no_note=0&currency_code=EUR&bn=PP%2dDonationsBF%3abtn_donate_LG%2egif%3aNonHostedGuest)
[![twitter](https://img.shields.io/twitter/follow/xoseperez.svg?style=social)](https://twitter.com/intent/follow?screen_name=xoseperez)

## RPN

First you should familiarize yourself with RPN calculation. 
Reverse Polish notation (RPN), also known as Polish postfix notation or simply postfix notation, is a mathematical notation in which operators follow their operands, in contrast to Polish notation (PN), in which operators precede their operands. It does not need any parentheses as long as each operator has a fixed number of operands. The description "Polish" refers to the nationality of logician Jan Łukasiewicz, who invented Polish notation in 1924.

A simple calculation in infix notation might look like this:

```
( 4 - 2 ) * 5 + 1 =
```

The same calculation in RPN (postfix) will look like this:

```
4 2 - 5 * 1 +
```

It results in a shorter expression since parenthesis are unnecessary. Also the equals sign is not needed since all results are stored in the stack. From the computer point of view is much simpler to evaluate since it doesn't have to look forward for the operands.

Check this [wiki page on the topic](https://en.wikipedia.org/wiki/Reverse_Polish_notation).

## Library usage

The RPNlib is not an object-based (OOP) library but a context-based set of methods. This means you don't instantiate a library object but instead, you create a data context object that is passed along to all methods in the library.

Using the library is pretty easy. Follow this steps:

* Create the context (where stack, variables and functions are stored)
* Initialize the functions
* Add any required variables (optional)
* Process a command
* Inspect stack

A simple code would be:

```
rpn_context ctxt;
rpn_init(ctxt);
rpn_process(ctxt, "4 2 - 5 * 1 +");

unsigned char size = rpn_stack_size(ctxt);
Serial.printf("Stack size: %d\n", size);

float value;
for (unsigned char i=0; i<size; i++) {
    rpn_stack_pop(ctxt, value);
    Serial.printf("Stack level #%d value: %f\n", i, value);
}
```

## License

Copyright (C) 2018 by Xose Pérez <xose dot perez at gmail dot com>

The rpnlib library is free software: you can redistribute it and/or modify
it under the terms of the GNU Lesser General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

The rpnlib library is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public License
along with the rpnlib library.  If not, see <http://www.gnu.org/licenses/>.
