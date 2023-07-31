# Serializing Numbers
In this chapter, we'll learn about how numbers are serialized on our CPU.

This will enable you to always pick the right type in order to:
- avoid wasting memory (RAM or on the Hard Drive)
- avoid number overflows (which can cause e.g. the player's health to drop from 255 to 0 instead of going 256)
- avoid bugs caused by lack of precision

## Unsigned Integers
For unsigned integers, the obvious solution is to make the byte's numeric value equal to the value of the number, e.g.:

|   Byte  |Hexadecimal|Byte Value|Numeric Value|
|:-------:|:---------:|:--------:|:-----------:|
|0000 0000|    00     |     0    |      0      |
|0000 0001|    01     |     1    |      1      |
|0000 1111|    0F     |     15   |     15      |
|1111 1111|    FF     |    255   |     255     |

This is how the C# Type `byte` and the C++ Type `char` work.

### Larger Numbers
If we want to store larger numbers, we just use additional bytes. In this case Little-Endian:

|       Bytes       |Hexadecimal|Bytes Value|Numeric Value|
|:-----------------:|:---------:|:---------:|:-----------:|
|0000 0000 0000 0000|   00 00   |      0    |      0      |
|0000 0001 0000 0000|   01 00   |      1    |      1      |
|0000 1111 0000 0000|   0F 00   |      15   |     15      |
|1111 1111 0000 0000|   FF 00   |     255   |     255     |
|0000 0000 0000 0001|   00 01   |     256   |     256     |
|0000 0011 0000 0001|   03 02   |     515   |     515     |
|1111 1111 1111 1111|   FF FF   |  65,535   |   65,535    |

This is how the C# Type `ushort` and the C++ Type `uint16_t` work.

### Overview of Unsigned Integer Types

|Byte Size|C# Type|C++ Type|Max Value|
|:-------:|:-----:|:------:|:-------:|
|1 byte|`byte`|`uint8_t`|255|
|2 bytes|`ushort`|`uint16_t`|65,535|
|4 bytes|`uint`|`uint32_t`|4,294,967,295|
|8 bytes|`ulong`|`uint64_t`|18,446,744,073,709,551,615|

### Integer Overflow & Underflow

We know that we can add to integer numbers:
```cs
byte number = 5;
number += 10;
```

We also know now, what happens in Memory:

```
 0000 0101 (5)
+0000 1010 (10)
----------
=0000 1111 (15)
```

But what happens, if we go over the limits of one byte?
```cs
byte number = 255;
number++;
```

```
   1111 1111 (255)
  +0000 0001 (1)
-----------
(1)0000 0000 (0)
```
Turns out the result is `0` because the bit #9 that repreents the value `256` is not part of a byte.

Integers can overflow even further:
```cs
byte number = 255;
number+=5;
```

```
   1111 1111 (255)
  +0000 0101 (5)
-----------
(1)0000 0100 (4)
```

Furthermore, integers can also underflow:
```cs
byte number = 0;
number--;
```

```
    0000 0000 (0)
   -0000 0001 (1)
-----------
(-1)1111 1111 (255)
```

You don't want this to happen in your games. Your players will be furious if their currency resets from 255 to 0 after collecting a coin ;)

## Signed Integers

### Idea: Sign Bit

We could use the first bit to represent whether it's a negative number or not. The remaining bits are interpreted as the number:

|   Byte  |Hexadecimal|Byte Value|Numeric Value|
|:-------:|:---------:|:--------:|:-----------:|
|0000 0000|    00     |     0    |      0      |
|0000 0001|    01     |     1    |      1      |
|0000 1111|    0F     |     15   |     15      |
|0111 1111|    7F     |    127   |     127     |
|1000 0000|    80     |    128   |     -0      |
|1000 0001|    81     |    129   |     -1      |
|1000 1111|    8F     |    143   |     -15     |
|1111 1111|    FF     |    255   |     -127    |

#### Problem 1: Negative Zero
`1000 0000` represents a negative zero. That number doesn't make any sense.

#### Problem 2: Addition doesn't work anymore

```
 0000 0100 (4)
+1000 0001 (-1)
---------------
 1000 0101 (-5)
```

### Two's Complement Notation
This solution solves both problems. To create a negative number, e.g. -4:
- you start with the positive number: `0000 0100 (4)`
- then, you invert the byte: `1111 1010 (251)`
- then, you add one: `1111 1011 (252)`

#### Negative Zero?
Let's see, what happens when we try to define negative zero:
- we start with zero: `0000 0000 (0)`
- then, we invert the byte: `1111 1111 (255)`
- then, we add one: `(1)0000 0000 (0)`
- so we end up with positive zero again, cool.

#### What about addition?
```
    0000 0100 (4)
   +1111 1111 (-1)
---------------
(-1)0000 0011 (3)
```

Other Types of Numbers:
<img width="359" alt="image" src="https://github.com/marczaku/xxx-computer-science/assets/7360266/aa7ac179-41bf-4f6f-8e6b-e999d0b1e32f">

## Rational Numbers
- How can we store rational numbers in binary?
- All numbers that can be written as $p\diva$
  - where `p` and `a` are whole numbers
  - (that's why it's called RATIOnal numbers)
- All rational numbers can be written either
  - as a finite fraction: $1\div2 = 0.5$
  - a period fraction: $1\div3 = 0.333...$
 
### Decimal Point
You all know, how the decimal point works. Similarly how each additional digit before the Decimal Point increases the power by one, each digit after the Decimal Point decreases it:
|Power|3|2|1|0|.|-1|-2|
|:---:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|Name |Thousand|Hundred|Ten|One|Dot|Tenth|Hundredth|
|Value|$10^3=1000$|$10^2=100$|$10^1=10$|$10^0=1$|.|$10^{-1}=0.1$|$10^{-2}=0.1$|
|Sample|1|3|3|7|.|2|5|

Value: 1337.25

### Binary Point
This works the same in the binary system:
|Power|3|2|1|0|.|-1|-2|
|:---:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|Name |Eight|Four|Two|One|Dot|Half|Quarter|
|Value|$2^3=8$|$2^2=4$|$2^1=2$|$2^0=1$|.|$2^{-1}=0.5$|$2^{-2}=0.25$|
|Sample|1|0|0|1|.|1|1|

Value:
- $1\mult2^{3} = $1\mult8 = 8
- $0\mult2^{2} = $0\mult4 = 0
- $0\mult2^{1} = $0\mult2 = 0
- $1\mult2^{0} = $1\mult1 = 1
- $1\mult2^{-1} = $1\mult0.5 = 0.5
- $1\mult2^{-2} = $1\mult0.25 = 0.25

Sum: 9.75

## Fixed-Point Numbers
In programming, we can express rational numbers by defining that a number has a point at a fixed bit.
- C# does not come with an implementation per default
- we will later see why
- but fixed point numbers have use cases in game programming
- because they behave deterministically

### Fixed-Point Unsigned Byte Definition
- The first 4 bits represent bits before the decimal point
- The remaining 4 bits represent bits after the decimal point

Let's try defining `5.7`:

|Power|3|2|1|0|.|-1|-2|-3|-4|
|:---:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|Name |Eight|Four|Two|One|Dot|Half|Quarter|Eigth|Sixteenth|
|Value|$2^3=8$|$2^2=4$|$2^1=2$|$2^0=1$|.|$2^{-1}=0.5$|$2^{-2}=0.25$||$2^{-3}=0.125$||$2^{-2}=0.0625$|
|Sample|0|1|0|1|.|1|0|1|1|

- $0\mult2^{3} = $1\mult8 = 0
- $1\mult2^{2} = $0\mult4 = 1
- $0\mult2^{1} = $0\mult2 = 0
- $1\mult2^{0} = $1\mult1 = 1
- $1\mult2^{-1} = $1\mult0.5 = 0.5
- $0\mult2^{-2} = $1\mult0.25 = 0
- $1\mult2^{-3} = $1\mult0.125 = 0.125
- $1\mult2^{-4} = $1\mult0.0625 = 0.0625

Sum: 5.66875

That's close to 5.7 but not exact.
- This is a problem we'll always have with fractional numbers in Programming.

What's the largest number representable by this byte?
- 15.9375

What's the precision of this data type (how accurate is it, what's the smallest step you can do?)
- $2^{-4} = 0.0625$

How can we improve the largest number representable?
- Move the Binary Point to the right (losing precision)
- Adding More Bytes

How can we improve the precision?
- Move the Binary Point to the left (reducing the largest representable number)
- Adding More Bytes

So either way, we would waste performance:
- for very small numbers like `0.0001255`, we waste bits before the point at a loss of precision
- for very large numbers like `91224` we waste bits after the point at a loss of how large the number can be

## Floating-Point Numbers
The solution to thi
