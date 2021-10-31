# 3.2 Program Encodings

Relative to the computations expressed in the C code, optimizing compilers can rearrange execution order, eliminate unneeded computations ,replace slow operations with faster ones, and even change recursive computations into iterative one.



gcc -Og -o p p1.c p2.c

The gcc command invokes an entire sequence of programs to turn the source code into executable code.

First, the C .preprocessor expands the source code to include any files specified with #include commands and to expand any macros, specified with #define declarations,\
Second, the **compiler** generates assembly code versions of the two sources file having names p1.s, p2.s\
Next, the **assembler** converts the assembly code into binary object-code files p1.o, p2.o Object code is one form if machine code - it contains binary representations of all of the instructions, but the address of global values are not yet filled in.\
Finally, the linker merges these two object-code files along with code implementing library functions and generated the final executable code file p.

### 3.2.1 Machine-Level Code

Tow fo these abstraction are especially important for machine-level programming.

* &#x20;First, the format and behavior of machine-level program is defined by the _i**nstruction set architecture**, **or ISA,** defining the processor state, the format of the instructions, and the effect each of these instructions will have on the state_. Most ISAs, including x86-64, describe the behavior of a program as if each instruction is executed in sequence, with one instruction completing before the next one begins. **The processor hardware is far more elaborate, executing many instructions concurrently, but it employs safeguards to ensure the overall behavior matches the sequential operation dictated by the ISA.**
* &#x20;Second, the memory addresses used by a machine-level program are **virtual memory**, providing a memory model that appears to be a very large byte array, **The actual implementation of the memory system involves a combination of multiple hardware memories and operating system software. **

对于机器码级编程来说，有两种抽象非常重要：

* 首先，机器码级程序的格式和行为由**指令集架构**定义，他定义了处理器状态，指令的格式，以及每条指令对状态的影响。大部分指令集架构，包括 x86-64，将程序的行为描述成好像每条指令是顺序执行的，一条指令执行结束，下一条指令才会开始。处理器硬件远比描述的精密复杂，他会并发执行多条指令，但是有相关机制保证整体的行为和ISA描述的顺序执行行为完全一致。
* 第二个，机器级编程用到的内存地址是虚拟内存，提供的内存模型看上期是一个很大的字节数组，实际的内存系统的实现涉及到多个存储器硬件和操作系统软件。

The machine code for x86-64 differs greatly from the original C code.Parts of the processor state are visible that normally are hidden from the C programmer.

* The **program counter **(commonly referred to as the **PC**, and called **%rip** in x86-64) indicates the address in memory of the next instruction to be executed.
* The integer **register file **contains 16 named locations storing 64-bit values. These registers can hold addresses (corresponding to C pointers) or integer data, Some registers are used to keep track of critical parts of the program state, while others are used to hold temporary data, such as the arguments and local variables of a procedure, as well as the value to be returned by a function.
* The **condition code registers** hold status information about the most recently executed arithmetic or logical instruction These are used to implement conditional changes in the control or data flow, such as required to implement if and while statements.
* A set of **vector registers** can each hold one or more integer or floating-point values.\


x86-64的机器码和原始的C代码相差非常大，部分通常对C程序员隐藏的处理器状态是可见的:

* **程序计数器**（通常称为PC，在x86-64中用 %rip 表示）给出将要执行的下一条指令在内存中的地址
* 整数**寄存器文件**包含16个命名的地址，分贝存储64位的值。这些寄存器可以储存地址（对应C语言中的指针）或者整数。一些寄存器用来跟踪程序中重要的状态。其他的寄存器用来保存临时数据，例如执行过程中的参数和局部变量，以及函数的返回值。
* **条件码寄存器**用来保存最近一次执行算术或逻辑指令的状态信息，他们用来实现控制流或数据流中的条件改变，例如用来实现 **if** 或 **while** 语句。
* 一组向量寄存器可以用来保存一个或多个整数或浮点数。

Whereas C provides a model in which objects of different data types can be declared and allocated in memory, machine code views the memory as simply **a large byte-addressable array**. Aggregate data type in C such as arrays and structures are represented **in machine code as contiguous collections of bytes.** Even for scalar data types, assembly code makes no distinctions between signed or unsigned integers, between different types of pointers, or even between pointers and integers.\
虽然C语言提供了一种模型，可以在内存中声明和分配不同的数据类型，但是机器码只把内存看作是简单的巨大的，按字节寻址的数组。C语言中的聚合数据类型，例如数组和结构，在机器码中用一组连续的字节表示。甚至对于标量数据，汇编代码也不区分有符号无符号整数，不区分指针的类型，不区分指针和整数。

The program memory contains the executable machine code for the program, some information required by the operation system, a run-time stack for managing procedure calls and returns, and the blocks of memory allocated by the user(e.g., by using the malloc library function). As mentioned earlier, **the program memory is addressed using virtual address. At any given time, only limited subranges of virtual address are considered valid. **For example, x86-64 virtual addresses are represented by 64-bit words. In current implementations of these machines, the upper 16 bits must be set to zero, and so an address can potentially specify a byte over a range of 2^48, or 256 terabytes. More typical programs will only have access to a few megabytes, or perhaps several gigabytes. The operating system manages this virtual address space, translating virtual addresses into the physical addressed of values in the actual processor memory.\
程序内存中包含：

* 程序的可执行机器码
* 操作系统需要的信息
* 用来管理过程调用和返回的运行时栈
* 用户分配的内存块（例如使用 malloc 库函数分配）

正如前面提到的，程序内存使用虚拟内存来寻址。在任意给定的时刻，只有有限的一部分虚拟地址被认为是合法的。例如，x86-64的虚拟地址使用 64 位的字来表示。在目前的实现中，这些地址的高 16 为必须置为0，所以一个地址可以潜在地指定一个超过$$2^{48}$$或 64 TB 范围的字节。更典型的程序只能访问几兆字节，或者可能是几千兆字节。操作系统管理这些虚拟地址，将虚拟地址转化为实际处理器内存中的物理地址。\


**A single machine instruction performs only a very elementary operation.** For example, it might add two numbers stored in registers, transfer data between memory and a register, or conditionally branch to a new instructions address, The compiler must generate sequences of such instructions to implement program constructs such as arithmetic expression evaluation, loops, or procedure calls and returns.\
单条机器指令只会执行一个非常基本的操作。例如，将存放在寄存器中的两个数字相加，在内存和寄存器之间传递数据，或按条件转移搭配新的指令地址。编译器必须生成这些指令序列，从而实现（像算数表达式求值，循环，过程调用和返回）的程序结构。\
