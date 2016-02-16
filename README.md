**Winter 2016**
**COMP-250: Introduction to Computer Science**


Algorithms
==========
#####Informal Definition
An algorithm is the specification of a sequence of instructions to be carried out by a processor.

Algorithms can be run on a computer, but not necessarily:

* The Mayans had algorithms to predict solar eclipses centuries in advance.
* Egyptians had algorithms to build pyramids.
* Indians had algorithms for factorizing polynomials.
* Greeks had algorithms to build all kinds of geometric constructions using only a compass and straight-edge.

An example of a compass and straight-edge construction:

    e.g. Compass and straight-edge construction
    Problem: angle bisection
        Input: An angle defined by three points AOB
        Output: A point C such that AOC = BOC
    Algorithm:
        Draw circle centered at O to find A' and B'
        Draw circles centered at A' and B' of the same radius to find C
        AOC and BOC will then bisect AOB



We will mostly be concerned with algorithms that can be run by computers, since they are much faster than we are.

Computer Science
----------------
####Definition
Computer science is the study of algorithms for computing machines.

####Formal definition of an algorithm
A well-ordered collection of unambiguous effectively computable operations that when executed produces a result and halts in a finite amount of time.

####What distinguishes computer algorithms?

* Instructions are executed very quickly
* Algorithms must be fully specified before execution
* Algorithms must be unambiguous

Grade School Algorithms
-----------------------
* Unary addition: taking a set A and a set B, their sum C will be the union of A and B. You have to count everything out individually.
* Unary multiplication: taking a set A and a set B, their product B will be the substition of each element of set A by a set B'.

Unary representation is quite inefficient. It works well for addition of small sets because it's easy to visualize, but once sets get too large, your representation will contain way too many characters.

Numeral representations, such as the Roman numerals, came up with more efficient representations to describe larger numbers. However, since the largest character M only represents 1000, it is hard to represent numbers that are of orders of magnitude larger. Furthermore, the romans didn't even have a zero in their system, which means that addition was impossible. Adding 0 expanded how mathematics could work.

####Algorithm 1: Addition

Using a base-10 addition table:

