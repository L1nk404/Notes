## How CPU Loads Number in Memory
Take the following flowchart as example:
![[Number_Asm_dark.drawio.png]]
The process is:
1. The user inputs a decimal value: **123456**
2. The CPU converts from decimal to hexadecimal: **0x bc 61 4e**
3. The hexadecimal value is stored in memory in reverse order: **0x 4e 61 bc**

## Numeric Data Type

| Data Type | Size              | Range (Signed)                      | Range (Unsigned)               |
| --------- | ----------------- | ----------------------------------- | ------------------------------ |
| `.byte`   | 1 byte (8 bits)   | $-128$ to $127$                     | $0$ to $255$                   |
| `.word`   | 2 bytes (16 bits) | $-32.768$ to $32.767$               | $0$ to $65.535$                |
| `.int`    | 4 bytes (32 bits) | $-2.147.483.648$ to $2.147.483.647$ | $0$ to $4.294.967.295$         |
| `.long`   | 4 bytes (32 bits) | Same as `.int`                      | Same as `.int`                 |
| `.quad`   | 8 bytes (64 bits) | $-2^{63}$ to $2^{63} - 1$           | $0$ to $2^{64} - 1$            |
| `.short`  | 2 bytes (16 bits) | Same as `.word`                     | Same as `.word`                |
| `.float`  | 4 bytes (32 bits) | Single-precision float              | Single-precision float         |
| `.double` | 8 bytes (64 bits) | Double-precision float              | Double-precision float         |
| `.ascii`  | Variable          | ASCII text, no null terminator      | ASCII text, no null terminator |
| `.asciz`  | Variable          | ASCII text, null-terminated         | ASCII text, null-terminated    |

### Integers
#### Signed
In a short way, signed integers are integers which has sign. A **signed** integer can represent both **negative** and **positive** values. In the common **two’s complement** representation (used in x86 processors), the most significant bit (MSB) is used to indicate the **sign** of the number
###### Range:
The range for signed numbers is  $[-2^{n-1}, 2^{n-1} - 1]$,  where $n$ is the number of bits.
So, in 32 bits system, $n=32$, the range is $[−2.147.483.648 , 2.147.483.647]$.
#### Unsigned
An **unsigned** integer can only represent **non-negative** values (i.e., zero and positive numbers). All bits in an unsigned integer are used to represent the magnitude of the number.
###### Range:
The range of unsigned integers are $[0, 2^n - 1]$. In 32 bit system, $n=32$, so, $[0, 4.294.967.295]$.
#### Practical Example
```nasm
.section .data
	signed_value: 		    .int -10 	# Example of a signed integer for comparison
	unsigned_value: 	    .int 10  	# Example of an unsigned integer

	result_signed:		    .int 0		# Placeholder for signed result
    	result_unsigned:	.int 0 		# Placeholder for unsigned result

.section .text
.globl _start

_start:
	# Load signed integer into EAX
	movl 	signed_int, %eax	# EAX now hold -10
	addl 	$5, %eax 		    # add 5 to EAX ( -10 + 5 = -5)
	movl 	%eax, result_signed	# Store result in result_signed

	# Load unsigned integer into EAX
	movl 	unsigned_int, %eax	    # EAX now hold 10 (unsigned)
	addl	$5, %eax		        # add 5 to EAX ( 10 + 5 = 15)
	movl 	%eax, result_unsigned	# Store result in result_unsigned

exit:
	movl	$1, %eax
	movl	$0, %ebx
	int	$0x80
```

