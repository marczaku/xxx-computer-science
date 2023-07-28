# Numeric Systems

## Power

${base}^{power}$
- nth power of the base

Power describes how often 1 is multiplied with the base:
- $3^2 = 1\times3\times3 = 9$
- $2^5 = 1\times2\times2\times2\times2\times2 = 32$

The default power is 1:
- $3^1 = 3$

A power of 0 always results in the number 1:
- $3^0 = 1$
- $1577^0 = 1$

A negative power describes how often 1 is divided by that number:
- $3^{-1} = \frac{1}{3} = 0.333\dots$
- $2^{-2} = \frac{1}{2\times2} = \frac{1}{4} = 0.25$

As a period increases/decreases, the result is multiplied/divided by the base:
- $3^4 =   81 = 1\times3\times3\times3\times3$
- $3^3 =   27 = 1\times3\times3\times3$
- $3^2 =    9 = 1\times3\times3$
- $3^1 =    3 = 1\times$
- $3^0 =    1 = 1$
- $3^{-1} = 0.33 = 1\div3$
- $3^{-2} = 0.11 = 1\div3\div3$
- $3^{-3} = 0.037 = 1\div3\div3\div3$

## Decimal System

### Digits
- $0,1,2,3,4,5,6,7,8,9$
- 10 different digits
- ergo: `Decimal`

### Numbers
- We can count from $0-9$. What then?
- We need an extra Digit: $10$
- The value of the second digit is 10 times higher
  - because the first digit changes value 10 times
  - before the second digit changes its value once:
    - second digit: 0
    - first digit: $0,1,2,3,4,5,6,7,8,9$
    - second digit: $1$
    - first digit: $0,1,2,3,4,5,6,7,8,9$
    - second digit: $2$
- This goes on to hundreds and thousands:
- The value of the third digit is 100 times higher
  - because the first digit changes the value 10 times
  - before the second digit changes its value once
  - the second digit changes its value 10 times
  - before the third digit changes its value once:
  - $10\times10 = 100$
  - $0\dots10\dots20\dots30\dots40\dots90\dots97,98,99,100$

The value of each digit can be expressed through powers of 10:

|Thousand|Hundred|Ten|Single|
|:-----:|:-----:|:-:|:----:|
|1000|100|10|1|
|$10^3$|$10^2$|$10^1$|$10^0$|

### Calculating Decimal Number Values
|$10^3$|$10^2$|$10^1$|$10^0$|
|:-----:|:-----:|:-:|:----:|
|0|2|3|4|

$0\times10^3 = 0\times1000 = 0$\
$+2\times10^2 = 2\times100 = 200$\
$+3\times10^1 = 3\times10 = 30$\
$+4\times10^0 = 4\times1 = 4$\
$= 200 + 30 + 4 = 234$

### Multiplication with 10
We add a zero:
- $234\times10 = 2340$

Or rather: we move the decimal point one digit to the right:
- $234.00\times10 = 2340.0$
- $234.71\times10 = 2347.1$

### Division by 10
We remove a zero:
- $2340\div10 = 234$

Or rather: we move the decimal point one digit to the left:
- $2340.0\div10 = 234.00$
- $2347.1\div10 = 234.71$

### Divisible by 10
We can easily tell whether a number is divisible by 10:
- this means that there is no remainder/fraction left after division
- look, at whether the last digit is a zero:
- $2340$ is divisible by 10: $2340\div10=234$
- $234$ is not divisible by 10: $23 + \frac{4}{10}$

### Adding Decimal Numbers
```
 6789    6789    6789    6789    6789    6789
+4567 = +4567 = +4567 = +4567 = +4567 = +4567
           1      11     111    1111    1111 
-----   -----   -----   -----   -----   -----
            6      56     356    1356   11356
```

- we add numbers one digit at a time from small to large
- anytime the sum of two digits is larger than 9:
  - we add 1 to the next digit
  - e.g.: 9+7 = 16, therefore 6 on 1st digit + 1 on 2nd

### Subtracting Decimal Numbers
```
 6421    6421    6421    6421    6421
-5793 = -5793 = -5793 = -5793 = -5793
           1      11     111     111 
-----   -----   -----   -----   -----
            8      28     628     628
```

- we subtract numbers one digit at a time from small to large
- anytime the subtrahend is larger than the minuend
  - we borrow 1 from the next digit
  - to decrease the current minuend's digit by 10:
  - e.g.: 1-3 = -2, so we borrow 10: 11-3 = 8 on 1st and -1 on 2nd

