# Digital Information

## Why Binary?
More than anything, the binary system is the simplest numeric system imaginable.
- it works using only two digits: `0` and `1`

### Accuracy
We store digital information using physical, analogue methods e.g. magnetic charge or electric voltage.\
The problem: physical, analogue information is never 100% accurate.
- e.g. while trying to apply 2V voltage
- the measured value might fluctuate between 1.9V and 2.1V

Therefore, we use buffer zones. With a binary system, we can simply set up two easy rules:
- Voltage < 1V: OFF
- Voltage >= 2V: ON

And then we give 3V voltage anytime we want to send an ON signal.
<img width="300" alt="image" src="https://github.com/marczaku/xxx-computer-science/assets/7360266/bd36fe4e-fef8-46d3-9c64-ce9b84b3e5e5">

### Simplicity
Without any Logic Gates, computers would just be very inconvenient cables.\
Logic Gates allow the computer to combine information and create new information from it.
- e.g. If left mouse button is clicked AND the cursor position is over button X THEN ...

On the lowest level, such Logic Gates combine information 1 Bit at a Time. Some examples:

|Input A|Input B|A AND B|A OR B|
|:-----:|:-----:|:-----:|:----:|
|0|0|0|0|
|1|0|0|1|
|0|1|0|1|
|1|1|1|1|

Such Logic Gates come in various forms. The number of possible inputs for `A` and `B` is $2^2 = 4$:
- `00`, `01`, `10`, `11`

The number of possible outputs is either ON or OFF for each possible input, so: $2^{2^2} = 16$. Among those:
- `A AND B` is only on if the input is `11`: `0001`
- `A OR B` is on if any bit is `1`: `0111`
- `A NAND B` is on if no bit is `1`: `1000`
- ...

However, a Ternary Logic Gate would have $3^2 = 9$ possibe inputs:
- `00`, `01`, `02`, `10`, `11`, `12`, `20`, `21`, `22`

And then has three possible putputs 0,1,2 for each possible input, so: $3^{3^2} = 19.683$ possible outcomes.
- That escalated quickly.

### Versatility
In fact, we just don't need more than two states. Because we can just use more bits to express more complex states.\
- e.g. to represent the four seasons, we can simply use two bits:
  - `00`, `01`, `10`, `11`
  
To represent the 16 months, we can use 4 bits:

|Month|Bits|
|:---:|:--:|
|January|0000|
|February|0001|
|March|0010|
|April|0011|
|May|0100|
|June|0101|
|July|0110|
|August|0111|
|September|1000|
|October|1001|
|November|1010|
|December|1011|

- we even have space for 4 more months if ever needed: `1100`, `1101`, `1110`, `1111`

In fact, in 1936, Alan Turing deducted that any type of information can be stored using Binary Numbers.\
And so far, he's been right. Nowadays, we store various types of data, including:
- numbers
- text
- photos
- websites
- movies

## Bit
The term Bit is just short for Binary Digit.

## Byte
A Byte is a group of bits.\
- like how 10mm = 1cm.\

### 8 Bits = 1 Byte
One byte is the number of bits stored to store a single text character. e.g. `a`\
Therefore, it's also been used as the smallest addressible unit of a computer.\
Historically, the size has been platform-dependent and ranged from 1-48.\
It was standardized in the 60s to 6 bits, because that's as many bits as an ASCII character took.\
In 1993, 8 bits became an ISO-certified standard and has been since then.

## KByte and More
- 1024 Byte = 1 Kilo Byte
- 1024 Kilo Byte = 1 Mega Byte
- 1024 Mega Byte = 1 Giga Byte
- 1024 Giga Byte = 1 Tera Byte

## Hexadecimal System
The Hexadecimal is a 16-based system.
- It has 16 digits: $0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F$
- We use letters for the extra digits that our decimal system doesn't have.

|Hexadecimal Digit|0|1|2|3|4|5|6|7|8|9|A|B|C|D|E|F|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
Decimal Value|0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|