#### Converting ASCII <=> Integers
Converting a string to an integer and vice versa in Assembly revolves around understanding how characters (in ASCII format) map to their numeric values and performing arithmetic or format operations accordingly.
You can check the ASCII table that will be useful [here](https://www.ascii-code.com)
##### ASCII => Integer
###### Understanding ASCII Representation:
- Each digit character ($0$ to $9$) is stored as an ASCII value.
- ASCII value of $0$ is $48_d$ (or `0x30` in hex). (**To avoid misunderstanding, let denote the ASCII value of 0 by $0_A$**)
- To get the numeric value of a digit character, subtract $0$ ($48_d$) from its ASCII value.
You may ask why subtracting  the ASCII value from $48_d$ ($0$ in ASCII table) works. The point is, we are just doing an arithmetic operation involving two integers ($48$ and ($48+n$) where $n \in [0,9]$ ) and it will return a integer value. Take the number $3$ as example. So in the ASCII table, $3$ have the code $51_d$ ($48 + 3$) .
Subtracting $51$ from $48$ we have $3$, an integer number!
###### Algorithm:
- Start with an initial value of $0$ in a register to accumulate the result.
- For each character in the string:
	- Subtract $0_A$ to get the numeric value of the character.
	- Multiply the current accumulated result by $10$.
	- Add the numeric value to the accumulated result.
###### Example:
```nasm
.section .data
    str:    .asciz "1234"           # Null-terminated string
    result: .long 0                 # To store the final integer result

.section .text
    .globl _start
    
_start:
    # Initialize variables
    movl $0, %eax                  # EAX = 0 (accumulator for the integer result)
    movl $str, %esi                # ESI points to the start of the string

convert_loop:
    movzbl (%esi), %ebx            # Load the current byte into EBX (zero-extended)
    cmpb $0, %bl                   # Check if it's the null terminator
    je end_conversion              # If null terminator, jump to the end

    subb $'0', %bl                 # Convert ASCII character to integer
    imull $10, %eax, %eax          # Multiply current result by 10
    addl %ebx, %eax                # Add the numeric value to EAX

    incl %esi                      # Move to the next character
    jmp convert_loop               # Repeat the loop

end_conversion:
    movl %eax, result              # Store the result in memory

    # Exit syscall
    movl $60, %eax                 # syscall: exit
    xorl %edi, %edi                # status: 0
    syscall
```
##### Integer => ASCII
To convert an integer ($1234$) to its string representation ("1234"), reverse the logic of string-to-integer conversion:
###### Understand the Modulo Operation:
- Extract the least significant (LSD) of the number using the modulus operation (`mod 10`).
- Convert the digit into its ASCII representation by adding $0$ ($48_d$).
###### Algorithm:
- Repeat until quotient becomes 0:
	1. Get the LSD of the number (`number % 10`).
	2. Convert it to ASCII by adding $0$ ($48_d$).
	3. Store the character (in reverse order)
- After all digits are processed, reverse the order of the stored characters to get the final string.
###### Example:
```nasm 
.section .data
    result: .space 16               # Buffer for the resulting string (null-terminated)
    integer: .long 1234             # Example integer to convert

.section .text
    .globl _start
    
_start:
    movl integer, %eax              # Load the integer into EAX
    leal result+15, %edi            # Start writing from the end of the buffer
    movb $0, (%edi)                 # Null-terminate the string
    decl %edi                       # Move back to the first free position

convert_to_string:
    xorl %edx, %edx                 # Clear EDX (for division remainder)
    movl $10, %ebx                  # Divisor = 10
    divl %ebx                       # Divide EAX by 10: quotient in EAX, remainder in EDX

    addb $'0', %dl                  # Convert remainder to ASCII
    movb %dl, (%edi)                # Store ASCII character in the buffer
    decl %edi                       # Move to the previous position

    testl %eax, %eax                # Check if quotient is 0
    jnz convert_to_string           # If not, repeat

    # Exit syscall
    movl $60, %eax                  # syscall: exit
    xorl %edi, %edi                 # status: 0
    syscall
```
### SIMD Integers
**SIMD** (Single Instruction Multiple Data) is a technique used in computer architecture that allows a single instruction to perform the same operation on multiple pieces of data simultaneously. **SIMD integer operations** specifically apply SIMD techniques to **integer data types**.
##### What is SIMD
In SIMD, multiple data elements are **packed** into a single **wide register**, and operations are performed on these elements **in parallel**. This parallelism can lead to significant performance improvements in tasks that require repetitive calculations on arrays or vectors of data, such as image processing, audio processing, scientific simulations, and more.
#### SIMD Registers in x86 Architecture
Different than the general purposes registers that holds only 32 bits of data. It holds instruction section such:
- **MMX** (Multimedia Extension) - holds 64 bits registers
- **SSE** (Streaming SIMD Extension) - 128 bits 
##### MMX Registers
The MMX are a instruction section that holds 8 64 bits registers, named from **mm0** to **mm7**. These registers are used to store integer data.

Each MMX register can hold:
- **Eight 8-bit integers** (bytes), or
- **Four 16-bit integers** (words), or
- **Two 32-bit integers** (doublewords).

So, in a nutshell, we have
- **Primarily Function**: Enable integer-based SIMD (Single Instruction, Multiple Data) operations for multimedia tasks, such as image processing, audio processing, and basic 2D graphics.
- **Data Types**: 8-bit, 16-bit, and 32-bit integers (packed within each 64-bit MMX register).
- **Key Focus**: Integer arithmetic on multiple small data elements in parallel.
###### Syntax
To move data to the MMX register we use the letter "q" on the _mov_ operator, _movq_ is used to move 64-bit data.

```nasm
.section .data
	value1:	.int	1,2   # 32 bit size

.section .text
.globl _start
_start:
	nop
	
	# mov value1 into one of the MMX registers
	movq 	value1, %mm0
	
	# exit
	movl 	$1, %eax
	movl 	$0, %ebx
	int	    $0x80
```

Using the debugger we can see the _mm0_ register before the value be store:

![[Pasted image 20241110173601.png]]

In the _uint64_, the _u_ stands for unsigned, the _64_ stands for 64 bits, It shows the data stored in this 64 bit unsigned register.
The _v2_ means that the 64 bits register are divided into two 32 bits parts, and how the value stored on each one.
After execute the movq instruction, we can see how the data was stored:

![[Pasted image 20241110174003.png]]

So we can notice that the first element (1) was stored in the most right hand side, and the second element (2) was stored on the top left.
Now look to the _v4\_int16_, notice that there is a 0x0 stored between 0x1 and 0x2. The reason for that is because our data is 32 bit, so the 0x0 is alocated for the 32 bits number.
##### SSE Registers
Like the MMX instruction, the SSE is a collection of  8 register, but instead of 64 bits, they have 128-bits size, named from **xmm0** to **xmm7**.

Each **SSE register** can hold:
- **Sixteen 8-bit integers** (bytes), or
- **Eight 16-bit integers** (words), or
- **Four 32-bit integers** (doublewords), or
- **Two 64-bit integers** (quadwords), or
- **Four 32-bit single-precision floats**, or
- **Two 64-bit double-precision floats**

So, in a nutshell, we have
- **Primarily Function**: Extend SIMD capabilities to support **floating-point arithmetic** in addition to integer operations, making them versatile for scientific and multimedia applications.
- **Data Types**: 32-bit single-precision and 64-bit double-precision floating-point numbers, as well as 8, 16, and 32-bit integers (packed within each 128-bit XMM register).
- **Key Focus**: High-performance floating-point calculations, as well as integer SIMD operations, across multiple data elements.
###### Syntax
To move data to the SSE registes, we use the operator _mvdqa_ where _dqa_ stands for **double quadword aligned**.

```nasm
.section .data
	value1: .int 	1,2,3,4            # int have 32-bit size
	value2: .byte 	1,2,3,4,5,7,8      # byte have 8-bit size

.section .text
.globl _start
_start:
	nop
	
	movdqa    value1, %xmm0
	movdqa	  value2, %xmm1

	# exit
	movl	  $1, %eax
	movl 	  $0, %ebx
	int 	  $0x80
```

### Binary Coded Decimal

Binary Coded Decimal (**BCD**) is a binary enconding method for decimal numbers where each decimal digit is represented by a fixed number of binary bits, usually four. In BCD, decimal digits 0 through 9 are encoded individually, meaning each digit in the decimal number is separately converted to its four-bit binary equivalent.
For example, the decimal number $47$ in BCD is represented as $0100\ 0111$ (where
$4_{d}=0100_{b}$ e $7_{d}=0111{b}$). So, 47 well allocate 1 byte of memory, and we can represent it like:

| 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 1   | 0   | 0   | 0   | 1   | 1   | 1   |
##### Pros
- BCD allows for easier manipulation of decimal numbers in digital systems, especially in applications like calculator and digital displays, **where precise decimal representation is critical**.
- BCD avoids rounding error that can occur with floating-point binary representation in some cases.
##### Cons
 - BCD is less space-efficient than pure binary, as it uses more bits to represent the same decimal value.
 - Arithmetic operations on BCD are generally slower because they require extra processing to handle the decimal digit constraints.
#### Storing BCD Data
We can only store data in the _FPU_ (Floating Point Unite) registers named from _st0_ to _st7_. The size of each _st_ register is 80 bit, but we can use only 72 bits where the last one is used for signed operations to store BCD data, the another 10 bits are used by the system.
##### Syntax
To BCD data we only need to use the _fbld_ (Floating-Point Binary Load) instruction and the source `fbld   <source>`.

**Loading and Storing BCD Values:**
- `fbld <source>` – Load BCD value from memory into `ST(0)` (convert to float)
- `fbstp <destination>` – Convert `ST(0)` to BCD and store in memory, then pop

```nasm
.section  .data
	bcd_data: 	
		.byte	0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8, 0x9, 0x5   # 10 bytes

.section .text
.globl _start
_start:
	nop
	
	fbld 	bcd_data	 # it will store bcd_data into st0 register

	# exit
	movl 	$1, %eax
	movl	$0, %ebx
	int 	$0x80
```

bcd_data have 10-bytes size, as we see above, the _st_ registers can hold only 9 bytes. What will happens?

![[Pasted image 20241114211833.png]]

On the debugger, you can see that only 9 bytes was stored, the 10th value (`0x5`)  was not stored.
### Floating Point Number

A **floating-point number** is a data type used to represent real numbers with fractional parts, allowing for a wide range of values. It consists of three main components:

1. **Sign bit**: Indicates whether the number is positive or negative.
2. **Exponent**: Determines the scale or "magnitude" of the number.
3. **Mantissa (or significand)**: Represents the actual digits of the number.

Floating-point numbers use scientific notation, where a number is represented (in binary) as:
$$
sing\times mantissa \times 2^{exponent}
$$
##### Floating-Point Standards:

Most systems use the **IEEE 754 standard** for floating-point representation, which defines:
- **Single precision** (32 bits): Offers approximately 7 decimal digits of precision.
- **Double precision** (64 bits): Offers approximately 15 decimal digits of precision.
##### Floating Point Range and Precision:

Floating-point numbers are suited for very large or very small values, unlike integers, which have fixed ranges. However, they can suffer from **rounding errors** due to limited precision, especially when representing numbers with many decimal places.

**Example:** For single precision (32-bit):
- Sign bit: 1 bit
- Exponent: 8 bits
- Mantissa: 23 bits
##### Pros:
- Allows for representation of a vast range of values, including very small fractions and very large numbers.
- Well-suited for scientific calculations and graphics.
##### Cons:
- Limited precision can lead to rounding errors.
- Slower to process than integers on many CPUs.
##### Syntax
Like BCD, we use the _FPU_ registers. The instructions that we can use to manipulate floating point numbers is:

**Loading Floating-Point Values:**
- `flds <source>` – Load 32-bit single-precision float into `ST(0)`
- `fldl <source>` – Load 64-bit double-precision float into `ST(0)`
- `fld <source>`   – Load 80-bit extended-precision float into `ST(0)`
**Storing Floating-Point Values:**
- `fsts <destination>` – Store 32-bit single-precision float from `ST(0)`
- `fstl <destination>` – Store 64-bit double-precision float from `ST(0)`
- `fstp <destination>` – Store and pop `ST(0)`
**Integer Conversion:**
- `fild <source>` – Load integer into `ST(0)` (convert to float)
- `fistp <destination>` – Convert `ST(0)` to integer and store, then pop

We can also store multiple floating points in _xmm_ registers. For that, we use the instruction _movups_ indicating the source and destination (SSE register).

Let's see the following example:

```nasm
.section .data
    myfloat1:                                # Define a 4-byte (single-precision) floating-point number 1.23
        .float      1.23            
    myfloat2:                                # Define an 8-byte (double-precision) floating-point number 1234.5432
        .double     1234.5432       
	multifloat:                              # Define multiple 4-bytes floating-point numbers
        .float 		1.2, 3.5, 77.45, 11.06
.section .bss
    .lcomm data, 8                           # Reserve 8 bytes in uninitialized memory (common block)

.section .text
.globl _start                                # Declare _start as the entry point for the program
_start:
    nop                                      # Placeholder for alignment or debugging (no operation)

    # Load single-precision float (myfloat1) into FPU stack
    flds    myfloat1                         # Load single-precision (4 bytes) floating-point value into ST(0)
    # Load double-precision float (myfloat2) into top of FPU stack, myfloat1 will be passed to ST(1)
    fldl    myfloat2                         # Load double-precision (8 bytes) floating-point value into ST(0)
    # Store the top of the FPU stack (ST(0) = myfloat2) into memory (data)
    fstpl   data                             # Store and pop the double-precision value from ST(0) into memory
    
    # Load mulitple 4-bytes floating-point numbers at xmm0
    movups	multifloat,	%xmm0				 

    # Exit syscall
    movl    $1, %eax                         # Load syscall number for exit (1) into EAX
    movl    $0, %ebx                         # Clear EBX (set to 0) for exit status
    int     $0x80                            # Trigger the syscall using interrupt 0x80
```

