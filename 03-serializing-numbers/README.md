# Serializing Numbers
In this chapter, we'll learn about how numbers are serialized on our CPU.

This will enable you to always pick the right type in order to:
- avoid wasting memory (RAM or on the Hard Drive)
- avoid number overflows (which can cause e.g. the player's health to drop from 255 to 0 instead of going 256)
- avoid bugs caused by a lack of precision

## Types of Numbers

<img width="359" alt="image" src="https://github.com/marczaku/xxx-computer-science/assets/7360266/aa7ac179-41bf-4f6f-8e6b-e999d0b1e32f">

## Unsigned Integers
For unsigned integers, the obvious solution is to make the byte's numeric value equal to the value of the number, e.g.:

|   Byte  |Hexadecimal|Byte Value|Numeric Value|
|:-------:|:---------:|:--------:|:-----------:|
|`0000 0000`|    `00`     |     $0$    |     $0$     |
|`0000 0001`|    `01`     |     $1$    |      $1$      |
|`0000 1111`|    `0F`     |     $15$   |     $15$      |
|`1111 1111`|    `FF`     |    $255$   |     $255$     |

This is how the C# Type `byte` and the C++ Type `char` work.

### Larger Numbers
If we want to store larger numbers, we just use additional bytes. In this case Little-Endian:

|       Bytes       |Hexadecimal|Bytes Value|Numeric Value|
|:-----------------:|:---------:|:---------:|:-----------:|
|`0000 0000 0000 0000`|   `00 00`   |      $0$    |      $0$      |
|`0000 0001 0000 0000`|   `01 00`   |      $1$    |      $1$      |
|`0000 1111 0000 0000`|   `0F 00`   |      $15$   |     $15$      |
|`1111 1111 0000 0000`|   `FF 00`   |     $255$   |     $255$     |
|`0000 0000 0000 0001`|   `00 01`   |     $256$   |     $256$     |
|`0000 0011 0000 0001`|   `03 02`   |     $515$   |     $515$     |
|`1111 1111 1111 1111`|   `FF FF`   |  $65,535$   |   $65,535$    |

This is how the C# Type `ushort` and the C++ Type `uint16_t` work.

### Overview of Unsigned Integer Types

|Byte Size|C# Type|C++ Type|Max Value|
|:-------:|:-----:|:------:|:-------:|
|1 byte|`byte`|`uint8_t`|$255$|
|2 bytes|`ushort`|`uint16_t`|$65,535$|
|4 bytes|`uint`|`uint32_t`|$4,294,967,295$|
|8 bytes|`ulong`|`uint64_t`|$18,446,744,073,709,551,615$|

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
|`0000 0000`|    `00`     |     $0$    |      $0$      |
|`0000 0001`|    `01`     |     $1$    |      $1$      |
|`0000 1111`|    `0F`     |     $15$   |     $15$      |
|`0111 1111`|    `7F`     |    $127$   |     $127$     |
|`1000 0000`|    `80`     |    $128$   |     $-0$      |
|`1000 0001`|    `81`     |    $129$   |     $-1$      |
|`1000 1111`|    `8F`     |    $143$   |     $-15$     |
|`1111 1111`|    `FF`     |    $255$   |     $-127$    |

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

### Overview of Signed Integers

|Byte Size|C# Type|C++ Type|Min Value|Max Value|
|:-------:|:-----:|:------:|:-------:|:-------:|
|1 byte|`sbyte`|`int8_t`|$-128$|$127$|
|2 bytes|`short`|`int16_t`|$-32,768$|$32,767$|
|4 bytes|`int`|`int32_t`|$-2,147,483,648$|$2,147,483,647$|
|8 bytes|`long`|`int64_t`|$-9,223,372,036,854,775,808$|$9,223,372,036,854,775,807$|


## Rational Numbers
- How can we store rational numbers in binary?
- All numbers that can be written as $\frac{p}{a}$
  - where `p` and `a` are whole numbers
  - (that's why it's called RATIOnal numbers)
- All rational numbers can be written either
  - as a finite fraction: $\frac{1}{2} = 0.5$
  - a period fraction: $\frac{1}{3} = 0.333...$
 
### Decimal Point
You all know, how the decimal point works. Similarly how each additional digit before the Decimal Point increases the power by one, each digit after the Decimal Point decreases it:
|Power|3|2|1|0|.|-1|-2|
|:---:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
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
- $1\times2^{3} = 1\times8 = 8$
- $0\times2^{2} = 0\times4 = 0$
- $0\times2^{1} = 0\times2 = 0$
- $1\times2^{0} = 1\times1 = 1$
- $1\times2^{-1} = 1\times0.5 = 0.5$
- $1\times2^{-2} = 1\times0.25 = 0.25$

Sum: 9.75

## Fixed-Point Numbers
In programming, we can express rational numbers by defining that a number has a point at a fixed bit.
- C# does not come with an implementation per default
- we will later see why
- but fixed-point numbers have use cases in game programming
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

- $0\times2^{3} = 1\times8 = 0$
- $1\times2^{2} = 0\times4 = 1$
- $0\times2^{1} = 0\times2 = 0$
- $1\times2^{0} = 1\times1 = 1$
- $1\times2^{-1} = 1\times0.5 = 0.5$
- $0\times2^{-2} = 1\times0.25 = 0$
- $1\times2^{-3} = 1\times0.125 = 0.125$
- $1\times2^{-4} = 1\times0.0625 = 0.0625$

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
The solution to this problem is Floating-Point Numbers, which means Numbers where the point can be moved.
- if you have numbers close to zero, then you can use almost all digits for the precision after the comma.
- if you have very large numbers, then all digits are used to represent that number at the cost of precision.

### IEEE Standard 754 Float

<img width="476" alt="image" src="https://github.com/marczaku/xxx-computer-science/assets/7360266/535fb988-9d1c-42b7-9021-d4a4cf9482dd">

- inspired by the scientific notation: `1.25E+25`
- it uses 4 bytes (32 bit)
- The first bit is used for the sign.
- The next 8 bits are used for the power of 2 to multiply the number with.
  - offset by the bias (+127) to allow for negative powers as well.
  - this allows the fractional number to be
    - multiplied with up to $2^{127} = 1.7014e38$
    - multiplied with up to $2^{-127} = 5.8774e-39$
 - The remaining 23 bits are used for the fractional part of the number 1.xxxxxx

### An Example
- 100,000.625

Convert Integer Part to Binary:
- $2^{16} = 65,536$ - Remainder: $34,464$
- $2^{15} = 32768$ - Remainder: $1696$
- $2^{10} = 1024$ - Remainder: $672$
- $2^9 = 512$ - Remainder: $160$
- $2^7 = 128$ - Remainder: $32$
- $2^5 = 32$ - Remainder: $0$

Result: $11000011010100000$

Convert Fractional Part to Binary:
- $2^{-1}$ = 0.5$ - Remainder: $0.125$
- $2^{-3}$ = 0.125$ - Remainder: $0$

Result: $101

Combine:
- $11000011010100000.101$

Normalize:
- - $1.1000011010100000101\times{2^16}$
 
Encode the IEEE 754 Format:
- The sign bit is $0$ (positive number)
- The exponent is $16 + 127 (bias) = 143 = 10001111 (binary)$
- The fraction is `10000110101000001010000` (truncated to 23 bits)

Putting it all together:
- `0 10001111 10000110101000001010000`

### Inaccuracy
Floating-Point Numbers are still as unprecise as Fixed-Points:
- $0.7$ in float is:
- $1.399999976158142\times2{-1}$
- Which is actually $0.699999988079071044921875$

### Size and Accuracy
The larger Floating Point Numbers are the more inaccurate they become:

```cs
cw(10_001_000f - 10_000_000f); // Result: 1000
cw(1_000_001_000f - 1_000_000_000f); // Result: 1024
cw(100_000_001_000f - 100_000_000_000f); // Result: 0
```

|Exponent|Range|Accuracy|
|:------:|:---:|:---:|
|0|[1,2[|0.00000011920929|
|1|[2,4[|0.000000238418579|
|2|[4,8[|0.000000476837158|
|9|[512,1024[|0.00006103515|
|10|[1024,2048[|0.00012207031|
|19|[524288,1048576[|0.0625|
|23|[8388608,16777216[|1|

In other words: For numbers larger than $8,388,608$, `float` can only represent integers.

### Double-Precision
Quick Fix: Use type `double` instead of `float`

- It uses 8-bytes (64 bit)
- The first bit is used for the sign.
- The next 11 bits are used for the power of 2 to multiply the number with.
  - offset by the bias (+1023) to allow for negative powers as well.
  - this allows the fractional number to be
    - multiplied with up to $2^{1023} = 8.9884e+307$
    - multiplied with up to $2^{-1023} = 1.1125e-308$
 - The remaining 52 bits are used for the portion of the fractional number 1.xxxxx

```cs
cw(10_001_000d - 10_000_000d); // Result: 1000
cw(1_000_001_000d - 1_000_000_000d); // Result: 1000
cw(100_000_001_000d - 100_000_000_000d); // Result: 1000
```

### Overview of Floating Point Numbers

|Byte Size|C# Type|C++ Type|Min Value|Max Value|
|:-------:|:-----:|:------:|:-------:|:-------:|
|4 bytes|`float`|`float`|$~1.5\times10^{-45}$|$~3.4\times10^{38}$|
|8 bytes|`double`|`double`|$~5.0\times10^{-324}$|$~1.7\times10^{308}$
|16 bytes|`decimal`|N/A|$-79,228,162,514,264,337,593,543,950,335$|$79,228,162,514,264,337,593,543,950,335$|

## Arbitrary-Precision Integers
Another noteworthy mention is C#'s `BigInteger`. They automatically scale an array of `uint` to represent numbers:
```cs
public struct BigInteger {
 internal int _sign;
 internal unit[] _bits;
}
```

This way, this Integer can scale endlessly. At the cost of performance and memory overhead. Very useful for game mechanics where you expect numbers to scale endlessly and exponentially. Like Idle Games.
