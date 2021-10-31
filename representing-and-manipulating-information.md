# Representing and Manipulating Information

Modern computers store and process information represented as two values signals.&#x20;

We consider the three most important representations of numbers: **unsigned encodings**, **Two's-complement encodings**, **Floating-point encodings**.

Multiplication is associative and commutative.\
Floating-point arithmetic is not associative

## 2.1 Information Storage

Rather than accessing individual bits in memory, most computers **use blocks of 8 bits, or bytes, as the smallest addressable unit of memory. **A machine-level program views memory as **a very large array of bytes**, referred to as **virtual memory**.\
The value of a pointer in C -- whether it points to an integer, a structure, or some other program object -- is the virtual address of the first byte of some block of storage.

### 2.1.1 Hexadecimal Notation

Conversely, given a binary number, you can covert it tot hexadecimal by first splitting it into groups of 4 bits. Note, however, that if the total number of bits is not a multiple of 4, you should make the **leftmost** group be the one with fewer than 4 bits, effectively  padding the number with leading zeros.

### 2.1.2 Data Sizes

Every computer has a **word size(字长)**, indicating the **nominal size(标称大小)** of pointer data. Since a virtual address is encoded by such  a word, the most important system parameter determined by the word size is the maximum size of the **virtual address space**. That is, for a machine with a w-bit word size, the virtual address can range from 0 to 2^w - 1, giving the program access to at most 2^w bytes.

### 2.1.3 Addressing and Byte Ordering

For program objects that **span multiple bytes(跨越多字节)**, we must establish two conventions: **what the address of the object will be**, and **how we will order the bytes in memory.**

**In virtually all machines, a multi-byte object is stored as a contiguous sequence of bytes, with the address of the object given by the smallest address of the bytes.**

For ordering the bytes representing an object, there are two common conventions. Some machine choose to store the object in memory ordered from least significant byte to most, while other machines store them from most to least. as **little endian**, or **big endian**. There is no technical reason to choose one byte ordering convention over the other.

When binary data are communicated over a network between different machines, **code written for networking applications must follow established conventions for byte ordering to make sure the sending machine converts its internal representation to the network standard, while the receiving machine converts the network standard to its internal representation.**

### 2.1.4 Representing String

A string in C is encoded by an array of characters terminated by the null(having the value 0) character, text data are more platform independent than binary data.

### 2.1.5 Representing Code

Different machine types use different and incompatible instructions and encodings. Binary code is seldom portable across different combinations of machine and operating system.

### 2 .1.6 Introduction to Boolean Algebra

One useful application if bit vectors is to represent finite sets. We can encode any subset A include {0, 1, ...., w-1} with a bit vector \[aw-1, aw-2, ..., a1, q0]

### 2.1.7 Bit-Level Operation in C

One common use of bit-level operations is to implement _masking operations _where a mask is a bit pattern that indicates a selected set of bits within a word.

### 2.1.8 Logical Operation in C

The logical operations treat any nonzero argument as representing TREUE and argument 0 as representing FALSE. They return either 1 or 0, indicating a result of either TREU or FALSE, respectively.

### 2.1.9 Shift Operations in C

Generally, machines support two forms of right shift:\
Logical, A logical right shift fills the left end with k zeros.\
Arithmetic. An arithmetic right shift fills the left end with k repetitions of the most significant bit.

In practice, almost all compiler/machine combinations use arithmetic right shifts for signed data, and many programers assume this to be the case.  For unsigned data, on the other hand, right shifts must be logical.

## 2.1 Integer Representations

### 2.2.1 Integral Data Types

**The ranges are not symmetric -- the range of negative numbers extends one further than the range of positive numbers.**

### 2.2.2 Unsigned Encoding

B2Uw (Binary to Unsigned) is **bijection**(双射)\
**PRINCIPLE: Uniqueness of unsigned encoding**

### 2.2.3 Two's-Complement Encodings

B2Tw(Binary to tow's-com) is a **bijection**\
**PRINCIPLE: Uniqueness of tow's-complement encoding**

Integer 类型转换，由 short 到 int， 由 int 到 short 都是补最高位或者截断最高位 \
implicit conversion of signed to unsigned can lead to errors or vulnerabilities, unsigned 类型进行减法时容易出现bug，如 0 - 1 得到的结果是 Umax， 因为 unsigned 类型没有复数，因此出现负数时会自动转为正书，应避免 unsigned 类型的运算
