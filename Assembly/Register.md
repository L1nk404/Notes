
A register is a small space in the CPU that hold any CPU instruction and any memory address or any king of data.

![[Pasted image 20240522112750.png]]

![[Pasted image 20240522112857.png]]

### Types

In a 32-bit architecture, there are three main types of registers:

#### 1.General Purpose Registers (GPRs)
These are used for various operations, including arithmetic and data manipulation. Examples include `EAX`, `EBX`, `ECX`, and `EDX`. In a 32-bit architecture, there are eight General Purpose Registers (GPRs), each with a specific use, although they can be used for various purposes. Here they are:

1. **EAX (Extended Accumulator Register)**:	
	- **Primary Use**: Used in arithmetic operations. It often holds the return value of functions. Often holds system call numbers in Linux when making system calls via the int 0x80 instruction.
	- **Example**: Results of calculations, such as addition or multiplication. Sometimes used to pass the first argument to system calls.
2. **EBX (Extended Base Register)**:	
	- **Primary Use**: Often used as a base pointer for memory access in some addressing modes.
	- **Example**: Accessing arrays or memory locations.
3. **ECX (Extended Count Register)**:	
	- **Primary Use**: Used as a counter in loop operations.
	- **Example**: Counting iterations in loops.
4. **EDX (Extended Data Register)**:	
	- **Primary Use**: Used in input/output operations and multiplication/division and to store data for input/output operations. Sometimes used to pass arguments to system calls.
	- **Example**: Storing data for multiplication or division operations.
5. **ESI (Extended Source Index)**:	
	- **Primary Use**: Used as a pointer to the source in string operations.
	- **Example**: Reading data from memory in string manipulation functions.
6. **EDI (Extended Destination Index)**:	
	- **Primary Use**: Used as a pointer to the destination in string operations.
	- **Example**: Writing data to memory in string manipulation functions.
7. **EBP (Extended Base Pointer)**:	
	- **Primary Use**: Used to point to the base of the stack frame, facilitating stack operations.
	- **Example**: Accessing local variables and function parameters on the stack.
8. **ESP (Extended Stack Pointer)**:	
	- **Primary Use**: Points to the top of the stack, managing stack operations.
	- **Example**: Pushing and popping values from the stack.
9. **ESI (Extended Source Index)**:
	- **Primary Use**: Used as a pointer to the source data in string and memory operations. It is particularly useful in operations that manipulate strings or arrays, allowing for efficient data copying and manipulation.
	- **Example**: In string manipulation functions, `%esi` is often used to point to the source data that is being read or copied. For instance, when using the `rep movsb` instruction, which copies bytes from the source address pointed to by `%esi` to the destination address pointed to by `%edi`, `%esi` indicates where the data is coming from. This makes it ideal for operations like reading from an array or a string buffer.

> [!NOTE] Additional Context:
> - **Common Usage**: In assembly programs that involve loops or repetitive tasks, `%esi` is frequently paired with string operations (like `movsb`, `stosb`, etc.) to handle data efficiently. It typically points to the beginning of a source buffer, and as the operation proceeds, it may increment automatically depending on the operation.
> 
> - **Preservation**: Since `%esi` is often used for string operations, care must be taken to preserve its value if it is being used in a more complex function or algorithm. This can be done by saving its value on the stack or using another register temporarily.
> 

##### Let's see a example:

Given the following part of Hello World code:

```
movl $4, %eax               # Calling write syscall (4)
movl $1, %ebx               # stdout
movl $msg, %ecx             # Moving buffer pointer to write syscall (just the first element of msg)
movl $str_lenght, %edx      # buffer size of msg (using static symbol)
int $0x80                   # interrupt
```

The function of the registers is:
- **%eax**:    
    - **Value**: `4`
    - **Purpose**: Holds the system call number for `sys_write` (4).
    - **Role**: Indicates to the kernel that the `write` system call should be performed.

- **%ebx**:    
    - **Value**: `1`
    - **Purpose**: Holds the file descriptor for the system call.
    - **Role**: Specifies the output destination as `stdout` (file descriptor 1).

- **%ecx**:    
    - **Value**: `msg`
    - **Purpose**: Holds the address of the buffer to write.
    - **Role**: Points to the string "Hello World\n" that will be written to `stdout`.

- **%edx**:    
    - **Value**: `str_lenght` (which is 13)
    - **Purpose**: Holds the length of the buffer to write.
    - **Role**: Specifies the number of bytes to write from the buffer.
- **int $0x80**: This instruction triggers the system call interrupt, transferring control to the kernel to execute the `write` system call with the provided arguments.

##### Use of the register to  syscall.

In x86 assembly, the choice of registers in a syscall block is not arbitrary; it follows a specific convention. For Linux x86 (32-bit) system calls, each parameter must be placed in a specific register as defined by the calling convention. You can't choose any general-purpose register freely.

The convention for 32-bit Linux syscalls:
1. **EAX**: Holds the syscall number.
2. **EBX**: Holds the first parameter (e.g., file descriptor in `sys_read`).
3. **ECX**: Holds the second parameter (e.g., the buffer pointer in `sys_read`).
4. **EDX**: Holds the third parameter (e.g., the number of bytes to read in `sys_read`).
5. **ESI** and **EDI**: Used for the fourth and fifth parameters, if the syscall takes more than three arguments.

#### 2.Segment Registers 
These are used to access different segments of memory. Examples include `CS` (Code Segment), `DS` (Data Segment), `SS` (Stack Segment), and `ES` (Extra Segment).In 32-bit architecture, there are six Segment Registers that help manage and access different segments of memory. Here's an explanation of each:
1. **CS (Code Segment)**:
	
	- **Purpose**: Points to the segment containing the code (instructions) that the CPU executes.
	- **Example**: It helps the CPU know where the current executable code resides.
2. **DS (Data Segment)**:
	
	- **Purpose**: Points to the segment containing data used by the program.
	- **Example**: It helps access global and static variables.
3. **ES (Extra Segment)**:
	
	- **Purpose**: An additional data segment used for extra data storage.
	- **Example**: Often used in string operations alongside other segment registers.
4. **FS (General Purpose Segment)**:
	
	- **Purpose**: Can be used to point to additional segments, often used for thread-local storage.
	- **Example**: In modern operating systems, it's often used to access thread-specific data.
5. **GS (General Purpose Segment)**:
	
	- **Purpose**: Another general-purpose segment register, similar to FS, and can be used for specific purposes defined by the operating system or application.
	- **Example**: Sometimes used for handling special data structures in multi-threaded applications.
6. **SS (Stack Segment)**:
	
	- **Purpose**: Points to the segment containing the stack, which is used for function calls and local variables.
	- **Example**: It helps manage function calls, returns, and local variables stored on the stack.
	    
#### 3.Control Registers or Instruction Pointer Register
These are used to control various aspects of the CPU's operation, such as the `EIP` (Instruction Pointer) which points to the next instruction to be executed, and `EFLAGS` which contains flags that control CPU operations.
##### EIP Register
The _i_ stands for instruction. The _p_ on the end of any register stands for pointer. So the _EIP_ register is a instruction pointer, which points to what will be executed in the next step.

![[Pasted image 20241029141014.png]]

The in the image indicate where the _EIP_ instruction are. Notice that the memory location hold by _EIP_ is the same as the instruction which the arrow are. 

> [!warning] Note
> Is important to note that the instruction that the EIP is pointed are not executed, because the EIP register point to the **next instruction to be executed**