## Binary System

### Digits
- $0,1$
- 2 different digits
- ergo: `Binary`

### Numbers
- We can count from $0-1$. What then?
- We need an extra Digit: $10$
- The value of the second digit is 2 times higher
  - because the first digit changes value 2 times
  - before the second digit changes its value once:
    - $000$
    - $001$
    - $010$
    - $011$
- This goes on to more multiples of $2$ like $5,8,16,\dots$:
- The value of the third digit is $4$ times higher
  - because the first digit changes the value $2$ times
  - before the second digit changes its value once
  - the second digit changes its value $2$ times
  - before the third digit changes its value once:
  - $2\times2 = 4$
  - $1,10,11,100,101,110,111,\dots$

The value of each digit can be expressed through powers of 2:

|Eights|Fours|Twos|Single|
|:-----:|:-----:|:-:|:----:|
|8|4|2|1|
|$2^3$|$2^2$|$2^1$|$2^0$|

### Calculating Binary Number Values
|$2^3$|$2^2$|$2^1$|$2^0$|
|:-----:|:-----:|:-:|:----:|
|1|0|1|1|

$1\times2^3 = 1\times8 = 8$\
$+0\times2^2 = 0\times4 = 0$\
$+1\times2^1 = 1\times2 = 2$\
$+1\times2^0 = 1\times1 = 1$\
$= 8 + 2 + 1 = 11$

### Creating Binary Numbers
What's 9 as a binary number?

|$16$|$8$|$4$|$2$|$1$|
|:-----:|:-----:|:-----:|:-:|:----:|
|?|?|?|?|?|

Fill in from left to right:

Does 16 fit in 9? No, so place a zero:
|$16$|$8$|$4$|$2$|$1$|
|:-----:|:-----:|:-----:|:-:|:----:|
|0|?|?|?|?|

Does 8 fit in 9? Yes, so place a one and subtract it from 9:
|$16$|$8$|$4$|$2$|$1$|
|:-----:|:-----:|:-----:|:-:|:----:|
|0|1|?|?|?|

Does 4 fit in 1? No, so place a zero:
|$16$|$8$|$4$|$2$|$1$|
|:-----:|:-----:|:-----:|:-:|:----:|
|0|1|0|?|?|

Does 2 fit in 1? No, so place a zero:
|$16$|$8$|$4$|$2$|$1$|
|:-----:|:-----:|:-----:|:-:|:----:|
|0|1|0|0|?|

Does 1 fit in 1? Yes, so place a one and subtract it from 1:
|$16$|$8$|$4$|$2$|$1$|
|:-----:|:-----:|:-----:|:-:|:----:|
|0|1|0|0|1|

$01001$ is Binary for 9.

### Multiplication with 2
We add a zero:
- $101\times10 = 1010$

Or rather: we move the decimal point one digit to the right:
- $101.00\times10 = 1010.0$
- $101.11\times10 = 1011.1$

### Division by 2
We remove a zero:
- $1010\div10 = 101$

Or rather: we move the decimal point one digit to the left:
- $1010.0\div10 = 101.00$
- $1011.1\div10 = 101.11$

### Divisible by 2
We can easily tell whether a number is divisible by 2:
- this means that there is no remainder/fraction left after division
- look, at whether the last digit is a zero:
- $1010$ is divisible by 10: $1010\div10=101$
- $101$ is not divisible by 10: $10 + \frac{1}{10}$

### Adding Decimal Numbers
```
 0101    0101    0101    0101    0101
+0110 = +0110 = +0110 = +0110 = +0110
                         1       1   
-----   -----   -----   -----   -----
            1      11     011    1011
```

- we add numbers one digit at a time from small to large
- anytime the sum of two digits is larger than 1:
  - we add 1 to the next digit
  - e.g.: 1+1 = 10, therefore 0 on 1st digit + 1 on 2nd

### Subtracting Decimal Numbers
```
 1010    1010    1010    1010    1010
-0110 = -0110 = -0110 = -0110 = -0110
                         1       1   
-----   -----   -----   -----   -----
            0      00     100    0100
```

- we subtract numbers one digit at a time from small to large
- anytime the subtrahend is larger than the minuend
  - we borrow 1 from the next digit
  - to decrease the current minuend's digit by 10:
  - e.g.: 0-1 = -1, so we borrow 10: 11-1 = 0 on 1st and -1 on 2nd
