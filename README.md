# RISCV

This github repository summarizes the progress made in the ASIC class about Riscv. Quick links:

- [Day 1-Introduction to RISC-V ISA And GNU compiler toolchain ](#Day1--Introduction-to-RISC-V-ISA-And-GNU-compiler-toolchain)
  
- [Day 2-Introduction to Application Binary Interface And Basic Error Flow](#Day2--Introduction-to-Application-Binary-Interface-And-Basic-error-flow)

- [Day 3 Introduction to TL Verilog and Makerchip](#Day-3-Introduction-to-TL-Verilog-and-Makerchip.)

- []()

- [Word of Thanks](#Word-of-Thanks)

- [Reference](#reference)

# Day 1-Introduction to RISC-V ISA And GNU compiler toolchain 

<details> 
<summary> Installation </summary>
	
Steps to install Risc-tools (linux)

```
sudo apt install libboost-all-dev
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh
./run.sh

```

 Once you run it you will get make error. ignore it  and type the following command

 ```

cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install

```

- To set the PATH variable in .bashrc

```

gedit .bashrc
#Instead of Alwin put your username
export PATH="/home/Alwin/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH" #Type at last line # close the bashrc and type
source .bashrc

```
For installation in mac just follow the below github link given in reference.

</details>

<details>
  <summary>Introduction to RISC-V ISA </summary>

  RISC-V ISA is a base integer ISA and must be present in any implemenatation along with some optional extension. The RISC-V has been designed to support extensive customization and specialization which can be extended  with  one  or  more  optional  instruction-set  extensions,  but  the  base  integer instructions cannot be redefine. The different instructions included in RISC-V are listed below.

1. Pseudo instructions - For e.g- mv,li,ret etc
2. Base integer instruction (RV64I, RV32I)-For e.g-lui,addi etc
3. Multiply extension (RV64M) -For e.g- mulw,divw etc
4. Single and double floating point instruction (RV64F, RV64D) -For e.g- flw,fadd etc
5. Application binary instruction 
6. Memory allocation and stack pointer

The detail of the RISC-V instructions set manual can be found [here](https://riscv.org/wp-content/uploads/2017/05/riscv-spec-v2.2.pdf).

Each base integer set is characterized by the  width  of the register (XLEN) and size of the user address space. The most important advantage of RISC-V is that it is an open standard instruction which is easily available for academics and commercial purposes free of cost.

</details>

<details>
  <summary>GNU compile toolchain</summary>

  The GNU compile toolchain is a set of programming tools in LINUX system that can be use for compiling a code to generate certain executable program, library and debugger and whose detail can be found in references. RISC-V is one such toolchain which supports C and C++ cross compiler. It supports two build modes: a generic ELF/Newlib toolchain and a more sophisticated Linux-ELF/glibc toolchain and the github link for the same can be found in references. 

1. Compiler and linker which transform the source code into an executable program
2. Libraries which provide interfaces to the operating system 
3. Debugger which is used to test and debug created program

To start off a c program to compile sum from 1 to n was written whose  codes given below as [sum1to6.c]

```

#include <stdio.h>

int main () {
	int i,sum = 0, n = 6;
	for (i = 1; i <=n; ++i) {
		sum += i;
	}
	printf("The sum of the number from 1 to %d is %d\n", n,sum);
	return 0;
	}

```

In case RISC-V GNU toolchain the follwing commands are executed

- To use the RISC-V gcc compiler or simulator, type
  
```
riscv64-unknown-elf-gcc  -o <object filename.o> <filename.c>

```
Here -01 gives 15 instructions set while -0fast gives us 12 instructions set(ubuntu) here in macos we get 15 instructions

More generic command with different options:

    `riscv64-unknown-elf-gcc <compiler option -O1 ; Ofast> <ABI specifier -lp64; -lp32; -ilp32> <architecture specifier -RV64 ; RV32> -o <object filename> <C      filename>`
    
-  To list the details of a file
  
  ```
ls -ltr <filename.o>

```

<img width="682" alt="Screenshot 2023-08-18 at 11 33 19 PM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/c768b542-363a-4a6f-87b0-754f5d9a3484">

- To deassemble the object file 

```

riscv64-unknown-elf-objdump <object file> -d <object filename.o>

```

- Below image shows the disassembled file `sum1to6.o` with `main` function highlighted.


```

riscv64-unknown-elf-objdump <object file> -d <object filename.o> | less

```

<img width="682" alt="Screenshot 2023-08-18 at 11 33 45 PM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/27322cef-c5db-4cbe-942f-4889623380cd">

```
/main
n

```
This code helps in seeing the main file:

<img width="1440" alt="Screenshot 2023-08-18 at 11 34 06 PM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/0ddce653-dfe2-4331-a82d-8b4ec1fc1d1f">

here we can check the instruction set is 15 by subtracting  10214-101ac = 58\4=15 instruction sets 

** To compile:**

```
spike pk sum1ton.o
```
<img width="682" alt="Screenshot 2023-08-19 at 12 30 02 AM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/5bbd2c8d-65e2-4e03-b234-551ad411197f">

** To debug using spike:**

```
spike -d pk sum1ton.o
```
After running the above code line a number of things can be done as demonstrated in the image below. The code can be manually debugged, part of it can be run and contents of registers can be checked.

```:q``` to quit. 

  </details>

  <details>
	  <summary>
	  Number system in RiscV
  </summary>
	  
The RISC-V architecture defines several different data types and number systems to represent and manipulate data. Here, I'll explain the basic number systems used in RISC-V:

- Binary Number System: RISC-V, like most digital systems, primarily operates on binary data. In the binary number system, numbers are represented using only two symbols: 0 and 1. Each digit in a binary number represents a power of 2. For example, the binary number "1101" represents (1 * 2^3) + (1 * 2^2) + (0 * 2^1) + (1 * 2^0) = 13 in decimal.
- Integer Representation: RISC-V supports different integer data types with varying sizes. The most common are 32-bit and 64-bit integers, denoted as "RV32" and "RV64" respectively. Integers are typically represented in two's complement form, which allows both positive and negative values to be stored and manipulated using the same hardware.
- Floating-Point Representation: RISC-V also supports floating-point operations for real numbers. Floating-point numbers are represented using a sign bit, an exponent, and a fraction (also known as mantissa). RISC-V defines different formats for floating-point numbers, including the IEEE 754 standard formats (single precision, double precision, etc.). These formats allow a wide range of values to be represented with varying levels of precision.
- Hexadecimal Notation: While binary is the fundamental representation in RISC-V, hexadecimal (base-16) notation is often used to represent binary numbers in a more human-readable form. Each hexadecimal digit represents four bits. For example, the binary number "11011010" can be represented as "DA" in hexadecimal.
- Memory Addressing: RISC-V CPUs use memory addresses to access data stored in memory. Memory addresses are typically represented in hexadecimal form. The exact memory addressing scheme depends on the specific RISC-V implementation and the memory model being used.
Overall, the RISC-V architecture provides a flexible framework for representing and manipulating different types of numbers, allowing software developers and hardware designers to efficiently perform arithmetic and logical operations on various data types within the context of RISC-V-based systems.

In computer architecture, the terms "bit," "byte," "word," and "double word" refer to different units of data storage and manipulation. These terms are used to describe the size of data that a computer's memory and processing units can handle. The specific sizes of these units can vary based on the architecture and implementation, but I'll provide you with some common interpretations:

- Bit: A bit is the smallest unit of data in computing. It can represent one of two values: 0 or 1. Bits are the building blocks of all digital information and are used to represent various types of data and instructions in a computer's memory and processing units.
- Byte: A byte is a group of 8 bits. It is the basic addressable unit of memory storage in most computer architectures. Bytes are commonly used to represent characters, numbers, and other small data elements. For example, the ASCII code for the letter 'A' is 65, which can be represented as a byte with the binary value 01000001.
- Word: The term "word" refers to the natural data size that a computer's central processing unit (CPU) can process in a single operation. The size of a word can vary between different computer architectures. In the context of x86 and x86-64 architectures, a word is typically 16 bits, while in other architectures like RISC-V, a word can be 32 bits or 64 bits. The size of a word determines the maximum amount of data that the CPU can manipulate at once, which can impact the efficiency of data processing.
- Double Word (Dword): The term "double word" (often abbreviated as "dword") is used to describe a data unit that is twice the size of a standard word. In x86 and x86-64 architectures, a double word is 32 bits, while in some other architectures, it can refer to a 64-bit value. The term "dword" is often used in the x86 family of processors to describe a 32-bit data value.
It's important to note that the exact sizes of these units can vary based on the computer architecture and implementation.
**Total Number of pattern by RV64 will be 2^64**
**RISC- doubleword can represent '0' to '(2^64 - 1)' unsigned numbers or positive numbers**

  <img width="1440" alt="Screenshot 2023-08-19 at 2 15 56 AM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/44fcfeb1-56a6-4982-b172-864357dfe28d">

## Signed Numbers
To find negative number: we find 2's complement
- First the normal binary of the +ve number 
- Then we invert the number
- Then we add +1 to it
(-1,152,991,877,645,991,936)dec = (0001000000000000010000000000000100000000000000000000000000000000)bin
				  (1110111111111111101111111111111011111111111111111111111111111111)bin
											+1
  				    (1110111111111111101111111111111100000000000000000000000000000000)bin


  </details>
  
<details>
<summary>Lab For Signed And Unsigned Numbers</summary>

Code for unsignedHighest:

```
#include <stdio.h>
#include <math.h>
int main() {
unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
printf("highest number represented by unsigned long long int is %llu\n", max);
return 0;
}

```

<img width="682" alt="Screenshot 2023-08-19 at 2 37 14 AM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/abfe12ba-f1e9-40c1-b3a3-1c267d73fcec">


This show the Highest value for Unsigned Numbers
if we tweak the code to get the lowest value for Unsigned Number by changing (pow(2,64) * -1);

![Screenshot 2023-08-19 at 2 41 16 AM](https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/7e655ffe-1b19-40d5-a041-14108865fc2b)

Code for signed Numbers:

```
#include <stdio.h>
#include <math.h>
int main(){
long long int max = (long long int) (pow(2,10) * -1);
printf("highest number represented by long long int is %lld\n", max);
return 0;
}

```

<img width="682" alt="Screenshot 2023-08-19 at 2 46 18 AM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/ecf3d7cf-ee06-4d48-a146-9a974c09660b">


## Example no.1

code:

```
#include <stdio.h>
#include <math.h>
int main() {
long long int max = (int) (pow(2,63) -1);
long long int min = (int) (pow(2,63) * -1);
printf("highest number represented by long long int is %lld\n", max);
printf("lowest number represented by long long int is %lld\n", min);
return 0;

```

![Screenshot 2023-08-19 at 2 56 52 AM](https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/d881e7f5-2837-402c-a6e0-3df8a7336d8e)

Here in this code we have to change :

```
long long int max = (long long int) (pow(2,63) -1);
long long int min = (long long int) (pow(2,63) * -1);

```
so corrected output :

<img width="682" alt="Screenshot 2023-08-19 at 3 06 23 AM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/fc038bb5-9e97-4c83-afaa-ba2e1aa22224">

Table : 

<img width="1440" alt="Screenshot 2023-08-19 at 2 59 33 AM" src="https://github.com/alwinshaju08/Alwin_iiitb_asic_class/assets/69166205/2769ceb4-e909-48f9-a051-d5b23163e190">


## labwork:

1.FOR THE C PROGRAM USED IN LABS, USE A VALUE OF N=9, COMPILE AND SIMULATE USING GCC COMPILER. WHAT IS THE OUTPUT YOU GET?

<img width="682" alt="Screenshot 2023-08-19 at 2 58 25 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/f9aa4d1f-af6a-4afc-96a4-e014b9af6dd6">

2.AS SHOWN IN LABS, COMPILE THE C PROGRAM (N=9) USING RISCV-GCC COMPILER WITH O1 SWITCH AND LOOK AT ASSEMBLY CODE USING RISCV-OBJDMP. WHAT IS THE MEMORY LOCATION OF "PRINTF" SUBROUTINE ?

<img width="1440" alt="Screenshot 2023-08-19 at 3 01 02 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/825fca29-1ccb-45ca-b05e-0c1293afa521">

3.HOW MANY INSTRUCTIONS ARE USED IN "PRINTF" SUBROUTINE FOR C PROGRAM (N=9) COMPILED WITH RISCV-GCC AND O1 SWITCH?

ans.21

4.AS SHOWN IN LABS, COMPILE THE C PROGRAM (N=9) USING RISCV-GCC COMPILER WITH OFAST SWITCH AND LOOK AT ASSEMBLY CODE USING RISCV-OBJDMP. HOW MANY INSTRUCTIONS ARE USED BY "MAIN" PROGRAM ?

ans.11

5.DEBUG C PROGRAM (N=9) WITH SPIKE AND RUN UNTIL PC IS 10200. WHAT ARE THE CONTENTS OF REGISTER A0?

![Screenshot 2023-08-19 at 3 07 34 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/10b6bea1-6fcf-4342-9940-f6d6f5d779a9)

6.DEBUG C PROGRAM (N=9) WITH SPIKE AND RUN UNTIL PC IS 10200. WHAT ARE THE CONTENTS OF REGISTER SP?

<img width="682" alt="Screenshot 2023-08-19 at 3 09 51 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/f23d64f2-9974-453c-832c-7fefdd59e029">

7.DEBUG C PROGRAM (N=9) WITH SPIKE AND RUN UNTIL PC IS 101C4. WHAT ARE THE CONTENTS OF REGISTER A0?

<img width="682" alt="Screenshot 2023-08-19 at 3 12 38 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/80952a0c-6c71-42d0-ba5e-92878cdd238b">

8.DEBUG C PROGRAM (N=9) WITH SPIKE AND RUN UNTIL PC IS 10212. WHAT IS THE OUTPUT ON SHELL?

<img width="682" alt="Screenshot 2023-08-19 at 3 14 05 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/78a4d405-fdde-467a-82e1-560844915563">

</details>


## Day 2-Introduction to Application Binary Interface And Basic Error Flow

<details>
<summary>
	Introduction to Application Binary Interface
</summary>
	
* The application program can directly access the registers of the RISC V architecture using something known as system calls. The ABI (also known as system call interface enables the application to access the hardware resources via registers.
  
* In RISC V architecture, the width of the register is defined as XLEN. For RV64 and RV32, the widths are 64 bits and 32 bits, respectively.
  
* RISC V belongs to the little endian memory addressing system, which means that the least significant byte of a word is stored in the smallest memory address.

An Application Binary Interface is a set of rules enforced by the operating system on a specific architecture. So, Linker converts relocatable machine code to absolute machine code via ABI interface specific to the architecture of machine.
Just like how application program interface (API) is used by application programs to access the standard libraries, an application binary interface or system  call interface is utilised to access hardware resources . The ISA is inherently divided into two parts: *User & System ISA* and *User ISA*  the latter is available to the user directly by system calls. 
  
Now, how does the ABI access the hardware resources? 
- It uses different registers(32 in number) which are each of width `XLEN = 32 bit` for RV32 (~`XLEN = 64 for RV64`) . On a higher level of abstraction these registers are accessed by their respective ABI names.
  
  For base integer instructions there are broadly 3 types of of such registers:
  - I-type : For instructions having immediate values as operands.
  - R-type : For instructions having only registers as operands.
  - S-type : For instructions used for storing operations.

So, it is system call interface used by the application program to access the registers specific to architecture. Overhere the architecture is RISC-V, so to access 32 registers of RISC-V below is the table which shows the calling convention (ABI name) given to registers for the application programmer to use.


# Load,Add And Store Instructions

	ld x8,16(x23)
 
 here ld is for load doubleword,x8 shows destination register (rd),16 is offset,x23 is source register . This is I type Instructions  :

<img width="1033" alt="Screenshot 2023-08-19 at 11 24 09 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/d9f8e2ce-f424-47dc-934f-b00ef5d9de4a">

	add x8,x29,x8
 here add is function,x8 is destination register (rd),x29 & x8 is source register. This is R type Instructions  :

 ![Screenshot 2023-08-19 at 11 24 19 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/32aeb799-fa17-4dc2-9186-7b142f341f10)

 	sd x8,8(x23)
  here store is store doubleword,x8 is data registers,8 tell offset(immediate) ,x23 is source register. This is S type Instructions  :

  ![Screenshot 2023-08-19 at 11 31 29 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/b20b0475-88b1-45e1-88cd-84e19cbec61a)

Here in each Instructions set we can see register are of 5 bits so total number of register = 2^5 = 32 registers

# RISC-V Block Diagram

![RISC-V](https://github.com/kuby1412/RISC-V-MYTH-Workshop/blob/master/Documentation/Block%20Diagram.PNG)

![RISC-V](https://github.com/kuby1412/RISC-V-MYTH-Workshop/blob/master/Documentation/Block%20Diagram%20v2.PNG)


![calling_convention](https://github.com/kuby1412/RISC-V-MYTH-Workshop/blob/master/Documentation/ABI.png)

## labworks

Code for labwork:

<img width="1440" alt="Screenshot 2023-08-19 at 3 59 15 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/8440a5c2-7781-4288-8a73-07f7872c76db">


1.MODIFY 1TO9_CUSTOM.C AND LOAD.S AS SHOWN IN VIDEO. WHAT IS THE OUTPUT OF SIMULATION WITH -OFAST ?

![Screenshot 2023-08-19 at 3 42 33 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/cb6dd5a6-f0bb-4c53-b6ec-ecda65c6e1e7)

2.WHAT IS THE MEMORY LOCATION OF LOAD SUBROUTINE?

![Screenshot 2023-08-19 at 3 33 59 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/8fa6ac44-a7e0-47ab-bba8-c4f47cdb0b6f)

3.WHAT IS THE MEMORY LOCATION OF LOOP SUBROUTINE?

 <img width="682" alt="Screenshot 2023-08-19 at 3 37 39 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/aeb90c06-d65e-4214-80d4-568d12f52eb3">

4.OPEN SPIKE DEBUGGER AND RUN 1TO9_CUSTOM.O UNTIL PC IS 101AC WHAT IS THE VALUE OF A0 AND A1 REGISTERS?

<img width="682" alt="Screenshot 2023-08-19 at 3 50 20 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/7d2f5b78-0e7e-45ae-a72b-b2117004e0bc">

5.OPEN SPIKE DEBUGGER AND RUN 1TO9_CUSTOM.O UNTIL PC IS 101B2 WHAT IS THE VALUE OF A0 AND A1 REGISTERS?

<img width="638" alt="Screenshot 2023-08-19 at 3 54 07 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/409a3ab5-0485-4336-826a-4e4c29bb05a7">

</details>

<details>
	<summary>Lab to run c program on RISC-V CPU</summary>

 <img width="1440" alt="Screenshot 2023-08-19 at 5 49 22 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/ff0d999e-3631-455c-9e23-7dca14779118">

Here we have riscv cpu program code through which we send the HEX format file of c program to show output the output of the given code 

```
chmod 777 rv32im.sh
./rv32im.sh 

```
<img width="682" alt="Screenshot 2023-08-19 at 5 52 57 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/3db2b137-a683-40db-bfb8-e9ef5f5480fd">

Input hex file to sent through verilog code:

firmware.hex:

<img width="682" alt="Screenshot 2023-08-19 at 5 53 44 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/bbe00737-8a05-4ab7-9f7f-52ac30308975">

firmware32.hex:

<img width="682" alt="Screenshot 2023-08-19 at 5 53 54 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/4743a68b-6822-485f-a33d-c0a13e2d2e11">

</details>

## Day 3 Introduction to TL Verilog and Makerchip
<details >
<summary >Introduction</summary>

TL-Verilog was used as the HDL of choice for this project. Projects on Makerchip can be completely designed using TL-Verilog. Transaction Level - Verilog standard is an extension of Verilog which has various advantages like simpler syntax, shorter codes and easy pipelining. You can learn more about TL-Verilog [here](http://tl-x.org/).

Timing abstract can be done in TL-Verilog. This model is specified for pipelines where the sequential elements are generated by tools from the pipelined specification. This allows for easy retiming without the risk of introduction of any functional bugs. More information on timing abstract in TL-Verilog can be found in the IEEE paper ["Timing-Abstract Circuit Design in Transaction-Level Verilog" by Steven Hoover](https://ieeexplore.ieee.org/document/8119264).

</details>

<details >
<summary >Lab for Combinational logic </summary>

Makerchip IDE

Makerchip is a free online environment for developing high-quality integrated circuits. You can code, compile, simulate, and debug Verilog designs, all from your browser. Your code, block diagrams, and waveforms are tightly integrated.

## Loading pythagorean Example on Makerchip IDE

<img width="1274" alt="Screenshot 2023-08-19 at 10 11 28 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/91877916-5394-48e6-a156-659fbe506cc6">

## AND Gate Example on Makerchip IDE

<img width="1274" alt="Screenshot 2023-08-19 at 10 33 45 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/2adf559c-45f2-41f4-9f19-2602b09266a4">

## Lab On Understanding Usage Of Vector

<img width="1290" alt="Screenshot 2023-08-20 at 2 31 28 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/83c2d94c-ac46-40e0-9c78-fbe64a7669f6">

## Multiplexer on Makerchip IDE

<img width="1290" alt="Screenshot 2023-08-20 at 2 30 47 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/5094aaab-ad29-45fe-992d-4215f90eea83">

## Calculator on Makerchip IDE

<img width="1401" alt="Screenshot 2023-08-20 at 3 09 19 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/ece71b8a-04b6-404a-ac81-384eba0d87a3">

</details>
<details >
<summary >Lab for Sequential  logic </summary>

Sequential logic refers to a type of digital logic circuit or system in which the output depends not only on the current inputs but also on the previous states of the circuit. Unlike combinational logic, which only considers the current inputs to generate outputs, sequential logic incorporates memory elements to store information and generate outputs based on both current inputs and past history.

## Lab On Free Running Counter

<img width="475" alt="Screenshot 2023-08-20 at 3 18 34 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/8ecfee27-aa83-49a3-808d-21a4882d2741">

Output:

<img width="1263" alt="Screenshot 2023-08-20 at 11 49 29 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/44fc4d1b-b32a-483c-8b74-18c6eafa5809">

## Sequential Calculator To Remembers Previous Results For Next Calculations 


 
</details>

## Word of Thanks
I sciencerly thank **Mr. Kunal Gosh**(Founder/**VSD**) for helping me out to complete this flow smoothly.

## Acknowledgement
- Kunal Ghosh, VSD Corp. Pvt. Ltd.
- Chatgpt
- Kanish R,Colleague,IIIT B
- Sumanto Kar,Sr. Project Technical Assistant , IIT Bombay
- Pruthvi Parate,Colleague,IIIT B
- Emil Jayanth Lal,Colleague,IIIT B
- Bhargav Dv,Colleague,IIIT B
- Sai Sampath,Colleague,IIIT B
  
## Reference 
- https://www.vsdiat.com
- https://en.wikipedia.org/wiki/Toolchain
- https://en.wikipedia.org/wiki/GNU_toolchain
- https://github.com/riscv/riscv-gnu-toolchain
- https://github.com/KanishR1
- https://github.com/riscv-software-src/homebrew-riscv/tree/main
