# RISCV

This github repository summarizes the progress made in the ASIC class about Riscv. Quick links:

- [Day 1-Introduction to RISC-V ISA And GNU compiler toolchain ](#Day1--Introduction-to-RISC-V-ISA-And-GNU-compiler-toolchain)
  
- [Day 2-Introduction to Application Binary Interface And Basic Error Flow](#Day2--Introduction-to-Application-Binary-Interface-And-Basic-error-flow)

- [Day 3-Introduction to TL Verilog and Makerchip](#Day-3--Introduction-to-TL-Verilog-and-Makerchip.)

- [Day 4-Basic RISC-V CPU micro-architecture](#Day-4--Basic-RISC-V-CPU-micro-architecture)

- [Day 5-Complete Pipelined RiscV CPU Micro-architecture](#Day-5--Complete-Pipelined-RiscV-CPU-Micro-architecture)

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

we start with understanding the Makerchip IDE platform by trying some basic digital logic gate with And Gate being the standard one. In TL verilog we simply code the logic itself viz $out = $in1 & $in2  without requiring to declare the variables separately and $in assignment is also not required. The output of the above is as shown in figure below. We note that simultaneous highlighting of the variable is possible at the output.

<img width="1274" alt="Screenshot 2023-08-19 at 10 33 45 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/2adf559c-45f2-41f4-9f19-2602b09266a4">

## Lab On Understanding Usage Of Vector

<img width="1290" alt="Screenshot 2023-08-20 at 2 31 28 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/83c2d94c-ac46-40e0-9c78-fbe64a7669f6">

## Multiplexer on Makerchip IDE

<img width="1290" alt="Screenshot 2023-08-20 at 2 30 47 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/5094aaab-ad29-45fe-992d-4215f90eea83">

## Calculator on Makerchip IDE

Now a lab on combinational calculator is implemented that can perform +, -, *, / on two input values. The snapshot of the code, waveform and diagram is as shown below.

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

The sequential calculator is implemented where the output of the previous stage serves as the input of this stage. The snapshot of the sequential calculator is included below and the code is provided 

<img width="733" alt="Screenshot 2023-08-20 at 12 22 10 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/32959f2b-0dac-42f4-a178-9e0322df3b09">

```
$val1[31:0] = >>1$out[31:0];
         $val2[31:0] = $rand2[3:0];
         $op[1:0] = $rand3[1:0];
   
         $sum[31:0] = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
   
         $out[31:0] = $reset ? 32'b0 : (($op[1:0]==2'b00) ? $sum :
                                       ($op[1:0]==2'b01) ? $diff :
                                          ($op[1:0]==2'b10) ? $prod : $quot);
   
   `BOGUS_USE($out);
   `BOGUS_USE($reset);

```

Output:

![Screenshot 2023-08-20 at 2 12 47 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/8749d857-1971-40a9-b9ac-f44168bbba09)


</details>
<details> <summary > Pipelining </summary>
	
Pipelining or timing abstract is an important feature in TL verilog as it can be implemented very easily with fewer codes as compared to system verilog which reduces bugs to a great extent. An example of the pipeling for pythogoras theorem using both TL verilog and system verilog in this repo . In TL verilog pipeling can be implemented by defining the pipeline as |calc and the different pipeline stages should be properly align and are indicated by @1, @2 and so on.

## Lab To Compute Total Distance 

![Screenshot 2023-08-20 at 3 05 33 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/17c91005-4a6a-4425-923f-40228d54ae90)


In the above implenation, we can observe the errors in the pipeline:

<img width="1332" alt="Screenshot 2023-08-20 at 2 09 18 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/0b4f0017-c51d-4c5f-84d2-e625987694ae">

## Lab On Counter and Calculator in Pipeline

Block diagram : 

<img width="745" alt="Screenshot 2023-08-20 at 3 10 10 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/8ad0d3de-d540-47fc-ad70-9f200127d8aa">

```
   $reset = *reset;
   
   |calc
      @1
         $val1[31:0] = >>1$out;
	 $val2[31:0] = $rand2[3:0];

         $sum[31:0] = $val1+$val2;
         $diff[31:0] = $val1-$val2;
         $prod[31:0] = $val1*$val2;
         $quot[31:0] = $val1/$val2;

         $out[31:0] = $reset ? 0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
         
         $cnt[31:0] = $reset ? 0 : >>1$cnt + 1; 

```

Output:

<img width="1270" alt="Screenshot 2023-08-20 at 3 11 50 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/422d4563-1faa-4f36-ac92-8b9812d9d70a">

## Lab On 2 Cycle Calculator

Below the snapshot of the pipeline sequential calcuator is included. Here the first pipeline stage consists of the input followed by arithimetic operation in the second pipeline stage and finally the ouput is included 2 cycles ahead in the third pipeline stage.

```
|calc
      @1
         $reset = *reset;
         
         
         $val1[31:0] = >>2$out[31:0];
         $val2[31:0] = $rand2[3:0];
         $op[1:0] = $rand3[1:0];
         $sum[31:0] = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
         
         $num = $reset ? 0 : >>1$num+1;
      @2   
         $out[31:0] = ($reset|!$num) ? 32'b0 : (($op[1:0]==2'b00) ? $sum :
                                       ($op[1:0]==2'b01) ? $diff :
                                          ($op[1:0]==2'b10) ? $prod : $quot);
         
         
         
   
   `BOGUS_USE($out);
   `BOGUS_USE($reset);

```
Block diagram : 

<img width="750" alt="Screenshot 2023-08-20 at 2 30 44 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/1f2a9c07-32e3-4215-8916-8cf76ba03c1f">


Output:

<img width="1319" alt="Screenshot 2023-08-20 at 2 49 17 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/8960b92f-8bd8-474f-a930-aa75572d6d82">
 
</details>

<details>
	<summary>Validty </summary>

Validity is another feature in TL verilog which is asserted if a particular transactions in a pipeline is valid or true. A new scope, called “when” scope is introduced for this and it is denoted as `?$valid`. This new scope has many advantages - easier design, cleaner debug, better error checking and automated clock gating.
Validity provides :
- Easier debug
- Cleaner design
- Better error checking
- Automated Clock gating
  
# Clock gating

-Why clock gating?

• Clock signals are distributed to EVERY flip-flop.
• Clocks toggle twice per cycle.
• This consumes power.

- Clock gating avoids toggling clock signals.
- TL-Verilog can produce fine-grained gating (or enables).

## Lab On Distance Accumulator with Pythagoran's theorem:

Block diagram :

<img width="800" alt="Screenshot 2023-08-20 at 4 16 23 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/0aefad73-b238-4cd7-8d01-472ff9d53a15">

```
$reset = *reset;

   |calc
      @1
         $reset = *reset;
            
      ?$vaild      
         @1
            $aa_seq[31:0] = $aa[3:0] * $aa;
            $bb_seq[31:0] = $bb[3:0] * $bb;;
      
         @2
            $cc_seq[31:0] = $aa_seq + $bb_seq;;
      
         @3
            $cc[31:0] = sqrt($cc_seq);
            
      @4
         $total_distance[63:0] = 
            $reset ? '0 :
            $valid ? >>1$total_distance + $cc :
                     >>1$total_distance;
         
```

Output:

<img width="1295" alt="Screenshot 2023-08-20 at 5 36 41 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/b7700db5-cd2c-4f23-975b-edbcfed9b571">


## Lab on cycle Calculator with validity 

Block Diagram :

<img width="699" alt="Screenshot 2023-08-20 at 4 31 53 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/38d83577-ec28-4dc0-b5af-77299d6bf3d0">

```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $quot[31:0] = $val1 / $val2;
            
         @2   
            $out [31:0] = $reset ? 32'b0 :
                          ($op[1:0] == 2'b00) ? $sum :
                          ($op[1:0] == 2'b01) ? $diff :
                          ($op[1:0] == 2'b10) ? $prod :
                                                $quot ;
            
```

Output:

<img width="1295" alt="Screenshot 2023-08-20 at 5 29 11 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/eaed8689-1adc-432f-80c0-8b14bb6a7498">

## Lab on Calculator with single value memory:

Block diagram :

<img width="733" alt="Screenshot 2023-08-20 at 5 40 20 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/53f28899-8504-4279-ad9e-0ae191fcf70b">

```
 |calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out;
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $quot[31:0] = $val1 / $val2;
            
         @2   
            $mem[31:0] = $reset ? 32'b0 :
                         ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
            
            $out [31:0] = $reset ? 32'b0 :
                          ($op[2:0] == 3'b000) ? $sum :
                          ($op[2:0] == 3'b001) ? $diff :
                          ($op[2:0] == 3'b010) ? $prod :
                          ($op[2:0] == 3'b011) ? $quot :
                          ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;
            
            
```

Output:

<img width="1295" alt="Screenshot 2023-08-20 at 5 41 24 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/218bbb95-5f88-4e15-b471-4f933451a5ae">

</details>

<details>
	<summary>Wrap-Up</summary>
Here we gonna jst explore some examples given in makerchip ide 

## Conway Game Of Life

Here we can study Hierarchy Concept

![Screenshot 2023-08-20 at 5 52 33 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/11e8c3d3-8f62-4ef5-af41-c111f425ee4b)

## Pythagoran's theorem:

Block Diagram:

<img width="675" alt="Screenshot 2023-08-20 at 5 55 35 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/dcbe146d-edca-4b94-9a52-ab3cfec1e04f">

```
   |calc
      
      // DUT
      /coord[1:0]
         @1
            $sq[9:0] = $value[3:0] ** 2;
      @2
         $cc_sq[10:0] = /coord[0]$sq + /coord[1]$sq;
      @3
         $cc[4:0] = sqrt($cc_sq);


      // Print
      @3
         \SV_plus
            always_ff @(posedge clk) begin
               \$display("sqrt((\%2d ^ 2) + (\%2d ^ 2)) = %2d", /coord[0]$value, /coord[1]$value, $cc);
            end


```
Output:

<img width="1289" alt="Screenshot 2023-08-20 at 5 57 23 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/41c27397-3d7e-4489-8650-d6b182f0a0a4">

</details>

# Day 4-Basic RISC-V CPU micro-architecture

<details>
<summary>Basic RISC-V CPU microarchitecture </summary>
	
The block diagram of a basic RISC-V microarchitecture is as shown in figure below. Now, using the Makerchip platform the implementation of the RISC-V microarchitecture or core is done. For starting the implementation a starter code present [here](https://github.com/stevehoover/RISC-V_MYTH_Workshop) is used. The starter code consist of -

- A simple RISC-V assembler.
- An instruction memory containing the sum 1..9 test program.
- Commented code for register file and memory.
- Visualization.

![RISCV-BD](https://user-images.githubusercontent.com/63381455/123661532-3e5afb80-d852-11eb-8ab9-55629049586b.png)
</details>

<details>
	<summary> Fetch and Decode </summary>
	
Designing of processor is based on three core steps fetch, decode and execute :

Here we gonna design RiscV Cpu Core for which block diagram is given below :

![Screenshot 2023-08-20 at 6 20 34 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/eafc0b5c-d4f8-4c56-a5b6-986421bbb3a0)


## Template For Running Viz:

```
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;



      // YOUR CODE HERE
      // ...

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      //m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
      //m4+myth_fpga(@0)  // Uncomment to run on fpga

   //m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
\SV
   endmodule
```

## PC Logic 

The Program Counter, often referred to as the "PC," is a fundamental component of a processor that keeps track of the address of the next instruction to be executed. In the RISC-V architecture, the PC is typically called "pc" or "pc_reg." Overall, the PC logic is crucial for the control flow of a program. It determines which instruction will be executed next and how the program progresses. RISC-V, as a RISC (Reduced Instruction Set Computer) architecture, emphasizes simplicity and regularity in its design, which extends to its PC handling mechanisms.

<img width="1101" alt="Screenshot 2023-08-20 at 6 24 39 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/0bdcc243-3ae6-4d80-8e50-5cdc03401181">

```
|cpu
      @0
         $reset = *reset;
         
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
```
Output:

<img width="1278" alt="Screenshot 2023-08-21 at 10 03 22 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/3b837e42-ed4c-4dd7-82ed-7621f84aac83">


## Fetch

During the fetch stage, processors fetches the instruction from the memory to the address pointed by the program counter. The program counters holds the address of the next stage, in our case it is after 4 cycle and the instruction memory holds the set of instruction to be executed. The snapshot of the fetch stage is shown below.

Fetch Block diagram part 1:

<img width="1158" alt="Screenshot 2023-08-20 at 6 32 23 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/a3d0eb41-2f8c-4592-a73f-0a877ba48728">

```
|cpu
      @0
         $reset = *reset;
         
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                      
```

Output:

<img width="1278" alt="Screenshot 2023-08-21 at 10 07 20 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/2c64927b-f1c2-43c1-b2ba-7df46d83c77f">

VIZ:
![Screenshot 2023-08-21 at 10 07 49 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/86834280-15b6-4019-8dad-5fbd90ac1760)


Correct fetch Block Diagram :

![Screenshot 2023-08-20 at 6 36 42 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/8d8dcddd-235d-4217-8c08-14448a8403a1)

```
|cpu
      @0
         $reset = *reset;
         
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
      @1 
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;   
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4) 
```
Output:

<img width="1278" alt="Screenshot 2023-08-21 at 10 12 58 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/235044b7-cf7a-4236-9f93-4dd4eac75e7e">

VIZ:

<img width="596" alt="Screenshot 2023-08-21 at 10 13 38 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/3b241c11-7bc8-4443-a348-e7c0b06fed10">

## Decode

For decoding a particular instruction, it is necessary that the isntruction type and format is known to the processor. The decoding is a crucial part and has to be done properly according to the given format to avoid error. There are 6 instructions type in RISC-V :

1. Register (R) type 
2. Immediate (I) type
3. Store (S) type
4. Branch (B) type
5. Upper immediate  (U) type
6. Jump (J) type

<img width="1253" alt="Screenshot 2023-08-20 at 7 09 19 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/af7645fb-fa15-47bc-9197-9278ab430cae">

Following the decoding of the above, the instruction immediate decode for all the above, except the register type is added. The 6 others instruction format/fields including the opcode, 2 source register, destination register, funct3 and funct7 decode is included. Next the instruction field decode of the different instruction type is inserted to ensure that only valid registers are used. Finally the base instruction set decode for the various fields is incorporated. 

![instr_format](https://user-images.githubusercontent.com/63381455/123752003-009fb680-d8d6-11eb-8b8e-874c4b1a4872.png)

Instruction Type  Decode Block diagram:

![Screenshot 2023-08-20 at 6 48 21 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/d24d89b4-258b-46d8-90b5-bc997e29f24f)

```
@1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
```

Output:

<img width="1280" alt="Screenshot 2023-08-21 at 10 20 04 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/ce459fab-9caf-4ebb-850c-3d88baf64adf">

Viz:

<img width="596" alt="Screenshot 2023-08-21 at 10 17 00 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/3f1b4f48-387f-4299-a5f2-70dfd68f350f">


## LAB ON INSTRUCTION IMMEDIATE DECODE

![Screenshot 2023-08-20 at 7 32 33 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/d7e6dfe3-38a7-433a-81d1-89b099f3095c)

```
		      $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
```

Output:

![Screenshot 2023-08-21 at 10 22 37 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/c9af72f4-d925-483e-9c2f-017728fe6142)

Viz:

![Screenshot 2023-08-21 at 10 23 02 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/d93abfd8-bc2f-4a94-85d3-edba9a8ed98e)


## LAB ON INSTRUCTION DECODE

<img width="1156" alt="Screenshot 2023-08-20 at 7 33 30 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/e0ee92e2-c2d0-4ad1-9731-ce09d68f7e3f">

```
	 $rs2[4:0] = $instr[24:20];
         $rs1[4:0] = $instr[19:15];
         $rd[4:0]  = $instr[11:7];
         $opcode[6:0] = $instr[6:0];
         $func7[6:0] = $instr[31:25];
         $func3[2:0] = $instr[14:12];
         
```

Output:

<img width="1277" alt="Screenshot 2023-08-21 at 10 37 49 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/a5d77c0b-46bb-45b2-b4aa-197b530b598f">


Viz:

<img width="596" alt="Screenshot 2023-08-21 at 10 37 06 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/5e10a050-43e7-46d1-942b-b5ebca81b446">


## Lab To Decode Instruction Field Based 

![Screenshot 2023-08-20 at 7 40 20 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/9ef22dc5-7ba0-4812-b83e-5b3e8cea031c)

```
	$rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
```

Output:

![Screenshot 2023-08-21 at 10 39 38 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/af83a076-56c9-4733-9c03-3fba9886ea45)


Viz:

<img width="591" alt="Screenshot 2023-08-21 at 10 40 22 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/cf19a0b9-bc57-4f13-b101-a1c365aec30c">


## LAB ON Individual Instruction Decode

```
	 $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
```

<img width="819" alt="Screenshot 2023-08-20 at 7 52 58 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/f58b523d-46b6-40a6-87b8-3d3cf09083c7">

Output:

<img width="1275" alt="Screenshot 2023-08-21 at 10 45 24 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/6bd2abb3-d6ab-4246-b13a-a90880711e36">

Viz:

<img width="591" alt="Screenshot 2023-08-21 at 10 44 41 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/938aec26-7218-4a1b-8175-a304fdc7e862">
 
</details>
<details>
	<summary> Risc-V Control Logic</summary>
	
## Execute and Register file read/write

The next task is to 'read from' and 'write into' the registers. In this operation, 2 read and write operation can be carried out simulatenously. The two `src1_value`/`src2_value` takes input from the two read register `rf_read_data1`/ `rf_read_data2` and pass it on to the ALU unit. At present, `ADDI` and `ADD` is execute whose result is obtained in register `rf_write_data`. The figure below shows the input and output registers.

![register](https://user-images.githubusercontent.com/63381455/123816062-a245f880-d914-11eb-952a-214868287994.png)

code:
```
	 $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
```

The snapshot of the REGISTER FILE READ operation is included below:

<img width="1275" alt="Screenshot 2023-08-21 at 10 50 40 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/a6fe1d80-e161-4e0c-bae8-6e581358727b">

## Lab on Changes on Register File Read

![Screenshot 2023-08-20 at 9 20 54 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/8ad72456-bf73-4d65-bec3-a23b4132d370)

code:
```
	 $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1; //new
         $src2_value[31:0] = $rf_rd_data2;
```

The makerchip implementation output:

<img width="1275" alt="Screenshot 2023-08-21 at 11 10 47 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/4145043d-9bda-4716-a3d4-77ba334284fe">

Viz:

![Screenshot 2023-08-21 at 11 12 17 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/2432b5ee-cb08-4412-95c3-dcd4915d41a0)

## Lab On ALU

<img width="1268" alt="Screenshot 2023-08-20 at 9 25 26 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/3b464ec5-2d66-49db-907c-3170122b32bf">

code:
```
$result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
```
Implementation:

![Screenshot 2023-08-21 at 11 14 58 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/5d98a691-3cb9-4a3d-920d-7350c07535cc)

Viz:

<img width="597" alt="Screenshot 2023-08-21 at 11 14 05 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/2f297f46-e3c1-40ea-91ea-bcf60dc4dafa">

## Lab On Register File Write

![Screenshot 2023-08-20 at 9 28 40 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/81ad9def-7a13-45c2-9761-6bc789c493ab)

code:
```
	 $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
```
Implementation:

<img width="1279" alt="Screenshot 2023-08-21 at 11 22 46 AM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/3d4f886a-7a88-4118-8f80-ecf66c1e6cc6">

## ARRAYS

In RISC-V, arrays are typically implemented using a combination of registers and memory. Arrays can be stored in memory, where each element is stored at a specific memory address. The processor can use load and store instructions to access these elements. For example, you might load an element from an array in memory into a register, perform operations on it, and then store the result back into memory.

Alternatively, arrays can also be partially stored in registers. If an array is small enough to fit into the available registers, the elements can be loaded into registers for faster processing. This approach can improve performance for certain operations, as register access is generally faster than memory access.

In both cases, whether an array is stored in memory or registers, the RISC-V architecture provides instructions to perform operations on array elements, such as loading, storing, and arithmetic operations.

In summary, the register file in RISC-V architecture consists of a set of registers that are used for temporary storage and fast access to data, while arrays are collections of elements that can be stored either in memory or registers and are manipulated using load, store, and computation instructions.

![Screenshot 2023-08-20 at 9 34 17 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/c42ace43-59fb-4219-a6ae-5e66f1f87c9b)

## Lab For Implementing Branch Instructions

The next stage in the building of the RISC-V microarchitecture, is the addition of branches. Apart from immediate addition or addition, there may certain other conditions to be satisfied which requires to direct the PC to the branch target address. Now, we have simply added few branch instruction and updated the PC. 
Some Branching Instructions are :

- BEQ;== 
- BNE:!=
- BLT: (x1 < ×2) ^ (x1[311!=×2[311)
- BGE: (×1 >= ×2) ^ (x1[31]!=×2[31])
- BLTU: <
- BGEU: >=

code:
```
$taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         
```

![Screenshot 2023-08-21 at 11 22 46 AM](https://github.com/alwinshaju08/RISCV/assets/69166205/314825b2-59ce-4544-aab6-55f1cde5d193)

## Lab For Complementing Branch Instructions

Block Diagram:

<img width="1120" alt="Screenshot 2023-08-21 at 12 02 42 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/0e86494f-052b-431a-bdba-d96e7b0448cf">

code:

```
	   //BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         
         //BRANCH INSTRUCTIONS 2
         $br_target_pc[31:0] = $pc +$imm;
```

Output:

<img width="1284" alt="Screenshot 2023-08-21 at 12 05 44 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/3f405b45-466e-4a47-8031-aaf491885677">

## Lab For Testbench

code:
```
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;

```
<img width="1284" alt="Screenshot 2023-08-21 at 12 10 50 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/c2a6784d-fc3a-44a0-8d88-eb405e82438d">

</details>

# Day 5-Complete Pipelined RiscV CPU Micro-architecture

<details>
<summary> Pipelining the CPU </summary>
	
Now pipelining of the CPU core is done, which allows easy retiming and reduces functional bug to a great extent . Pipelining allows faster computaion. For pipelining as mentioned earlier we simply need to add @1, @2 and so on. The snapshot of the pipelining is as shown below. In TL verilog, another advantage is defining of pipeline in systematic order is not necessary. More inforamtion on timming abstract can be found in the IEEE paper "Timing-Abstract Circuit Design in Transaction-Level Verilog"  by Steeve Hoover in makerchip platform itself or else [here](https://ieeexplore.ieee.org/document/8119264).

## Lab on 3 cycle valid signal 

Block Diagram:

<img width="648" alt="Screenshot 2023-08-21 at 3 23 52 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/44bc20db-bfe8-47ec-b216-ccef5973ec94">

code:
```
	 $start = >>1$reset && !$reset;
         $valid = $reset ? 1'b0 : ($start || >>3$valid);
         $valid_or_reset = $valid || $reset;
```
Output:

<img width="1284" alt="Screenshot 2023-08-21 at 3 23 03 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/5a267b30-8b88-458c-89be-b8961f7b13b9">

## Lab for generating valid signals for each instruction

code:
```
//Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
```

Below is snapshot of pipelined CPU with a test case of assembly program which does summation from 1 to 9 then stores to r10 of register file. In snapshot r10 = 45. Test case:

```
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9);

```

<img width="1267" alt="Screenshot 2023-08-21 at 9 38 05 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/5b22d3fb-81c3-4724-b716-30535c9c27f0">

</details>

<details>
	
<summary> Load, store and data memory </summary>

The load and store option is also included for which a 1 read/write data memory is added. Similar to branch instruction the load also has 3 cycle delay. For checking the functionality of load and store instructions a test bench is added and the data is on address 4 of Data Memory and loaded that value from Data Memory to r17. 

**Code For Lab For Register File Bypass To Address Rd-After-Wr Hazard**

```
//Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
```
**Code For Branches To Correct The Branch Target Path**

```
 //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid 	|| >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
```

Added test case to check fucntionality of load/store. Stored the summation of 1 to 9 on address 4 of Data Memory and loaded that value from Data Memory to r17.

```
*passed = |cpu/xreg[17]>>5$value == (1+2+3+4+5+6+7+8+9);

```
Below is snapshot from Makerchip IDE after including load/store instructions:

![Screenshot 2023-08-21 at 10 10 59 PM](https://github.com/alwinshaju08/RISCV/assets/69166205/56deb1f6-dd84-4377-b4a5-1869300a4f3a)

 
</details>

<details>
	<summary>
		Completing the RISC-V CPU
	</summary>
Finally the jump instruction is added and the decode instuction and the ALU for the entire RV32I base integer set is completed.

code:

```
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   // URL for this code : http://makerchip.com/sandbox/02kfkh98q/0oYhr39#
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```
Below is the image:

<img width="1273" alt="Screenshot 2023-08-21 at 10 38 24 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/1114608e-aba1-4c87-bd02-49f56d8af886">

<img width="898" alt="Screenshot 2023-08-21 at 10 14 47 PM" src="https://github.com/alwinshaju08/RISCV/assets/69166205/ee5e2203-22c0-473a-9f9a-9276fc8343e6">

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
- https://redwoodeda.com
- https://ieeexplore.ieee.org/document/8119264
