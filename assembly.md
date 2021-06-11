

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





IA32 REGISTERS

[ %eax		    %ax[[ %ah	    ][ %al	   ]]]
[ %ecx		    %cx[[ %ch	    ][ %cl	   ]]]
[ %edx		    %dx[[ %dh	    ][ %dl	   ]]]
[ %ebx		    %bx[[ %bh	    ][ %bl	   ]]]
[ %esi		    %si[			    ]]
[ %edi		    %di[			    ]]
[ %esp		    %sp[			    ]]	stack pointer
[ %ebp		    %bp[			    ]]	frame pointer

4 bits == one hex digit
8 bits == one byte == two hex digits
32 bits == four bytes == 8 hex digits


mov
add	src,dest    d = d + s
sub	src,dest    d = d - s
imul	src,dest    d = d * s
sal	src,dest    d = d << s
sar	src,dest    d = d >> s	// arithmetic
shr	src,dest    d = d >> s  // logical
xor	src,dest    d = d ^ s
and	src,dest    d = d & s
or	src,dest    d = d | s