![Addition Table](http://i.imgur.com/BigcVCd.png)

**Addition (base 10): Add two _N_ digit numbers _a_ and _b_ represented by an array of digits.**

	carry = 0
	for i = 0 to N-1 do 
		r[i]  <-- R[a[i],b[i],carry] // only keeps track of the right-most digit of the sum.
		carry <-- L[a[i],b[i],carry] // only keeps track of the left-most digit of the sum.

		/* 
			Note that:
			R[a[i],b[i],carry] = (a[i]+b[i]+carry) % 10
			L[a[i],b[i],carry] = (a[i]+b[i]+carry) / 10
		*/

	end for
	r[N] <-- carry

Example:

	2343+4519=?

	N = 4
	a[]:   [2 3 4 5]
	b[]:   [4 5 1 9]
	r[]: [0 0 0 0 0] 
	/*r[] has one more index because adding 2 numbers of equal magnitude 
	can only lead to a result that's 1 magnitude higher.*/

	The first iteration of the loop would look like this:
		r[0] = R[a[0],b[0],0] = (5 + 9 + 0) % 10 = 14 % 10 = 4
		carry = L[a[0],b[0],0] = 14 / 10 = 1

	So your r[] would be updated to look like so:        [0 0 0 0 4]
	Running your iteration would lead to a final r[] of: [0 6 8 6 4]


Generalization to any base B:
	
	R[a[i],b[i],carry] = (a[i]+b[i]+carry) % B
	L[a[i],b[i],carry] = (a[i]+b[i]+carry) / B

A base 8 example:
	
	 (1205)_8
	+ (736)_8
	---------
	 (2143)_8

	 Which is equal to 1123_X (X being the symbol for 10)

####Algorithm 2: Multiplication

Using a base-10 multiplication table:

![Multiplication table](http://i.imgur.com/RTyqZTI.png)

**Multiplication (base 10): Multiply two _N_ digit numbers _a_ and _b_ represented by an array of digits.**

	// Part 1: actually multiplying

	for j = 0 to N - 1 do 
		carry <-- 0
		for i = 0 to N - 1 do //Looking at all digits from 1st number multiplied by 1 digit of the 2nd number

			prod        <-- (a[i] * b[j] + carry)
			tmp[j][i+j] <-- prod % 10 //keeps least significant digit
			carry       <-- prod / 10 //drops least significant digit

		end for
		temp[j][N+j] <-- carry 
	end for	

	// Part 2: summing all the products together

	carry <-- 0 

	for i = 0 to 2 * N - 1 do
		sum <-- carry
		for j = 0 to N - 1 do
			sum <-- sum + tmp[j][i]
		end for
		r[i]  <-- sum % 10
		carry <-- sum / 10
	end for
	r[2*N] <-- carry

Generalizing for any base B:
	
	Replace all modulo and division operations by B instead of 10.

Analysis of Algorithms
----------------------
We analyze the space and time required to run an algorithm. The space used is less important than time.

We do not analyze the time required to run each possible combination of inputs. This would take into account too many cases and would include too many details, rendering our analysis useless.

In reality, we analyze time as a function of the size of the inputs (which is the parameter *N*). For all inputs of the same size, we would have a precise bound.

####Analysis of Addition
			
	carry = 0  //CONSTANT TIME

	for i = 0 to N-1 do //LOOP = LINEAR TIME
		
		r[i]  <-- (a[i]+b[i]+carry) % 10 //CONSTANT TIME
		carry <-- (a[i]+b[i]+carry) / 10 //CONSTANT TIME

	end for

	r[N] <-- carry //CONSTANT TIME


	Given c, c', c''

	c    >= T("carry <-- 0")
	c''  >= T("r[N]<--carry")
	c'   >= T("r[i]..., carry"")
	c'*N >= T("for...")



	Time(N) <= c + c'N + c'' <= c_1 + c_2 * N
	Time(N) is O(N).

The constants c1 and c2 depend on the hardware that you run the algorithm on. The important thing to take home is that this algorithm has linear time, meaning that doubling the inputs will double time. This statement becomes more true for larger input sets.

####Analysis of Multiplication

	// Part 1: actually multiplying

	for j = 0 to N - 1 do 						  //LINEAR TIME
		carry <-- 0 							  //CONSTANT TIME
		for i = 0 to N - 1 do 					  //LINEAR TIME

			prod        <-- (a[i] * b[j] + carry) //CONSTANT TIME
			tmp[j][i+j] <-- prod % 10 			  //CONSTANT TIME
			carry       <-- prod / 10 			  //CONSTANT TIME

		end for
		temp[j][N+j]  				     		  //CONSTANT TIME
	end for	

	// Part 2: summing all the products together

	carry <-- 0 

	for i = 0 to 2 * N - 1 do 					  //LINEAR TIME
		sum <-- carry 							  //CONSTANT TIME
		for j = 0 to N - 1 do 					  //LINEAR TIME
			sum <-- sum + tmp[j][i] 			  //CONSTANT TIME
		end for
		r[i]  <-- sum % 10 					      //CONSTANT TIME
		carry <-- sum / 10 						  //CONSTANT TIME
	end for
	r[2*N] <-- carry 							  //CONSTANT TIME

	Time(N) = c_1 + c_2*N + c_3*N^2
	Time(N) is O(N^2)

Since both parts of the algorithm have two nested for loops with linear time, they will be of quadratic time (since N x N = N<sup>2</sup>).

####Big O Notation

This notation drops all lower exponents and constants in Time(N).
For example, addition is O(N) and multiplication is O(N<sup>2</sup>).

Today, the best multiplication algorithm has a running time of O (N x 2<sup>log* N</sup>). This is very close to linear time, as the iterated logarithm (log*) function increases incredibly slowly. Of course, this makes for a much more sophistic algorithm.

Bases and Binary Representation
===============================

####Base 8 to Base 2

(2143)<sub>8</sub> = 2 x 8<sup>3</sup> + 1 x 8<sup>2</sup> + 4 x 8<sup>1</sup> + 3 x 8<sup>0</sup>

Since 8 = 2<sup>3</sup>, you can simply convert the digit in base 8 to its binary representation. Each digit of 8 is equivalent to three binary digits. Thus, the conversion can be done by only local operations.

(2143)<sub>8</sub> = (010 001 100 011)<sub>2</sub> = (010001100011)<sub>2</sub>

An equivalent process is done to convert binary numbers to the much more readable hexadecimal base unit.

####Base 2 to Base 10

(10010010101010101)<sub>2</sub> = (75093)<sub>10</sub>

We take each power of 2 in base 10, multiply them by their binary digit, and sum the digits up.

	Powers of 2 in base 10:

	2 ** 0  = 1     |   2 ** 11 = 2'048 
	2 ** 1  = 2     |   2 ** 12 = 4'096
	2 ** 2  = 4     |   2 ** 13 = 8'192
	2 ** 3  = 8     |   2 ** 14 = 16'384
	2 ** 4  = 16    |   2 ** 15 = 32'768 
	2 ** 5  = 32    |   2 ** 16 = 65'536
	2 ** 6  = 64    |   2 ** 17 = 131'072
	2 ** 7  = 128   |   2 ** 18 = 262'144
	2 ** 8  = 256   |   2 ** 19 = 524'288
	2 ** 9  = 512  	|   2 ** 20 = 1'048'576
	2 ** 10 = 1'024 |   2 ** 21 = 2'097'152

This process is special to base 2 because we only have two options for each digit: included or excluded from the sum. Doing the same process in the other direction much more complicated operations, since include/exclude aren't the only operations. You can have from 0 to 9 times of each digit.

####Algorithm 3: Convert integer to binary

**Input: a number _n_**

**Output: the number _m_ expressed in base 2 using a bit array _b[]_**

	i <-- 0
	while m > 0 do
		b[i] <-- m%2
		m    <-- m/2
		i    <-- i+1
	end while

Example:

	75093 = 0b10101010111001000

	/2    %2  | /2    %2
	----- --  | ----- --
	37546  1  | 73     1
	18773  0  |	36     1
	9386   1  | 18     0
	4693   0  | 9      0
	2346   1  | 4      1
	1173   0  | 2      0
	586    1  | 1      0
	293    0  | 0      0
	146    1  |

Why does it work? 

	Note how any number can be represented as:
	m = b * (m/b) + m%b

	Since integer division of a number drops the remainder. When you add the remainder back, you get the original number again.

Generalizing for Base B:

	Replace 2 by B.

####Fractional numbers

Example:

26.375 = (11010.011)<sub>2</sub>

The integer portion of the number can be calculated separately from the fractional part. The latter can be found by multiplying negative powers of 2.

	0.375*8/8
	= 3/8
	= 11/1000
	= 1.1/100
	= 0.11/10
	= 0.011/1
	= 0.011

Certain fractions yield infinite decimal numbers. You can separate the fraction into a non-repeating part and a repeating part.

####Binary representation in computers

Computer memory does not expand or retract depending on the size of the data, so we always work with a fixed size of memory. The smallest unit of memory is a _byte_. A byte contains 8 bit registers for memory. 

Data representation on a byte is circular because adding to the maximum value of (11111111)<sub>2</sub> resets the number to 00000000 (which is equivalent to all operations being modulo 256).

* Unsigned representation: Each byte contains a positive value from 0 to 255 (2<sup>8</sup>-1).
* Signed representation: Each byte contains a value from -128(-2<sup>7</sup>) to 127(2<sup>7</sup>-1). The greatest bit is used to tell the sign of the number.

Here is a table of binary numbers:

	Binary     Signed   Unsigned

	00000000   0		0
	00000001   1		1
	. 		   .	    .
	.          .		.
	01111111   127	    127
	10000000   128	   -128
	10000001   129	   -127
	.		   .        .
	.		   .        .
	11111111   255     -1

On modern computers, we use more than one byte to represent our numbers. Thus, the bounds of our numbers would change depending on the amount of bits allocated to a number.

Interesting example of the rollover:

``` java

//This loop runs to infinity!
for (short s = 32767;s<32768;s++){
	System.out.println(s);
}
	/*Output:

	 32767
	-32768
	-32767
	   ...
	     0
	     1
	   ...
	 32766
	 32767
	   ...
	*/
```
In our computer memory, we store the location of our bytes in addresses. Address size varies depending on the number of bits of your computer.

####How this works in Java

In Java, we use different amounts of bytes for different primitive types.

	Boolean 00000000 								(1 byte)

	Byte    00000000 								(1 byte)

	Char    00000000 								(1 byte)

	Short   00000000 00000000 						(2 bytes)

	Int     00000000 00000000 00000000 000000000	(4 bytes)

	Long    00000000 00000000 00000000 000000000	(8 bytes)
	     	00000000 00000000 00000000 000000000

	Float   00000000 00000000 00000000 000000000	(4 bytes)

	Double  00000000 00000000 00000000 000000000	(8 bytes)
            00000000 00000000 00000000 000000000

The address keeps track of the first byte, since the bytes used are adjacent to each other. For instance, the address of an integer will only track the first byte and will automatically know that the other three bytes are adjacent.

Array Algorithms
================

####Algorithm 4: Insertion Sort

**Input: an array a[] with N elements that can be compared (<,=,>).**

**Output: the array a[] containing the same elements in increasing order.)**

	for k = 1 to N-1 do
		tmp <-- a[k]
		i   <-- k
		while (i>0) & (tmp<a[i-1]) do
			a[i] <-- a[i-1]
			i    <-- i-1
		end while
		a[i] = tmp
	end for

Example:

	a:[11|(13),2,5,21,99,12,51] tmp:13 //13 belongs after 11 so we don't move it
	a:[11,13|(2),5,21,99,12,51] tmp:2  //2 is smaller than 11 & 13, so it gets shifted
	a:[2,11,13|(5),21,99,12,51] tmp:5  //5 goes between 2 and 11
	a:[2,5,11,13|(21),99,12,51] tmp:21 //21 is already sorted
	a:[2,5,11,13,21|(99),12,51] tmp:99 //99 already sorted
	a:[2,5,11,13,21,99|(12),51] tmp:12 //12 goes under 13
	a:[2,5,11,12,13,21,99|(51)] tmp:51 //51 goes between 21 and 99

	a:[2,5,11,12,13,21,51,99] //The array is now sorted.

####Analysis of Insertion Sort

The inner while loop will take between constant and linear time depending on how sorted the algorithm is. For example, if the array is already sorted, then the while loop will only have one iteration. If the array is sorted in reverse, then we will have the worst case instead.

Since the outside for loops runs in linear time, the algorithm runs from linear to quadratic time.

Best case: Ω(N) = N
Worst case: O(N) = N<sup>2</sup>

Linked Lists
============

A list is an ordered set of elements.

Adding/removing elements from the beginning of an array is long and time-consuming, because you have to shift all subsequent elements. The same operations performed at the end are a lot easier.

Rather than an array representation, a linked list would work a lot better. Individual cells in these lists contain both a value and a pointer to the next cell. We lose the nice organization of elements that we have in arrays, but there is information that allows us to chain elements together.

The advantage of arrays is that you can go to a specific index just by its index number. With linked lists, you need to start from the head or the tail and work from there.

Singly Linked Lists:

	class SNode{ //an individual node
		Type 		element
		SNode 		next
	}

	class SLinkedList{
		SNode		head;
		SNode		tail;
		int 		size;
	}

	addFirst(newNode){
		newNode.next = head
		head = newNode
		size = size + 1
	}

	removeFirst(){
		//tmp is used to maintain knowledge of the previous head.
		tmp = head
		head = head.next
		tmp.next = null
		size = size - 1
	}

	addLast(newNode){
		tail.next = newNode
		tail = tail.next
		tail.next = null
	}

	removeLast(){
		if (head == tail)
			head = null
			tail = null
			size = 0
		else
			tmp = head
			while (tmp.next != tail){
				tmp = tmp.next
			}
			tmp.next = null
			tail = tmp
			size = size - 1
	}

####Java Generics

A way of writing your code that allows you to specify your type right as you use it. The definition of your class would not depend on which element type you input into it.

	Example: Singly linked lists

	class SNode<E>{
		E 			element;
		SNode<E> 	next;
			:
	}

	class SLinkedList<E>{
		SNode<E>	head;
		SNode<E>	tail;
		int 		size;
			:
	}

####Doubly Linked Lists

Contain a link to the previous node as well as the next node. This helps to make navigating your list easier. The drawback of this is that they take more space. Thus, if you don't need to use the increased functionality of doubly linked lists, you should used singly linked lists instead. In essence, a doubly linked list is like having two singly linked lists going in opposite directions. 

	class SNode<E>{
		E 			element;
		SNode<E> 	next;
		SNode<E>	prev;
			:
	}

	class SLinkedList<E>{
		SNode<E>	head;
		SNode<E>	tail;
		int 		size;
			:
	}

One operation that we consider often is finding an element from a Linked List the same way you would extract the element from an array.

	getNode(i){
		if (i<size/2){
			tmp = head;
			index = 0;

			while (index<i){
				tmp = tmp.next;
				index++;
			}
		} else{
			tmp = tail;
			index = size - 1

			while (index>i){
				tmp = tmp.prev;
				index--;
			}
		}
		return tmp;
	}

Another operation is removing a node from a Linked List.

	remove(node){
		node.prev.next = node.next;
		node.next.prev = node.prev;
		size = size - 1;
	}

Note how the above code does not work for the head and tail, because of the lack of next/prev in these nodes. The solution to this is to add a dummy head and a dummy tail to your list structure. (Another solution would be to test for if the node is a head or tail.)

	class DLinkedList<E>{
		DNode<E>		dummyHead;
		DNode<E>		dummyTail;
		int 			size;
	}

####Arrays vs. Linked Lists

	.				array 			S.L.L.			D.L.L.
	.				-----			------			------
	addFirst		N 				1				1
	removeFirst		N 				1				1
	addLast			1 				1 				1
	removeLast		1 				N 				1
	getNode(i)		1				i 				min(i, N/2 - i)

####Linked Lists operations

	add(i,element)	//Inserts element into i-th position
	set(i,element)	//Sets the element at the i-th position to a specific value
	remove(i)		//Removes element from i-th position
	get(i)			//Returns the element from i-th position
	clear()			//Removes all elements from the list
	isEmpty()		//Returns a boolean indicating if the list is empty
	size()			//Returns the size of the list

####Java LinkedList

* Implemented as a doubly linked list.
* Node class is private to avoid messing with the data structure itself such that the operations fail.

Linked list operation speeds:

	add(element)			1
	add(i,element)			n
	set(i,element)			n
	remove(i)				n
	get(i)					n
	clear()					1
	isEmpty()				1
	size()					1

Note how add, set, remove, and get are expensive operations. For instance, if you wanted to print all the elements of a list in order, you would have an O(N<sup>2</sup>) operation. For this reason, you would want to implement some properties of arrays into linked lists, which brings us to...

####Java ArrayList

* Implementation using arrays of growing sizes
* Cannot access using a[i] notation

ArrayLists store the array, the size of the array, and the capacity of the array.
Adding elements is done by creating a new array with double the capacity and adding the new element inside.
The cost of doing so is actually **linear**. The extra cost of creating a new array is proportional to the amount of elements already in the array. The charge is constant per element, so the cost is **amortized constant time**.

To get to an array of 2<sup>k</sup> length, you need to create arrays for all powers of 2 preceding it. The sum of these would actually be 2k.

####LinkedList vs ArrayList

	.						LinkedList		ArrayList
	.						----------		---------
	add(element)			1				1
	add(i,element)			n 				n
	set(i,element)			n 				1 //more efficient
	remove(i)				n 				n
	get(i)					n 				1 //more efficient
	clear()					1 				1
	isEmpty()				1 				1
	size()					1 				1

Abstract Data Types
===================

An Abstract Data Type (ADT) is an abstraction of a data structure: no coding is involved.

It specifies:
* What can be stored in it
* What operations can be done on/by it

There are a lot of formalized and standardized ADTs in Java. They are implemented through an interface.


Stacks
------
A **stack** is a container of objects athat are inserted and removed accoridng to the last-in-first-out principle.
Objects can be inserted at any time, but only this least(most recently inserted) object can be removed.
Inserting an item is known as pushing onto the stack.
Popping off the stack is synonymous with removing an item.

####Methods

Two main methods:

* push(o): Inserts object o onto top of stack.
* pop(): Removes the top object of stack and returns it. If the stack is empty, then an error occurs.


The following methods should also be defined:

* size(): returns the number of objects in stack
* isEmpty(): returns a boolean indicating if sstack is empty
* top(): returns the top object of the stack, without removing it; if the stack is empty then an error occurs.

####Examples

Simple push/pop operations on an empty stack:

	1. push(3)		
	2. push(6)
	3. push(4)
	4. push(1)							1		5
	5. pop()						4	4	4	4	4
	6. push(5)					6	6	6	6	6	6	6
	7. pop()				3	3	3	3	3	3	3	3	
	8. pop()				-	-	-	-	-	-	-	-
	.						1.	2.	3.	4.	5.	6.	7.	8.


Mathematical operations: 

	3 + (4 - x) * 7 + (y - 2 * (2 + x))
	
												(
									(		(	(	(	
								-	-	-	-	-	-	-

1. Push an open parenthesis on the stack.
2. Keep going until you see another opening parenthesis or a closing parenthesis.
3. If you see a closing one, pop the stack. If you see an opening one, you push it onto the stack.

If the stack contains any objects at the end, it means that something was wrong with the parenthesis matching.

To actually calculate this operation, you'd need two stacks:

	3 + (4 - x) * 7 + (y - 2 * (2 + x))

	Operator stack:

																						+	+
																				(	(	(	(	
																			*	*	*	*	*	*
						-	-	-									-	-	-	-	-	-	-	-	-
				(	(	(	(	(		*	*				(	(	(	(	(	(	(	(	(	(	(	
			+	+	+	+ 	+	+	+	+	+	+		+	+	+	+	+	+	+	+	+	+	+	+	+
	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--

	Number stack:

																						3
																				2	2	2	5
							1	1			7						2	2	2	2	2	2	2	10
					4	4	4	4	3	3	3	21				6	6	6	6	6	6	6	6	6	-4
		3	3	3	3	3	3	3	3	3	3	3	24	24	24	24	24	24	24	24	24	24	24	24	24	20
	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--

When an operator wants to be pushed onto the stack, but has lower priority than the top of the stack, the stack gets popped and the operation is performed on the top of the number stack.

This process in pseudocode:

	t = gettoken()
	while type(t)!=eol do
		if type(t)=number then...
		if type(t)=operator then...
		if t="(" then...
		if t=")" then...
		t=gettoken()
	end while

	while not isemptyO() do
		op=popO()
		arg2=popA()
		arg1=popA()
		pushA(exec(arg1,op,arg2))
	end while

	return popA()

	//Processing arithmetics

	if type(t) == number then pushA(t)

	if type(t) == operator then
		if prio(t) <= prio(topO())
			then op = popO()
				 arg2 = popA()
				 arg1 = popA()
				 pushA(exec(arg1,op,arg2))

			pushO(t)

	if t == "(" then pushO(t)

	if t == ")" then
		op = popO()
		while op != "(" do
			arg2 = popA()
			arg1 = popA()
			pushA(exec(arg1,op,arg2))
			op = popO()
		end while

A bunch of different brackets:

	(	(	[	]	)	)	[	]	{	[	]	}


		(
	(	(	(	(	(	
	-	-	-	-	-	-	-	-	-	-	-	-

A bunch of different (unmatching) brackets:

			( 	( 	[ 	) 	] 	) 	[ 	[ 	] 	] 	{ 	[ 	] 	} 
	

							//The stack fails because the element popped from the stack
					[		//doesn't correspond to the next bracket.
				(	(
			(	(	(
		-	-	-	-	x

HTML uses a stack for its markup tags, which transform plain text into what you see on a webpage. 
For instance:

	<HTML><HEAD><TITLE></TITLE></HEAD><BODY><CENTER>...



						<TITLE>							<CENTER>
				<HEAD>	<HEAD>	<HEAD>			<BODY>	<BODY>
		<HTML>	<HTML>	<HTML>	<HTML>	<HTML>	<HTML>	<HTML>
		------	------	-------	------	------	------	-------- ...

####Stacks in the Java Virtual Machine

* Each process running in a Java program has its own Java Method Stack
* Each time a method is called, it is pushed onto the stack.
* The choice ofa stack for this operation allows Java to do several useful things
	- Perform recursive method calls
	- Print stack traces to locate an error

Each item on the stack keeps count of the method information, as well as something called a program counter. This shows the line where the method has been interrupted by another method that was added on top of it in the stack. Once the other method is removed, the program reverts to the line saved in the program counter.

####Application: Time Series

The span *s<sub>i</sub>* of a stock's price on a certain day *i* is the max number of consecutive days (up to the current day) the price of the stock has been less than or equal to its price on day *i*.

![](http://i.imgur.com/QZ32TZ3.png)

There is a straightforward (but inefficient) way to compute the span of a stock on each of *n* days. You can just look back from each day individually. This would cause you to have two loops, meaning that your algorithm would have a time of **O(n<sup>2</sup>)**.

A stack would help make this a lot more efficient. 

We see that *s<sub>i</sub>** on day *i* can be easily computed if we know the closest day preceding *i*, such that the price is greater than on that day than on day *i*. If such a day exists, let's call it *h(i)*. Otherwise, we conventionally define *h(i)*=-1. The span is now computed as *s<sub>i</sub>*=*i*-*h(i)*.

We use a stack to keep track of *h(i)*.

![](http://i.imgur.com/kqy5d4R.png)

Queues
------

* A queue differs from a stack in that its insertion and removal routines follow the first-in-first-out (FIFO) principle.
* Elements may be inserted at any time, but only the element which has been in the queue the longest may be removed.
* Elements are inserted at the rear (enqueued) and removed from the front (dequeued)

The queue has two fundamental methods:

* enqueue(o): Inserts an object o at rear of queue
* dequeue(): Removes object from front of queue and returns it. An error occurs if queue is empty. *An error occurs if queue is empty.*

There are also a couple more optional methods:

* size(): Returns number of objects in the queue
* isEmmpty(): Returns a boolean value that indicates whether the queue is empty.
* front(): Returns (without removing) the element in front of the queue. *An error occurs if queue is empty.*


Example:

	Operation 	State
	---------	-----
	add(a)		a
	add(b)		ab
	remove()	b
	add(c)		bc
	add(d)		bcd
	add(e)		bcde
	remove()	cde
	add(f)		cdef
	remove()	def
	add(g)		defg

Queues are more interesting to implement than stacks because of how the insertion and extraction points are on opposite ends.

####Queues as arrays

You could implement queues in both LinkedLists and Arrays. However, there is no noticeable advantage for having a queue in a list, so it's simpler to just use an array.

To implement it in an array, we store in which indices the Head and the Tail are located. When an element is added, we store it in the next empty index, and the tail now points to the index. Likewise, when an element is removed, we remove the element at the head, and the head now points to the next index.

The queue is only full when it is equal to the size of the array it is contained in.

An array implementation:

	enqueue(element){
		if (size == length)
			increase length of array
		a[(head+size)%length]=element
		size++
	}

	dequeue(){
		out = a[head]
		head = (head+1)%length
		size--
		return out
	}

When the queue array runs out of space, we transfer it into a bigger array:

	create a bigger array
	for i = 0 to length-1
		big[i] = small[(head+i)%small.length]

Running Times and Asymptotic Notations
======================================

Computational Tractability
--------------------------

A quote from Charles Babbage:
>As soon as an Analytic Engine exists, it will necessarily guide the future course of the science. Whenever any result is sought by its aid, the question will arise - By what course of calculation can these results be arrive by the machine in the shortest time?


When we analyze an algorithm, we focus our attention on the time consumed by an algorithm. Computational tractability is the measurement of how easily an operation can be done.

####Brute force
For many non-trivial problems, there is a natural brute force search algorithm that tries every possible solution.

* Typically takes 2<sup>N</sup> time or worse for inputs of size N.
* Unacceptable in practice.

####Desirable scaling property
When the input size doubles, the algorithm should only slow down by some constant factor C. This is equivalent to saying that there exist constants a>0 and d>0 such that on every input of size N, its running time is bounded by a N<sup>d</sup> steps.

**Definition**: An algorithm is **poly-time** if the above scaling property holds.

####Worst case analysis
The worst case is the upper bound on the running time of an algorithm on any input of size N.

* Generally captures efficiency in practice
* Seems draconian, but it's hard to find an effective alternative

####Average case analysis
The average case is the bound on running time of an algorithm on a **random** input as a function of input size N.

* Hard (or impossible) to accurately model real instances by random distribution.
* Algorithm tuned for a certain distribution may perform poorly on other inputs.

####Worst case polynomial-time

**Definition**: An algorithm is **efficient** if its running time is polynomial. This is justified by practice.
**Caveat**: Although 6.02E23 x N<sup>20</sup> is technically poly-time, it would be useless in practice.
In practice, poly-time algorithms that people develop almost always have low constants and low exponents. Breaking through the exponential barrier of brute force typically exposes some crucial structure of the problem.
Exceptions: 

* Some poly-time algorithms do have high constants or exponents, and are useless in practice. 
* Some exponential-time (or worse) algortithms are widely used because the worst-case instances seem to be rare.

Example: Primality Testing

* If you have an N-bit number, it is a 2<sup>N</sup> number. Thus, doing operations to the square root of our number will still yield (sqrt(2))<sup>N</sup> running time.
* There have been improvements that have led to better running times such as log<sup>4</sup>N (Miller). We have not yet found a proof to the conjecture behind Miller's algorithm.
* Another algorithm by Miller's colleague Rabin would give log<sup>3</sup>N, but would be wrong on primes a quarter of the time.
* One guaranteed test discovered has had log<sup>12</sup>N, which is slow to the point of impracticality. Rabin's algorithm would in practice be better because of the insane running time from the foolproof algorithm.

Why does this matter?

![](http://i.imgur.com/OV8chh5.png)

####Comp Sci approach to problem solving

* If my boss formulates a problem to be sovled urgently, can I write a program to efficiently solve this problem?
* Are there some problems that cannot be solved at all? Are there problems that cannot be solved efficiently?

Asymptotic order of Growth and Notation
---------------------------------------

* Upper Bounds: T(n) is O(f(n)) if there exist constants c > 0 and n<sub>0</sub> ≥ 0 such that for all n ≥ n<sub>0</sub> we have T(n) ≤ c x f(n).
* Lower Bounds: T(n) is Ω(f(n)) if there exist constants c > 0 and n<sub>0</sub> ≥ 0 such that for all n ≥ n<sub>0</sub> we have T(n) ≥ c x f(n).
* Tight Bounds: T(n) is Θ(n) if T(n) is both O(f(n)) and Ω(f(n)).

Example: T(n) = 32n<sup>2</sup> + 17n + 32

* T(n) is O(n<sup>2</sup>) since there exists c = 81 and n<sub>0</sub> = 1 such that for all n ≥ 1 we have T(n) ≤ 32n<sup>2</sup> + 17n<sup>2</sup> + 32n<sup>2</sup> = 81n<sup>2</sup>.
* T(n) is Ω(n<sup>2</sup>) since there exists c = 1 and n<sub>0</sub> = 0 such that for all n ≥ 1 we have T(n) ≥ n<sup>2</sup>.

**Note:** Using T(n) = O(f(n)) is wrong because the expression doesn't actually denote equality.

**Meaningless statement:** "Any comparison-based sorting algorithm requires at least O(n log n) comparisons."

* The statement uses upper bound to describe a lower bound.
* The constant function f(n)=1 is O(n log n).
* The statement doesn't type-check.

####Properties

* Transitivity: if f is O(g) and g is O(h), then f is O(h).
* Additivity: if f is O(h) and g is O(h), then f + g is O(h).

Note: These apply for O, Ω, and Θ.

####Frequently used functions

* Polynomials: a<sub>0</sub>+a<sub>1</sub>n+...+a<sub>d</sub>n<sup>d</sup> is Θ(n<sup>d</sup>) if a<sub>d</sub>>0.
* Polynomial time: Running time is O(n<sup>d</sup>) for some constant *d* independent of the input size *n*.
* Logarithms: O(log<sub>a</sub>n) = O(log<sub>b</sub>n) for any constants a,b > 0.
	- For every x > 0, log n is O(n<sup>x</sup>). (i.e. every log grows slower than every polynomial.)
* Exponentials. For every r > 1 and every d > 0, n<sup>d</sup> is O(r<sup>n</sup>). (i.e. every exponential grows faster than every polynomial.)

####Asymptotic notation

Sometimes, one can also obtain an asymptotically tight bound directly by computing a limit as *n* goes to infinity. Essentially, if the ratio of functions f(n) and g(n) converges to a positive constant as *n* goes to infinity, then f(n) is Θ(g(n)).

![](http://i.imgur.com/7CB8VLS.png)

####O(n log n) time

* Also known as **linearithmetic** time. This time complexity arises in divide-and-conquer time.
* Sorting: Mergesort and Heapsort are sorting algorithms that perform O(n log n) comparisons.
* Largest Empty Interval: Given n time-stamps x<sub>1</sub>,...,x<sub>n</sub> on which copies of a file arrive at a server, what is the largest interval of time when no copies of the file arrive?
* O(n log n) solution: Sort the time-stamps. Scan the sorted list in order, identifying the maximum gap between successive time-stamps.

####Quadratic time

* Enumerates all pairs of elements.
* Closest pairs of points: Given a list of n points in the plane (x<sub>1</sub>,y<sub>1</sub>),...,(x<sub>n</sub>,y<sub>n</sub>), find the closest pair.
* Quadratic solution: For each pair of elements, determine whether they are smaller than the current minimum.

Pseudocode:

	min <-- (x1-x2)^2 + (y1 - y2)^2

	for i = 1 to n{
		for j = i+1 to n{
			d <-- (x[i]-x[j])^2+(y[i]-y[j])^2
			if (d<min)
				min <-- d
		}
	}

####Cubic time

* Enumerates all triples of elements.
* Set disjointness: Given n sets S<sub>1</sub>,...,S<sub>n</sub> each of which is a subset of 1,2,...,n, is there some pair of these which are disjoint?
* Cubic time solution: For each pair of sets, determine if they are disjoint.

Pseudocode:

	foreach set Si{
		foreach other set Sj{
			determine whether p also belongs to Sj
		}
		if (no element of Si belongs to Sj)
			report that Si and Sj are disjoint
	}

####Polynomial time

* Independent set of size k: Given a graph, are there k nodes such that no two are joined by an edge?
* Polynomial solution: Enumerate all subsets of k nodes.

Pseudocode:

	foreach subset S of k nodes{
		check whether S is an independent set
		if (S is an independent set){
			report S is an independent set
		}
	}

####Exponential time

* Independent set: Given an graph, what is the max size of an independent size?
* O(n^2 2^n) solution: Enumerate all subsets.

Pseudocode:

	s* <-- NULL

	foreach subset S of nodes{
		check whether S is an independent set

		if (S is largest independnet set seen so far){
			update s*<---S
		}
	}

Induction and Recursion
=======================

Induction proofs
----------------

####Process

**Predicate.**
P(n): f(n) = some formula in n

**Statement.**
for all n ≥ 1, P(n) is true.

**Proof.**
* Base case: prove that P(1) is true.
* Induction step: Show that for all n ≥ 1, P(n) being true implies P(n+1) being true as well.

####Example

Predicate: P(n): 1+2+...+n = n(n+1)/2

Base case: When n = 1, we have 1 = 2/2.

Induction step: let n ≥ 1. Assume for induction hypothesis that P(n) is true. We show P(n+1) is true as well:

	1+2+...+n+(n+1) = n(n+1)/2+n+1
					= (n+1)(n/2)+1
					= (n+1)(n+2)/2

**Note: Although the predicate works, using Σ summation notation is a lot more formal.**

Iteration vs Recursion
----------------------

f(n) = 1 + 2 + ... + n

	f(n)
	sum <-- 0
	for i = 2 to n {
		sum <-- sum+i
	}
	return sum

f(n) = 0 if n=0
f(n) = f(n-1)+1 if n>0

	f(n)
	if n = 0 (return 0)
	else {return f(n-1)+n}

####Redoing the induction proof with recursion

Predicate.
f(n) = n(n+1)/2
Statement.

Base case:


Generalized induction proofs
----------------------------

####Process

**Predicate.**
P(n): f(n) = some formula in n

**Statement.**
for all n ≥ 1, P(n) is true.

**Proof.**
* Base case: prove that P(1) is true.
* Induction step: Let n ≥ 1. Show that P(1)...P(n) being true implies P(n+1) being true as well.

####Example: Fibonacci Sequence

	fib(n) = n 					 if n≤1
	fib(n) = fib(n-1) + fib(n-2) if n>1

	//Recursive implementation:

	fib(n)
	if n<2 {return n}
	else {return fib(n-1)+fib(n-2)}

	//Iterative implementation:

	fib(n)
	a<--0
	b<--1
	for i = 1 to n{
		b <-- a + b
		a <-- b - a
	}
	return a

**Note: The recursive implementation is very inefficient, as we already compute smaller values in our Fibonacci sequence while finding larger values, leading to many repeat computations (the number of which actually follows the Fibonacci sequence itself, interestingly enough). On the other hand, the iterative version carries the values of the previous Fibonacci values, which leads to each computation being made only once.**

####Example 1

**Statement.**
for all n ≥ 0, P(n): fib(n) ≤ 2<sup>n</sup> is true.

**Proof.**
* Base case: 0 ≤ 1 is true and 1 ≤ 2 is true.
* Induction step: Let n ≥ 1. Show that P(1)...P(n) being true implies P(n+1) being true as well:

	fib(n+1) = fib(n)+fib(n-1)
			 ≤ 2^n + 2^(n-1)
			 ≤ 2^(n-1)*3
			 < 2^(n+1)

####Example 2

**Statement.**
for all n ≥ 1, P(n): fib(n) ≤ ϕ<sup>n</sup> is true.

**Proof.**
* Base case: 1 ≤ ϕ is true and 1 ≤ ϕ<sup>2</sup> is true. (Only if ϕ ≥ 1.)
* Induction step: Let n ≥ 1. Show that P(1)...P(n) being true implies P(n+1) being true as well:

	fib(n+1) = fib(n)+fib(n-1)
			 ≤ ϕ^n + ϕ^(n-1)
			 ≤ ϕ^(n-1)*(ϕ+1)
			 < ϕ^(n+1)

	(Only if 0 ≤ ϕ^2-ϕ-1)

####Example 3

**Statement.**
for all n ≥ 1, P(n): fib(n) ≥ ϕ<sup>n-2</sup> is true.

**Proof.**
* Base case: 1 ≥ ϕ<sup>-1</sup> is true and 1 = ϕ<sup>0</sup> is true. (Only if ϕ ≥ 1.)
* Induction step: Let n ≥ 1. Show that P(1)...P(n) being true implies P(n+1) being true as well:

	fib(n+1) = fib(n)+fib(n-1)
			 ≥ ϕ^(n-2) + ϕ^(n-3)
			 ≥ ϕ^(n-3)*(ϕ+1)
			 ≥ ϕ^(n-1)

	(Only if 0 ≥ ϕ^2-ϕ-1)

####Weak Binet Formula

For all n ≥ 1, fib(n) ≤ ϕ<sup>n</sup> is true.
(Only if 0 ≤ ϕ<sup>2</sup>-ϕ-1)
For all n ≥ 1, fib(n) ≥ ϕ<sup>n</sup> is true.
(Only if 0 ≤ ϕ<sup>2</sup>-ϕ-1)

So these statements are both true only if  0 = ϕ<sup>2</sup>-ϕ-1.

ϕ here is the Golden Ratio.

Recursive Algorithms
--------------------

There are many algorithms that are easier to think of recursively than iteratively. An example of this is Mergesort.


####Mergesort

#####Overview
* Divide array into two halves.
* Recursively sort each half.
* Merge two halves to make sorted whole.

![](http://i.imgur.com/AfjDuk5.png)

#####Merge step
* Keep track of smallest element in each sorted half.
* Insert smallest of two elements into auxiliary array.
* Repeat until done.

#####Recurrence relation

Definition: T(n) = number of comparisons to mergesort an input of size n.

Mergesort recurrence:

![](http://i.imgur.com/UsFM7ww.png)

Solution: T(n) is O(n log n).

