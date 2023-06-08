# CHEATSHEET

DECIMAL		BINARY		HEX
-------------------------------------------------
	0		0000		0
	1		0001		1
	2		0010		2
	3		0011		3
	4		0100		4
	5		0101		5
	6		0110		6
	7		0111		7
	8		1000		8
	9		1001		9
	10		1010		A
	11		1011		B
	12		1100		C
	13		1101		D
	14		1110		E
	15		1111		F


one bit = one binary digit
one hex digit = 4 binary digits = 4 bits
two hex digits = 8 binary digits = one byte
1024 bytes = kilobyte
1024 kilobytes = megabyte
1024 megabytes = gigabyte
1024 gigabytes = terabyte
1024 terabytes = petabyte
	...then exabyte, zettabyte, yottabyte

GUARANTEED RANGES FOR C DATA TYPES:
		MIN				MAX					BYTES
------------------------------------------------------------------------------------
char		-128				127					1
U-char		 0				255					1
short int	-32768				32767				2
U-short int	 0				65535				2
int		-32768				32767				2
U-int		 0				65535				2
long int	-2,147,483,648			2,147,483,647			4
U-long int	 0				4,294,967,295			4
long long int	-9,233,372,036,854,775,808  9,233,372,036,854,775,807		8
U-long long int	 0				18,446,744,073,709,551,615		8



FOR X86-64 ARCHITECTURE:
C DECLARATION	SIZE		INTEL		ASSEMBLY CODE
		(BYTES)		DATA TYPE		SUFFIX
------------------------------------------------------------------------------------
char			1		byte				b
short			2		word				w
int				4		double word			l
long int		4		double word			l
long long int	4		---					-
char *			4		double word			l
float			4		single precision	s
double			8		double precision	l
long double		10/12   extended precision	t

unsigned		4



IA32 REGISTERS

[ %eax			%ax[[ %ah		][ %al	   ]]]
[ %ecx			%cx[[ %ch		][ %cl	   ]]]
[ %edx			%dx[[ %dh		][ %dl	   ]]]
[ %ebx			%bx[[ %bh		][ %bl	   ]]]
[ %esi			%si[				]]
[ %edi			%di[				]]
[ %esp			%sp[				]]	stack pointer
[ %ebp			%bp[				]]	frame pointer

4 bits == one hex digit
8 bits == one byte == two hex digits
32 bits == four bytes == 8 hex digits



# COMPILING CODE

preprocessor	 compiler	 assembler	 linker
[file.c] --> [file.c] --> [file.s] --> [file.o] --> [file]
 <text>		  <text>	 <assembly>	<binary>	   <binary>


#include <stdio.h>
#include <stdint.h>

## GCC FLAGS:
-o: specify output file name
	gcc -o output file.c
-Wall: gives better warnings
	gcc -Wall file.c
-g: allow debuggin with gdb
	gcc -g file.c
-O: turn on optimization
	gcc -O file.c
-lXXX: look for library XXX.so or XXX.a
-c: just create an object file, not an executeable
	* useful for linking several object files together that are compiled separately
	gcc -c -Wall file.c
	gcc -c -Wall otherfile.c
	gcc -o total file.o otherfile.o -lm
	* -lm links the two .o files together (i think)
	* -Wall is only necessary in the compile stage

## A SIMPLE MAKEFILE:

hw: hw.o helper.o
	gcc -o hw hw.o helper.o -lm

hw.o: hw.c
	gcc -O -Wall -c hw.c

helper.o: helper.c
	gcc -O -Wall -c helper.c

clean:
	rm -f hw.o helper.o hw

/*

o type 'make' to create the executable
o type 'make clean' to run the clean function
o make automatically knows which .o files need to be recomplied and won't recompile
	files that haven't been changed.
*/




