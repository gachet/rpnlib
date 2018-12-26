# RPNlib

RPNlib is a **Reverse Polish Notation** calculator for ESP8266 & ESP32 microcontrollers. 
The library accepts a c-string with commands to execute and provides methods to evaluate the output.
It is meant to be embedded into third party software as a way to provide the user a simple to implement scripting language.

[![version](https://img.shields.io/badge/version-0.0.2-brightgreen.svg)](CHANGELOG.md)
[![codacy](https://img.shields.io/codacy/grade/dca10aead98240db83c23ef550b591dc/master.svg)](https://www.codacy.com/app/xoseperez/rpnlib/dashboard)
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

* Create the context (where stack, variables and operators are stored)
* Initialize the context (loads default operators)
* Add any required variables (optional)
* Add any additional custom operators (optional)
* Process a command
* Inspect stack
* Clear context

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

rpn_clear(ctxt);
```

## Supported operators

This is a list of supported operators with their stack behaviour. 
Operators (and variables) are case-sensitive.
All operators will throw an error if the number of available elements in the stack is less than the required parameters.
All elements in the stack are repesented as natural (float) numbers. True is represented as 1, whilst false is represented as 0.
Some operators perform an automatic boolean cast of the elements poped from the stack. If the value is 0 the cast result is false, in any other case the result is true.


```

pi      ( -> a ) where a is the value of PI
e       ( -> a ) where a is the value of e (base of the neperian logarithms)

+       ( a b -> a+b )
-       ( a b -> a-b )
*       ( a b -> a*b )
/       ( a b -> a/b ) throws error if b==0
mod     ( a b -> a\b ) integer modulus

round   ( a n -> b ) where b is a rounded to the n-th decimal
ceil    ( a -> b ) where b is a rounded to the closes greater or equal integer
floor   ( a -> b ) where b is a rounded to the closes lesser or equal integer
int     ( a -> b ) alias for "floor"

sqrt    ( a -> b ) where b is the square root of a
log     ( a -> b ) where b is the neperian logarithm of a
log10   ( a -> b ) where b is the base-10 logarithm of a
exp     ( a -> b ) where b is the a power of e (base of the neperian logarithms)
fmod    ( a b -> c ) real number modulus
pow     ( a b -> c ) where c is the b power of a
cos     ( a -> b ) where b is the cosinus of a, a in radians
sin     ( a -> b ) where b is the sinus of a, a in radians
tan     ( a -> b ) where b is the tangent of a, a in radians

==      ( a b -> c ) where if a==b then c=1 else c=0
!=      ( a b -> c ) where if a!=b then c=1 else c=0
>       ( a b -> c ) where if a>b then c=1 else c=0
>=      ( a b -> c ) where if a>=b then c=1 else c=0
<       ( a b -> c ) where if a<b then c=1 else c=0
<=      ( a b -> c ) where if a<=b then c=1 else c=0

and     ( a b -> c ) where c is 1 if both a and b are different from 0
or      ( a b -> c ) where c is 1 if a or b are different from 0
xor     ( a b -> c ) where c is 1 if either a or b are different from 0, but not both
not     ( a -> b ) where b is 1 if a is 0, 0 otherwise

dup     ( a -> a a )
dup2    ( a b -> a b a b )
swap    ( a b -> b a )
rot     ( a b c -> b c a )
unrot   ( a b c -> c a b )
drop    ( a ->  )
over    ( a b -> a b a )
depth   ( a b c ... -> a b c ... n ) where n is the number of elements in the stack

ifn     ( a b c -> d ) where if a!=0 then d=b else d=c

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