### Byte Representation
We use the Hexadecimal mainly to represent the value of 1 Byte.

#### 1 Hex Digit = 4 Bits
One Hexadecimal digit can be perfectly used to represent 4 Bits.
|Hexadecimal Digit|0|1|2|3|4|5|6|7|8|9|A|B|C|D|E|F|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
Bits|0000|0001|0010|0011|0100|0101|0110|0111|1000|1001|1010|1011|1100|1101|1110|11111|

#### 2 Hex Digits = 8 Bits = 1 Byte
Therefore, two hexadecimal digits can perfectly represent 8 bits:
|Hexadecimal Digits|Binary Digits|Decimal Value|
|:----------------:|:-----------:|:-----------:|
|        00        |   00000000  |      0      |
|        01        |   00000001  |      1      |
|        02        |   00000010  |      2      |
|        03        |   00000011  |      3      |
|        0F        |   00001111  |     15      |
|        10        |   00010000  |     16      |
|        11        |   00010001  |     17      |
|        12        |   00010010  |     18      |
|        13        |   00010011  |     19      |
|        20        |   00100000  |     32      |
|        40        |   01000000  |     64      |
|        79        |   01111111  |    127      |
|        80        |   10000000  |    128      |
|        FF        |   11111111  |    255      |

### Use Cases
This is so useful that you should get used to using Hexadecimal numbers.\

- Hackers and Debuggers to analyze the information stored in RAM or Binaries.
- Runtimes to print Memory Addresses / Pointer Values
- Apps to encode colors
  - RGBA8 is a format where there is each 1 byte for Red, Green, Blue and Alpha Channel.
  - You can express for example Visible Yellow Color as: `#FFFF00FF`
    - Red:   `255` or `11111111`, so full power
    - Green: `255` or `11111111`, so full power
    - Blue: `0` or `00000000`, so no blue
    - Alpha: `255` or `11111111`, so no transparency


## Random Access Memory

### Addresses
- All information stored on our computers Runtime Memory is stored at addresses.
- Those Addresses have typically either 32 or 64 bit nowadays
- If you have 4GB RAM and 32bit Addresses
  - Then, the first Byte has the address #0 (or hexadecimal 0x00000000)
  - And the last Byte has the address #4294967296 (or hexadecimal 0xFFFFFFFF)
 
At each of these addresses, you find one byte of information:

Therefore, two hexadecimal digits can perfectly represent 8 bits:

### Byte-Data
|Address|Byte-Data|Hex-Representation|
|:-----------------:|:-----------:|:---:|
|         00        |   00000000  |00|
|         01        |   00000001  |01|
|         02        |   00000011  |03|
|         03        |   00000100  |04|
|         04        |   00000100  |04|
|         05        |   00000101  |05|
|         06        |   00000110  |06|
|         07        |   00000110  |06|
|         08        |   00000111  |07|
|         09        |   00000111  |07|
|         10        |   11111111  |FF|
|         11        |   00000000  |00|
|         12        |   00000000  |00|
|         13        |   11111111  |FF|

So, what can we see here?

Actually, we can't know. Look for example at Address #01:
- `00000001` or `0x01`
  
Depending on what data there is at that address, it could stand for:
- `bool`: `True`
- `int`: `1`
- `char`: `'a'` (it wouldn't really be `'a'`, but for the sake of simplicity we'll pretend)
- `RGBA8`: a very, very light red value (almost unnoticable)

So, how do we know, which one it is? Only the program that put the data there, knows.

```cs
static void Main() {
  bool isRunning = true; // placed at address 01
  byte a = 3;            // placed at address 02
  byte b = 4;            // placed at address 03
}
```

Now, when you use those variables in your code, then the compiler will know, what type of value to find at that address:
```cs
  // compiler knows that byte a is at address 02
  // and that byte b is at address 03
  Console.WriteLine(a+b); // looks up value at address 02 and adds it to value at address 03
```
