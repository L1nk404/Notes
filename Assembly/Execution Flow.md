### Jump instruction
We can use the jump instruction to jump to any part of the code, the instruction syntax is _jmp_, we can use as parameter the address memory, or a label (similar to function in high-level language):
- `jmp <address_memory>
- `jmp <label>

Let's see a example:

```nasm
.section .text
.globl _start

_start:
	nop

	movl 	$1, %eax
	movl	$2, %ebx

	jmp  	move_data   # Here we'll jump to the move_data label

	# that part of code will not be executed after a jump to move_data
	movl 	$150, %eax
	movl 	$250, %ebx

jmp_back: 		# The code will be executed from here
	
	# exit
	movl	$1, %eax
	movl	$0, %ebx
	int	$0x80
	
move_data: 	 	# another label (similar to function)
	
	movl 	$100, %eax		
	movl 	$200, %ebx
	
	jmp	jmp_back
```

In the code above the the execution flow is:
$$
	\_start \rightarrow move\_data \rightarrow jmp\_back
$$

> [!warning] Note
> Is important to note that when we  jump to some local, we **don't** return back when that label ends, so, if we want to return, we must jump back to the place where we are just after the _jmp_ instruction

### Call Instruction
Different to _jmp_ instruction, _call_ instruction back to the last location after the label called by _call_, we just need to use _ret_ (return) instruction to it.
In the example above, we can save some lines:

```nasm
.section .text
.globl _start

_start:
	nop

	movl 	$1, %eax
	movl	$2, %ebx

	call  	move_data	# Here we'll call the move_data label

	# exit 
	movl 	$150, %eax
	movl 	$250, %ebx

move_data: 	 	# another label (similar to function)
	
	movl 	$100, %eax		
	movl 	$200, %ebx	

	ret			# return to the instruction right after call move_data <_start> 
```

So, the execution flow will be
$$
	\_start \rightarrow move\_data \rightarrow \_start
$$

The call instruction save the memory address on the stack of the instruction right after call. So when the _ret_ is triggered, it jmp back to that memory address.
We can see it using gdb debugger:

![[Pasted image 20241029213611.png]]

Note that the memory address of the next instruction before call on \_start is  saved on the stack.
After _ret_ instruction executed, the memory are excluded from 
stack frame.


