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
