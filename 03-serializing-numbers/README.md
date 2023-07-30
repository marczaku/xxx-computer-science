# Serializing Numbers
In this chapter, we'll learn about how numbers are serialized on our CPU.

This will enable you to always pick the right type in order to:
- avoid wasting memory (RAM or on the Hard Drive)
- avoid number overflows (which can cause e.g. the player's health to drop from 255 to 0 instead of going 256)

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