---
## Flags Register
In computing, especially in processor architectures like x86, a  [flag register](https://en.wikipedia.org/wiki/FLAGS_register) or simply _flag_ is a single bit in a special register that indicates a specific state or the result of an operation. In the case of x86 processors, these bits are contained in a register called **eflags** (or just **FLAGS** in older versions of x86).
### What is a Flag?
A _flag_ is a **status indicator** that generally reflects the result of an arithmetic, logical, or control operation. These indicators are useful because they inform the processor (and consequently the programmer) of what happened during a given instruction.
#### Common flags of x86 processors:
- **Carry Flag(CF)** 
	- Overflow of unsigned integers => CF = 1
- **Overflow Flag (OF)**
	- Overflow/underflow of signed integers => OF = 1
- **Parity Flag (PF)**
	- If the number of digit 1 on a $result$ is:
		- odd => PF = 0
		- even => PF = 1
- **Sign Flag (SF)**
	- if $result > 0$ => SF = 0
	- If $result < 0$ => SF = 1
- **Zero Flag (ZF)**
	- if $result \neq 0$ => SF = 0
	- if $result = 0$ => SF = 1
### Carry Flag
A **Carry Flag** (or _CF_) is one of the most important flags in the x86 processor, mainly used for arithmetic operations. It is part of the _EFLAGS_ (or _FLAGS_ in older processors), which is a 32-bit register that contains various status flags.
#### Function of the Carry Flag
The _carry flag_ is used to indicate that an _unsigned integer overflow_ occurred in a specific arithmetic calculation. Essentially, when performing an operation that results in a value larger than the available space, this flag is set to _1_. Otherwise, it is set to _0_.
For example:
	If an instruction has an 8-bit destination operand but the instruction generates a result larger than $11111111_{b}$ ($255_d$), the Carry flag is set to 1 (CF = 1)
#### Manipulating the Carry flag
We use two operators to control manipulate the CF:
- **stc** - stands for "set carry flag", it sets the C flag to one.
- **clc** - stands for "clear the carry flag", it reset the value to zero.

Here you can see an simple example of code using that instruction:

```nasm
.section .text
.globl _start

_start:
    nop
    nop

    # set the C Flag (stc - set CF)
    stc                             # here we are fliping the CF to 1

    # unset the C flag (clc - clear CF)
    clc                             # Here we are clearing the CF flag (0)

    movl    $1, %eax
    movl    $0, %ebx
    int     $0x80
```

### Overflow Flag

The **Overflow Flag** (or _OF_) is another crucial status flag in the x86 processor, and it is primarily used to detect **signed integer overflow**. It is part of the same **EFLAGS** (or **FLAGS** in older processors) register, which holds various status indicators used to describe the result of executed instructions.
#### Function of the Overflow Flag
The _overflow flag_ is used to indicate when the result of an arithmetic operation (typically addition or subtraction) has exceeded the capacity of the destination operand when treated as a **signed** number. Essentially, this means that if the sign of the result does not make sense given the signs of the operands, the _overflow flag_ is set to **1**. Otherwise, it is set to **0**.
For example:
	1. If an instruction has a 16-bit destination operando but generates a negative result smaller than $1111111111111111_b$ ($-32.768_d$), the _Overflow Flag_ is set (OF = 1)
		2. Let $a > b> 0>c$  three numbers, if:
			- $a+b<0$, then CF = 1
			- $b-a<0$, then CF = 1
			- $a-c<0$, then CF = 1
		 This happens because the result exceeds the representable range for the   signed data type. 
	
> [!abstract] Remember
> The range for signed numbers is  $[-2^{n-1}, 2^{n-1} + 1]$,  where $n$ is the number of bits.
> So, by putting $n=16$, the range is $[-32768,3267]$

#### Manipulating the Overflow Flag
The manipulation of the Overflow Flag is a bit tricky. The maximum amount of data a register can hold is **0x7fffffff** ($2147483647_d$). We send this value to the register to reach its limit, and then, we send more data, causing the Overflow Flag (OF) to be triggered by the register overflow.
To clear the register we use the XOR logical operator with itself. (A xor A = 0, for every A)

```nasm
.section .text
.globl _start
_start:  

nop
nop  

# Set the Overflow Flag
movl $0x7fffffff, %eax     # Here we reached the register data limit
addl $1, %eax              # adding 1_d and triggering the overflow

# Clear the OF
xorl %eax, %eax            # Clear the register, now eax = 0  

# exit
movl $1, %eax
movl $0, %ebx
int $0x80
```

Note that , when we trigger the OF, another flags such the Parity Flag and Sing Flag are triggered too.

![[Pasted image 20241025113431.png]]

We'll see these flags bellow, and you be able to understand why that happens. 
### Parity Flag
The **Parity Flag** (or _PF_) is a status flag in the x86 processor used to indicate the **parity** of the **result of an arithmetic or logical operation**. Like the other flags, it is part of the **EFLAGS** (or **FLAGS** in older processors), which is a 32-bit register that holds various status flags.
#### Function of the Parity Flag
The **parity flag** is used to indicate whether the number of **1-bits** in the **least significant byte (the lower 8 bits)** of a result of a arithmetical or logical operation is **even** or **odd**. If the number of 1-bits is even, the _parity flag_ is set to **1**. Otherwise, it is set to **0**.
For example:
	- $0011 + 1010 = 1101$ => $1101$ have a odd quantity of 1 => PF = 0
	- $0000+1111=1100$ => $1100$ have an even quantity of 1 => PF = 1

#### Manipulating the Parity Flag
In that case we need to perform an arithmetical operation and make the result have a pair number of digit 1.
For that we move a byte with even bits (take $0xA=1010$ for example) of data (_movb_) to the _al_ register which is the lower 8 bits of the _eax_ register[^eax-details]. After that, we use the _test_ logical operator, which is similar to the _and_ but doesn't store the result (see the difference on the end of this section). So:
$$
	0000\ 1010\ \land 0000\ 1010 \implies 0000\ 1010 
$$
Therefore, CF will set to 1.
To clear, we move a odd bit, like $0x1$ to the _al_ register and use _test_ logical operator. In that case:
$$
	0000\ 0001\ \land 0000\ 0001 \implies 0000\ 0001 
$$
thus, CF will be 0.

```nasm
.section .text
.globl _start
_start:

	nop
	nop
	
	# Setting Parity Flag (PF)
	movb 	$0xA,  %al 	# 0xA = 1010 binary (a pair of digit 1)	 	
	test	%al,   %al	# perform AND op: (0000 1010) & (0000 1010) => 0000 1010

	# Clearing the PF
	movb	$0x1,  %al	# Moving 0001 to the al register
	test    %al,   %al	 

	# exit
	movl 	$1, %eax
	movl 	$0, %ebx
	int	$0x80
```

There's another way to clear the CF.
We can add 1 bit to the _al_ register, but it's not so reliable as the method above, because it will only work if _al_ stores a even number of bits.

> [!info] Differences between **test** and **and**
>**Effect on the Destination Operand**:
> - **and** modifies the destination operand with the result of the AND operation.
> - **test** does **not** modify either operand; it just checks the result and updates the CPU’s flags.
>
>**Purpose**:
> - **and** Used to perform actual bitwise operations on data. For example, masking out specific bits.
> - **test**: Used to perform a comparison to see if certain bits are set or clear, without modifying the operands.

[^eax-details]: **EAX** (32-bit): The general-purpose 32-bit register. * **AX** (16-bit): The lower 16 bits of EAX. * **AH** (8-bit): The higher 8 bits of AX. * **AL** (8-bit): The lower 8 bits of AX.
### Sign Flag
The **Sign Flag** (or _SF_) is a status flag in the x86 processor that indicates the **sign** of the **result of an arithmetic or logical operation**. Like the other flags, it is part of the **EFLAGS** register, which contains multiple status indicators used to describe the outcome of operations.
#### Function of the Sign Flag
The **sign flag** is used to reflect the **most significant bit (MSB)** of the result. In x86 architecture, the most significant bit determines whether a number is positive or negative when working with **signed** integers. If the result’s MSB is `1`, the **sign flag** is set to **1** (_SF = 1_), indicating a **negative** result. If the MSB is `0`, the **sign flag** is cleared to **0** (_SF = 0_), indicating a **positive** result.
For example:
	If the most significant bit (MSB) of the destination operand is 1, the Sign Flag is set (_SF = 1_).

#### Manipulating Sign Flag
The manipulation of the SF is similar to the  PF, we sing a negative value to the _eax_ register and use test to do a logical operator resulting in a negative result, which trigger the SF. To clear we make the result be positive.

```nasm
.section .text
.globl _start

_start:
	nop
	nop

	# Setting the SF
	movl	$-1,  %eax	# Send a negative value to eax
	test	$eax, %eax	# Make an and op with negative result

	# Clearing the SF
	movl $1,   %eax
	test %eax, %eax

	# exit
	movl	$1, %eax
	movl	$0, %ebx
	int	$0x80
```

### Zero Flag
The **Zero Flag** (or _ZF_) is a status flag in the x86 processor that indicates whether the **result of an arithmetic or logical operation** is **zero**. Like other flags, it is part of the **EFLAGS** register, which holds various status indicators used to describe the outcome of operations.
#### Function of the Zero Flag
The **Zero Flag** is set to **1** (_ZF = 1_) if the result of an arithmetic or logical operation is **zero**. Otherwise, it is cleared to **0** (_ZF = 0_).
For example:
	if an operand is subtracted from another of equal value, the Zero flag is set (_ZF = 1_).
#### Manipulating Sign Flag
Here we use _xor_  operation to make a logical result to be zero, finally we use test to clear the flag:

```nasm
.section .text
.globl _start

_start:
	nop
	nop

	# Setting the ZF
	movl 	$1,   %eax
	xorl	%eax, %eax

	# Clearing the ZF
	movl 	$1,   %eax
	test	%eax, %eax	

	# exit
	movl	$1, %eax
	movl	$0, %ebx
	int	$0x80
```

---
## Conditional Jump
There's a lot of conditional jump instruction that can be use:

| Instruction       | Description                       | Condition              |
| ----------------- | --------------------------------- | ---------------------- |
| **je**            | Jump if Equal                     | ZF = 1                 |
| **jne**           | Jump if Not Equal                 | ZF = 0                 |
| **jg**            | Jump if Greater (signed)          | ZF = 0 and SF = OF     |
| **jge**           | Jump if Greater or Equal (signed) | SF = OF                |
| **jl**            | Jump if Less (signed)             | SF ≠ OF                |
| **jle**           | Jump if Less or Equal (signed)    | ZF = 1 or SF ≠ OF      |
| **ja**            | Jump if Above (unsigned)          | CF = 0 and ZF = 0      |
| **jae**           | Jump if Above or Equal (unsigned) | CF = 0                 |
| **jb**            | Jump if Below (unsigned)          | CF = 1                 |
| **jbe**           | Jump if Below or Equal (unsigned) | CF = 1 or ZF = 1       |
| **jz**            | Jump if Zero                      | ZF = 1 (Same as `je`)  |
| **jnz**           | Jump if Not Zero                  | ZF = 0 (Same as `jne`) |
| **js**            | Jump if Sign (negative)           | SF = 1                 |
| **jns**           | Jump if Not Sign (non-negative)   | SF = 0                 |
| **jo**            | Jump if Overflow                  | OF = 1                 |
| **jno**           | Jump if No Overflow               | OF = 0                 |
| **jc**            | Jump if Carry                     | CF = 1                 |
| **jnc**           | Jump if Not Carry                 | CF = 0                 |
| **jp** / **jpe**  | Jump if Parity (even)             | PF = 1                 |
| **jnp** / **jpo** | Jump if Not Parity (odd)          | PF = 0                 |
#### Common Usage with `cmp`
To compare results we usually use the _cmp_ (compare) instruction. The compare instruction uses subtraction, it subtract source from destination.
Here’s how these instructions are used in typical `cmp` scenarios:
1. **Signed Comparisons**:    
    - **`cmp eax, ebx`** followed by `jg`, `jge`, `jl`, or `jle` checks if the signed integer in `eax` is greater, greater or equal, less, or less or equal to `ebx`.
2. **Unsigned Comparisons**:    
    - **`cmp eax, ebx`** followed by `ja`, `jae`, `jb`, or `jbe` checks if the unsigned integer in `eax` is above, above or equal, below, or below or equal to `ebx`.
3. **Equality and Zero Checks**:    
    - **`cmp eax, ebx`** followed by `je` or `jne` checks for equality or inequality.
    - **`cmp eax, 0`** followed by `jz` or `jnz` checks if `eax` is zero or non-zero.

#### Jump if Equal
Using the operator `je`, that stands for (jump if equal) we check if the both source and destination are equal. If `cmp` raise the Zero Flag, $i.e$, $source=destination$, the `jmp` instruction will be taken.
Using GDB debugger, we can pre calculate the result of `cmp` by using the following command:

```shell
(gdb) p/d $ebx-$eax
```

#### Jump if Zero
In that case, we the instruction `jz` (jump if zero). It will jump the Zero Flag is raised.
##### How does jz differs from je?
The `je` (Jump if Equal) and `jz` (Jump if Zero) instructions in x86 assembly are **functionally equivalent** because they both check the **Zero Flag (ZF)** to decide whether to jump. However, they are used in different contexts to improve code readability.
- `je` is typically used after a **comparison** (e.g., `cmp` or `sub`) to indicate a jump if two values are **equal**.
-  `jz` is often used after an instruction that affects the Zero Flag based on a **result being zero**. For example, it’s frequently used after a `test` or `and` instruction.
