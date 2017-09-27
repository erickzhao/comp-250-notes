**Winter 2016**
**COMP-250: Introduction to Computer Science**

Algorithms
==========
##### Informal Definition
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
#### Definition
Computer science is the study of algorithms for computing machines.

#### Formal definition of an algorithm
A well-ordered collection of unambiguous effectively computable operations that when executed produces a result and halts in a finite amount of time.

#### What distinguishes computer algorithms?

* Instructions are executed very quickly
* Algorithms must be fully specified before execution
* Algorithms must be unambiguous

Grade School Algorithms
-----------------------
* Unary addition: taking a set A and a set B, their sum C will be the union of A and B. You have to count everything out individually.
* Unary multiplication: taking a set A and a set B, their product B will be the substition of each element of set A by a set B'.

Unary representation is quite inefficient. It works well for addition of small sets because it's easy to visualize, but once sets get too large, your representation will contain way too many characters.

Numeral representations, such as the Roman numerals, came up with more efficient representations to describe larger numbers. However, since the largest character M only represents 1000, it is hard to represent numbers that are of orders of magnitude larger. Furthermore, the romans didn't even have a zero in their system, which means that addition was impossible. Adding 0 expanded how mathematics could work.

#### Algorithm 1: Addition

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

#### Algorithm 2: Multiplication

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
We analyze the space and time required to run an algorithm. The space used is less important than time. We do not analyze the time required to run each possible combination of inputs. This would take into account too many cases and would include too many details, rendering our analysis useless.

In reality, we analyze time as a function of the size of the inputs (which is the parameter *N*). For all inputs of the same size, we would have a precise bound.

#### Analysis of Addition
			
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

The constants c<sub>1</sub> and c<sub>2</sub> depend on the hardware that you run the algorithm on. The important thing to take home is that this algorithm has linear time, meaning that doubling the inputs will double time. This statement becomes more true for larger input sets.

#### Analysis of Multiplication

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

#### Big O Notation

This notation drops all lower exponents and constants in Time(N).
For example, addition is O(N) and multiplication is O(N<sup>2</sup>).

Today, the best multiplication algorithm has a running time of O (N x 2<sup>log* N</sup>). This is very close to linear time, as the iterated logarithm (log*) function increases incredibly slowly. Of course, this makes for a much more sophistic algorithm.

Bases and Binary Representation
===============================

#### Base 8 to Base 2

(2143)<sub>8</sub> = 2 x 8<sup>3</sup> + 1 x 8<sup>2</sup> + 4 x 8<sup>1</sup> + 3 x 8<sup>0</sup>

Since 8 = 2<sup>3</sup>, you can simply convert the digit in base 8 to its binary representation. Each digit of 8 is equivalent to three binary digits. Thus, the conversion can be done by only local operations.

(2143)<sub>8</sub> = (010 001 100 011)<sub>2</sub> = (010001100011)<sub>2</sub>

An equivalent process is done to convert binary numbers to the much more readable hexadecimal base unit.

#### Base 2 to Base 10

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

#### Algorithm 3: Convert integer to binary

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

#### Fractional numbers

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

#### Binary representation in computers

Computer memory does not expand or retract depending on the size of the data, so we always work with a fixed size of memory. The smallest unit of memory is a _byte_. A byte contains 8 bit registers for memory. 

Data representation on a byte is circular because adding to the maximum value of (11111111)<sub>2</sub> resets the number to 00000000 (which is equivalent to all operations being modulo 256).

* Unsigned representation: Each byte contains a positive value from 0 to 255 (2<sup>8</sup>-1).
* Signed representation: Each byte contains a value from -128(-2<sup>7</sup>) to 127(2<sup>7</sup>-1). The greatest bit is used to tell the sign of the number.

Here is a table of binary numbers:

	Binary     Signed   Unsigned
	--------   ------   --------
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

#### How this works in Java

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

#### Algorithm 4: Insertion Sort

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

#### Analysis of Insertion Sort

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

#### Java Generics

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

#### Doubly Linked Lists

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

#### Arrays vs. Linked Lists

	.				array 			S.L.L.			D.L.L.
	.				-----			------			------
	addFirst		N 				1				1
	removeFirst		N 				1				1
	addLast			1 				1 				1
	removeLast		1 				N 				1
	getNode(i)		1				i 				min(i, N/2 - i)

#### Linked Lists operations

	add(i,element)	//Inserts element into i-th position
	set(i,element)	//Sets the element at the i-th position to a specific value
	remove(i)		//Removes element from i-th position
	get(i)			//Returns the element from i-th position
	clear()			//Removes all elements from the list
	isEmpty()		//Returns a boolean indicating if the list is empty
	size()			//Returns the size of the list

#### Java LinkedList

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

#### Java ArrayList

* Implementation using arrays of growing sizes
* Cannot access using a[i] notation

ArrayLists store the array, the size of the array, and the capacity of the array.
Adding elements is done by creating a new array with double the capacity and adding the new element inside.
The cost of doing so is actually **linear**. The extra cost of creating a new array is proportional to the amount of elements already in the array. The charge is constant per element, so the cost is **amortized constant time**.

To get to an array of 2<sup>k</sup> length, you need to create arrays for all powers of 2 preceding it. The sum of these would actually be 2k.

#### LinkedList vs ArrayList

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

#### Methods

Two main methods:

* push(o): Inserts object o onto top of stack.
* pop(): Removes the top object of stack and returns it. If the stack is empty, then an error occurs.


The following methods should also be defined:

* size(): returns the number of objects in stack
* isEmpty(): returns a boolean indicating if sstack is empty
* top(): returns the top object of the stack, without removing it; if the stack is empty then an error occurs.

#### Examples

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

			[
		(	(	(						[
	(	(	(	(	(		[		{	{	{
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

#### Stacks in the Java Virtual Machine

* Each process running in a Java program has its own Java Method Stack
* Each time a method is called, it is pushed onto the stack.
* The choice ofa stack for this operation allows Java to do several useful things
	- Perform recursive method calls
	- Print stack traces to locate an error

Each item on the stack keeps count of the method information, as well as something called a program counter. This shows the line where the method has been interrupted by another method that was added on top of it in the stack. Once the other method is removed, the program reverts to the line saved in the program counter.

#### Application: Time Series

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

#### Queues as arrays

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

#### Brute force
For many non-trivial problems, there is a natural brute force search algorithm that tries every possible solution.

* Typically takes 2<sup>N</sup> time or worse for inputs of size N.
* Unacceptable in practice.

#### Desirable scaling property
When the input size doubles, the algorithm should only slow down by some constant factor C. This is equivalent to saying that there exist constants a>0 and d>0 such that on every input of size N, its running time is bounded by a N<sup>d</sup> steps.

**Definition**: An algorithm is **poly-time** if the above scaling property holds.

#### Worst case analysis
The worst case is the upper bound on the running time of an algorithm on any input of size N.

* Generally captures efficiency in practice
* Seems draconian, but it's hard to find an effective alternative

#### Average case analysis
The average case is the bound on running time of an algorithm on a **random** input as a function of input size N.

* Hard (or impossible) to accurately model real instances by random distribution.
* Algorithm tuned for a certain distribution may perform poorly on other inputs.

#### Worst case polynomial-time

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

#### Comp Sci approach to problem solving

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

#### Properties

* Transitivity: if f is O(g) and g is O(h), then f is O(h).
* Additivity: if f is O(h) and g is O(h), then f + g is O(h).

Note: These apply for O, Ω, and Θ.

#### Frequently used functions

* Polynomials: a<sub>0</sub>+a<sub>1</sub>n+...+a<sub>d</sub>n<sup>d</sup> is Θ(n<sup>d</sup>) if a<sub>d</sub>>0.
* Polynomial time: Running time is O(n<sup>d</sup>) for some constant *d* independent of the input size *n*.
* Logarithms: O(log<sub>a</sub>n) = O(log<sub>b</sub>n) for any constants a,b > 0.
	- For every x > 0, log n is O(n<sup>x</sup>). (i.e. every log grows slower than every polynomial.)
* Exponentials. For every r > 1 and every d > 0, n<sup>d</sup> is O(r<sup>n</sup>). (i.e. every exponential grows faster than every polynomial.)

#### Asymptotic notation

Sometimes, one can also obtain an asymptotically tight bound directly by computing a limit as *n* goes to infinity. Essentially, if the ratio of functions f(n) and g(n) converges to a positive constant as *n* goes to infinity, then f(n) is Θ(g(n)).

![](http://i.imgur.com/7CB8VLS.png)

#### O(n log n) time

* Also known as **linearithmetic** time. This time complexity arises in divide-and-conquer time.
* Sorting: Mergesort and Heapsort are sorting algorithms that perform O(n log n) comparisons.
* Largest Empty Interval: Given n time-stamps x<sub>1</sub>,...,x<sub>n</sub> on which copies of a file arrive at a server, what is the largest interval of time when no copies of the file arrive?
* O(n log n) solution: Sort the time-stamps. Scan the sorted list in order, identifying the maximum gap between successive time-stamps.

#### Quadratic time

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

#### Cubic time

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

#### Polynomial time

* Independent set of size k: Given a graph, are there k nodes such that no two are joined by an edge?
* Polynomial solution: Enumerate all subsets of k nodes.

Pseudocode:

	foreach subset S of k nodes{
		check whether S is an independent set
		if (S is an independent set){
			report S is an independent set
		}
	}

#### Exponential time

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

#### Process

**Predicate.**
P(n): f(n) = some formula in n

**Statement.**
for all n ≥ 1, P(n) is true.

**Proof.**
* Base case: prove that P(1) is true.
* Induction step: Show that for all n ≥ 1, P(n) being true implies P(n+1) being true as well.

#### Example

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

#### Redoing the induction proof with recursion

Predicate.
f(n) = n(n+1)/2
Statement.

Base case:


Generalized induction proofs
----------------------------

#### Process

**Predicate.**
P(n): f(n) = some formula in n

**Statement.**
for all n ≥ 1, P(n) is true.

**Proof.**
* Base case: prove that P(1) is true.
* Induction step: Let n ≥ 1. Show that P(1)...P(n) being true implies P(n+1) being true as well.

#### Example: Fibonacci Sequence

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

#### Example 1

**Statement.**
for all n ≥ 0, P(n): fib(n) ≤ 2<sup>n</sup> is true.

**Proof.**
* Base case: 0 ≤ 1 is true and 1 ≤ 2 is true.
* Induction step: Let n ≥ 1. Show that P(1)...P(n) being true implies P(n+1) being true as well:

	fib(n+1) = fib(n)+fib(n-1)
			 ≤ 2^n + 2^(n-1)
			 ≤ 2^(n-1)*3
			 < 2^(n+1)

#### Example 2

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

#### Example 3

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

#### Weak Binet Formula

For all n ≥ 1, fib(n) ≤ ϕ<sup>n</sup> is true.
(Only if 0 ≤ ϕ<sup>2</sup>-ϕ-1)
For all n ≥ 1, fib(n) ≥ ϕ<sup>n</sup> is true.
(Only if 0 ≤ ϕ<sup>2</sup>-ϕ-1)

So these statements are both true only if  0 = ϕ<sup>2</sup>-ϕ-1.

ϕ here is the Golden Ratio.

Recursive Algorithms
--------------------

There are many algorithms that are easier to think of recursively than iteratively. An example of this is Mergesort.


#### Mergesort

##### Overview
* Divide array into two halves.
* Recursively sort each half.
* Merge two halves to make sorted whole.

![](http://i.imgur.com/AfjDuk5.png)

##### Merge step
* Keep track of smallest element in each sorted half.
* Insert smallest of two elements into auxiliary array.
* Repeat until done.

##### Recurrence relation

Definition: T(n) = number of comparisons to mergesort an input of size n.

Mergesort recurrence:

![](http://i.imgur.com/UsFM7ww.png)

Solution: T(n) is O(n log n).


#### Tower of Hanoi

**Goal:** Move the n discs from stack #3 to stack #2.

![](http://www.mathcs.emory.edu/~cheung/Courses/170/Syllabus/13/FIGS/hanoi03.gif)

**Constraints:**

* Only remove one disk at a time.
* Cannot stack a larger disk on top of a smaller disk.

**Inductive solution:**

Hanoi(n,S3,S2,S1) // n>=1

Base case (n = 1): move disk 1 from stack 3 to stack 2.

	if n>1, then Hanoi(n-1,S3,S1,S2)
	move disk n from S3 to S2
	if n>1, then Hanoi(n-1,S1,S2,S3)


Example: If we have 5 disks

	Hanoi(5,S3,S2,S1) //n>=1

	if 5>1 then Hanoi(4,S3,S1,S2)
		if 4>1 then Hanoi (3,S3,S2,S1)
			if 3>1 then Hanoi (2,S3,S1,S2)
				if 2>1 then Hanoi (1,S3,S2,S1)
					move disk 1 from S3 to S2

All these executions are stacked on the execution stack that compiles all processes.

**Recurrence relation:**

Def. T(n) = number of moves to Hanoi of n.

Hanoi recurrence.

[INSERT IMAGE]

Solution. T(n) is O(2<sup>n</sup>).

Proofs: [INSERT PROOF]

Master Theorem
==============


#### Definition
The *Master Theorem* is a formula that can be applied to divide-and-conquer algorithms to calculate their running time given their recurrence relation.

Given a relation: 

	T(n) = aT(n/b) + f(n)
	where: a ≥ 1, b > 1, and f(n) > 0

#### Three cases
*Case 1*: Leaves dominate the running time.
If f(n) is O(n<sup>L</sup>) for some constant L < log<sub>b</sub>a, T(n) is ϴ(n<sup>log<sub>b</sub>a</sup>).

*Case 2*: Initial case dominates the running time.
If f(n) is Ω(n<sup>L</sup>) for some constant L > log<sub>b</sub>a, T(n) is ϴ(f(n)).

*Case 3*: Nothing dominates the running time.
If f(n) is ϴ(n<sup>log<sub>b</sub>a</sup> log<sup>k</sup>n), for some k ≥ 0, T(n) is ϴ(n<sup>log<sub>b</sub>a</sup> log<sup>k+1</sup>n).

####Recursion tree illustration

![](http://i.imgur.com/3m3A82T.png)


Divide and conquer algorithms
=============================

#### Definition

* Break up problem into several parts
* Solve each part recursively
* Combine solutions to sub-problems in overall solution

#### Binary Search

*Input*: array *a*, value *v*, lower and upper bound indices *low*, *high*.
*Output*: the index *i* of element *v* (if present), -1 (if *v* is not present).

	if low==high then
		if a[low]==v then
			return low
		else
			return -1
		end if
	else
		mid <--(low+high)/2
		if v<=a[mid] then
			return binarySearch(a,v,low,mid)
		else
			return binarySearch(a,v,mid+1,high)
		end if
	end if

*Recurrence relation*

![](http://i.imgur.com/ilsG2S1.png)

#### Karatsuba multiplication

![](http://i.imgur.com/i1CEswL.png)

*Recursion Tree*
![](http://i.imgur.com/0mAdvkP.png)

Graphs
======
A vast number of problems in practice are solved with graphs. A subset of graphs, trees, are incredibly useful.

#### Definition: 

* A graph G = (V,E) is composed of V (set of vertices) and E (set of edges connecting the vertices in V)
* An edge e = (u,v) is a pair of vertices

Example:

![](http://i.imgur.com/lxU16CH.png)

#### Applications:

* Electronic circuits (finding the path of least resistance)
* Networks (roads, flights, communications)
* World Wide Web (hyperlinks connecting web pages)

These two examples have no orientation in their graphs. However, we can have direction between vertices, such as in the graph for a project workflow.

#### Terminology:

* **Adjacent vertices**: connected by an edge.
* **Degree (of a vertex)**: number of adjacet vertices. The sum of all the degrees in the graph, you get twice the number of edges. This is because every edge is a pair of 2 vertices.
* **Path**: sequence of vertices v<sub>1</sub>,v<sub>2</sub>,...,v<sub>k</sub> such that consecutive vertices v<sub>i</sub> and v<sub>i+1</sub> are adjacent.
* **Cycle**: simple path, but where the last vertex is the same as the first.
* **Connected graph**: any 2 vertices are connected by some path.
* **Subgraph**: subset of vertices and edges forming a graph.
* **Connected component**: maximal connected subgraph.
* **Tree**: connected graph without cycles
* **Forest**: Collection of trees
* **Complete graph**: all pairs of vertices are adjacent
* **Spanning tree**: a subgraph of G which is a tree that contains all of its vertices.

Connectivity
------------
The edges and vertices in a graph are generally two independent parameters.

Let m = #edges, n = #vertices

* For a complete graph: m = n(n-1)/2
* For a tree: m = n - 1
* If m < n - 1, G is not connected

Making a graph a cycle would make it more fault-tolerant and only requires n edges.

#### Konigsberg bridge problem

![](http://www.contracosta.edu/legacycontent/math/KBRIDG3.GIF)
![](http://jwilson.coe.uga.edu/EMAT6680Fa2012/Faircloth/Essay2alf/proof1.jpg)

Can one walk across all 7 bridges of Konigsberg without crossing any twice? Euler proved that this problem is unsolvable.

The graph ADT
-------------

The graph ADT is a positional container whose positions are the vertices and the edges of the graph.

![](http://i.imgur.com/tky9nlQ.png)

Methods:

* size()
* isEmpty()
* elements()
* positions()
* swap()
* replaceElement()
* numVertices()
* numEdges()
* vertices()
* edges()
* directedEdges()
* undirectedEdges()
* incidentEdges(v)
* inIncidentEdges(v)
* outIncidentEdges(v)
* opposite(v,e)
* degree(v)
* inDegree(v)
* outDegree(v)
* adjacentVertices(v)
* inAdjacentVertices(v)
* outAdjecentVertices(v)
* areAdjacent(v,w)
* endVertices(e)
* origin(e)
* destination(e)
* isDirected(e)
* makeUndirected(e)
* reverseDirection(e)
* setDirectionFrom(e,v)
* setDirectionTo(e,v)
* insertEdge(v,w,o)
* insertDirectedEdge(v,w,o)
* insertVertex(o)
* removeEdge(e)

Data Structures for Graphs
--------------------------
These data structures store the vertices and edges of a graph into two containers, where each edge object has references to the vertices it connects.

#### Edge List
The edge list structure stores the vertices and edges into unsorted sequences. It's the easiest to implement. Finding the edges incident on a given vertex is inefficient since it requires examining the entire edge sequence.

![](http://i.imgur.com/wvYjozs.png)

#### Adjacency List
The adjacency list of a vertex stores all adjacent vertices. You can construct a graph with the adjacency lists of all vertices. The adjacency list extends the edge list structure by adding incidence containers to each vertex.

![](http://i.imgur.com/3qelxo6.png)

#### Adjacency Matrix
The adjacency matrix is a matrix M with entries for all pairs of vertices. Boolean values indicate whether or not there are existing edges between two vertices. Notice how for undirected edges, the matrix is symmetric.

![](http://i.imgur.com/qtoq11x.png)

Trees
=====

Trees describe hierarchical structures (e.g. organizational structure of a corporation, file directory in a Unix-based system, or the table of contents of a manual)

#### Terminology

![](http://i.imgur.com/DWbSrR9.png)

#### Methods
* *Generic container methods*: size(), isEmpty(), elements()
* *Positional container methods*: positions(), swapElements(p,q), replaceElements(p,e)
* *Query methods*: isRoot(p), isInternal(p), isExternal(p)
* *Accessor methods*: root(), parent(p), children(p)
* *Update methods*: application-specific

Binary Trees
------------

*Ordered tree*: The children of each node are ordered.

*Binary tree*: Ordered tree with all internal nodes of degree 2.

*Recursive definition of a binary tree*: A binary tree is either an external node (leaf) or an internal node (root) with two binary subtrees (left and right).

Examples: 

*Arithmetic expression*

![](http://i.imgur.com/al2HBtI.png)

*Decision trees*

![](http://i.imgur.com/3JIizeB.png)

####Properties

![](http://i.imgur.com/vJX7oDN.png)

#### Methods
* *Accessor methods*: leftChild(p), rightChild(p), sibling(p)
* *Update methods*: expandExternal(p), removeAboveExternal(p)

##### Linked data structure for binary trees

Note: Focusing on one branch of the binary tree will be the same as a doubly linked list.

![](http://i.imgur.com/geYIqa8.png)

##### General tree representation

![](http://i.imgur.com/h1cWk75.png)

We can still use binary trees to represent general trees. If you have a general tree, you can sub it with a binary tree, where siblings are on the right side of the node, and children are on the left side of the node.


Depth-first search
------------------

Depth-first search is an algorithm that traverses trees and graphs. To implement it, we will be using a stack (FILO) process, as we go further into our tree and backtrack once we reach a dead end. 

#### Example

* A DPS in an undirected graph G is like wandering in a labyrinth with a string and a can of red paint without getting lost.
* We start at a vertex **s**, tying the end of our string to the point and painting **s** as "visited". Next, we label **s** as our current vertex **u**.
* Now we travel along an arbitrary edge (u,v).
* If edge (u,v) leads us to an already visted vertex **v**, we return to **u**.
* If vertex *v* is unvisited, we unroll our string to move to *v*, paint *v* "visited", set *v* as our current vertex, and repeat the previous steps.

* Eventually, we will get to a point where the edges on *u* lead to visited vertices. We then backtrack by unrolling our string to a previous visited vertex *v*. Then *v* becomes our current vertex and we repeat the previous steps.

* If all incident edges of *v* lead to visited vertices, we backtrack as we did before. We continue to backtrack along the path we have traveled, finding and exploring unexplored edges, and repeating the procedure.

* When we backtrack to vertex *s* and there are no more unexplored edges incident on *s*, we have completed our DFS search.

#### Algorithm

*Input*: Vertex v in a graph
*Output*: A labeling of the edges as "discovery" edges and "backedges"

Discovery edges: Edges leading to new node
Back edges: Edges leading to a visited node

	for each edge e incident on v do
		if edge e is unexplored then
			let w be the other endpoint of e
			if vertex w is unexplored then
				label e as a discovery edge
				recursively call DFS(w)
			else

			label e as backedge

![](http://i.imgur.com/ZTHQS6H.png)

*Animation*:

![](http://i.imgur.com/QcWrNK2.gif)

#### Determining Incident Edges

DFS depends on how you obtain incident edges. If we start at A and we examine the edge to F, then to B, then E, C, and finally G:

	A->F->B->E->C->G

At the end of the algorithm execution, all vertices will be visited once and all edges twice (once from each of its vertices). The discovery edges will form a spanning tree of the connected component of s.

#### Properties

If *G* is an undirected graph on which a DFS traversal starting at a vertex *s* has been performed, then:

* The traversal visits all vertices in the connected component of *s*.
* The discovery edges form a spanning tree of the connected component of *s*.

#### Runtime analysis

Recall:
* DFS is called on each vertex exactly once.
* Every edge is examined exactly twice, once from each of its vertices.

Thus, for n<sub>s</sub> vertices and m<sub>s</sub> edges in the connected componnet of the vertex *s*, the total running time for DFS starting at *s* is *O(n<sub>s</sub>+m<sub>s</sub>)*, but only if:
* The graph is represented in a data structure (like the adjacency list) where vertex and edge methods take constant time.
* Marking a vertex as explored and testing to see if a verrex has been explored takes O(degree)
* By marking visited nodes, we can systematically consider the edges incident on the current vertex so we do not examine the same edge more than once.

Breadth-First Search
--------------------

BFS is very similar to DFS, but visits everything on a certain level before descending to the next. Each level contains all elements at a certain distance away from the starting point.

![](http://i.imgur.com/NebNFAM.png)

BFS traverses a connected component of a graph, and in doing so defines a spanning tree that has several useful properties:

* The starting vertex *s* has level 0 and is an anchor.
* In the first round, the string is unrolled the length of one edge, and all the edges that are one edge away from the anchor are visited.
* These edges are placed into level 1.
* In the second round, all the new edges that can be reached by unrolling the string another edge away are placed in level 2.
* This continues until every vertex has been assigned a level.
* The label of any vertex *v* corresponds to the length of the shortest path from *s* to *v*.

*Animation*:

![](http://i.imgur.com/cpoHGKe.gif)

#### Algorithm

*Input*: a vertex s in a graph

*Output*: A labelling of the edges as discovery edges and cross edges.

	initialize container L_0 to contain vertex s

	i<--0

	while L_i is not empty  do
		create container L_i+1 to initially be empty
		for each vertex v in L_i do
			for each edge e incident on v do
				if edge e is unexplored then
					let w be the other endpoint of e
					if vertex w is unexplored then
						label e as a discovery edge
						insert w into L_i+1
					else
						label e as a cross edge
		i<--i+1

#### Properties

* All vertices in the connected component of s will be visited. 
* The discovery edges form a spanning tree T of the connected component of s.
* For each vertex v at level i, the path of the BFS tree T between s and v has i edges, and any other path of G between s and v has at least i edges.
* If (u,v) is an edge that is not in the BFS tree, then the level numbers of u and v differ by at most one.

#### Runtime analysis

BFS traversal of a graph G with n vertices and m edges will take time O(m+n). Also, there exist O(n+m) time algorithms based on BFS for the following problems:
* Testing whether G is connected
* Computing a spanning tree of G
* Computing the connected components of G
* Computing for every vertex *v*, the minimum number of edges on any path between *s* and *v*.


Binary Search Trees
-------------------

Purpose: to create a dictionary ADT to store information organized as a tree that can be looked up as efficiently as possible.

* A dictionary is an abstract model of a database.
* Like a priority queue, a dictionary stores key-element pairs.
* The main operation supported by a dictionary is searching by key.

#### Methods

* Simple container methods:
	- size()
	- isEmpty()
	- elements()

* Query methods:
	- findElement(k)
	- findAllElements(k)

* Update methods:
	- insertItem(k,e)
	- removeElement(k)
	- removeAllElements(k)

* Special element:
	- NO SUCH KEY, returned by an unsuccessful search

#### Sequence-based dictionary implementation

![Imgur](http://i.imgur.com/0AtfG2O.png)
* Unordered sequence:
	- Searching and removing takes O(n) time
	- Inserting takes O(1) time
	- Applications to log files (frequent insertions, rare searches, and removals)

![Imgur](http://i.imgur.com/0AtfG2O.png)
* Array-based ordered sequence:
	- Searching takes O(n lg n) time (binary search)
	- Inserting and removing takes O(n) time
	- Useful when you need frequent searches, rare insertions and removals (i.e. a real-life dictionary)

However, if we organize our sequence as a tree, we can get insertion/removal times that are very close to the time we get for searches (log n), which improves our dictionary ADT. For this, we use binary search trees.

A binary search tree is a binary tree T such that
* Each internal node stores an item (k,e) of a dictionary.
* Keys stored at nodes in the left subtree of v are less than or equal to k.
* Keys stored at nodes in the right subtree of v are greater than or equal to k.
* External nodes do not hold elements but serve as place holders.

![](http://i.imgur.com/4velnlQ.png)

A binary search tree T is a decision tree, where the question asked at an internal node v is whether the search key k is less than, equal to, or greater than the key stored at v.

#### Algorithm

Algorithm TreeSearch(k,v):

*Input*: A search key k and a node v of a binary search tree T.

*Output*: A node w of the subtree T(v) of T rooted at v.

	if (v is an external node) then return v
	if k = key(v) then return v
	if k < key(v) then return TreeSearch(k,T.leftChild(v))
	if k > key(v) then return TreeSearch(l,T.rightChild(v))

#### Example

![](http://i.imgur.com/5dery3a.png)

![](http://i.imgur.com/fOzgd8B.png)

#### Insertion

* To perform insertItem(k,e), let w be the node returned by TreeSearch(k,T.root())

* If w is external, we know that k is not stored in T. We call expandExternal(w) on T and store (k,e) in w.

![](http://i.imgur.com/0nJvYyR.png)

* If w is internal, we know another item with key k is stored at w. We call the algorithm recursively starting at T.rightChild(w) or T.leftChild(w).

![](http://i.imgur.com/pa0h1xK.png)

#### Removal

* We locate the node *w* where the key is stored with algorithm TreeSearch.
* If w has an external child z, we remove w and z with removeAboveExternal(z)

![](http://i.imgur.com/6GGe93R.png)

* If w has no external children:
	- Find the internal node y following w in order
	- Move the item at y into w
	- Perform removeAboveExternal(x), where x is the left child of y (guaranteed to be external)

![](http://i.imgur.com/tJsof36.png)

#### Time complexity

* A search, insertion, or removal visits the nodes along a root-to-leaf path, plus possibly the siblings of such nodes.
* Time O(1) is spent at each node.
* The running time of each operation is O(h), where h is the height of the tree.
* The height of a binary search tree is n in the worst case, where a binary search tree looks like a sorted sequence (only children on one side).
* To achieve good running time, we need to keep the tree balanced (with O(log n) height)

Heaps
-----

A heap is a binary tree T that stores a collection of keys (or key-element pairs) at its internal nodes and that satisfies two additional properties.
* *Order property*: key(parent) ≤ key(child).
* *Structural property*: all levels are full, except the last one, which list left-filled. We want to get as close to a complete binary tree as possible, but empty nodes are all on the bottom right.

Heaps are useful to find min/max values in a set. Note that we don't care about the order in which the children are positioned, unlike in binary search trees.

#### Height

![](http://i.imgur.com/2gzw3AA.png)

#### Insertion

![](http://i.imgur.com/K5D0gfD.gif)

* Upheap terminates when the new key is greater than the key of its parent or the top of the heap is reached.
* Total # of swaps < (h - 1), which is O(log n).

#### Removal

![](http://i.imgur.com/QBmFebu.gif)

* To remove the minimum node on a heap, we first promote the bottom right node to the top, which satisfies our structural property, but not our order property.
* To fix that, we downheap (comparing parent with the smallest child and switching the two).
* Downheap ends if the key is greater than both children or if the bottom fo the heap is reached.
* Total # of swaps ≤ (h-1), which is O(log n).

#### Implementation

One might be tempted to implement heaps as trees. The problem with this is that we have to know where we have to figure out where to insert following elements into the heap.

There are two cases for insertion:
* If the last node is the right child of every node going back to the root, then we know it's the last element of the level, and we have to start filling up the last level.

![](http://i.imgur.com/LTeefnl.png)

* If the last node is the left child at any point of the tree, then we move right from the left-child-node, and go down all the way to the left.

![](http://i.imgur.com/wvEkicQ.png)

As you can tell, this is very complicated. If we were to represent our heap with vectors (arrays), inserting a new element would just mean placing it at the next available spot.

A heap can be thus represented by a *vector*, where the node at rank *i* has:
* a left child at rank 2*i*
* a right child at rank 2*i*+1

![](http://i.imgur.com/82DqBPt.png)

Notes:
* The leaves do not need to be explicitly stored
* Insertion and removal methods correspond to insertLast() and removeLast() on the vector.


#### Heapsort

One of the main reasons for making heaps is to use them in sorting, since they're mostly used to find the smallest element in a set. Sorting using heaps is called *Heapsort*.

* All heap methods run in logarithmic time or better
* If we implement PriorityQueueSort using a heap for our priority queue, insertItem() and removeMin() each take O(log k), where k is the # of elements in the heap at a given time.
* We always have at most n elements in the heap, so the worst case time complexity of these methods is O(log n).
* Thus each phase takes O(n log n) time, so the algorithm runs in O(n log n) time as well.
* The O(n log n) running time of heapsort is much better than the O(n<sup>2</sup>) runtime of selection and insertion sort.

#### Bottom-up heap construction

We can in fact do an in-place heapsort, meaning that we can go through without actually making a heap separate from the unsorted array we started from. This saves the extra space of creating a second array, and we can do this in linear time. 

To do this, we do what we call bottom-up construction of a heap.

1. Insert (n+1)/2 nodes
2. Insert (n+1)/4 nodes and downheap
3. Insert (n+1)/8 nodes and downheap
4. ...

![Heap construction process](http://i.imgur.com/rzV8yjT.gif)

The total cost of bottom-up heap construction with n keys takes O(n) time. You end up with n inserts, n/2 upheaps with total O(n) running time.

![Heap construction process](http://i.imgur.com/pz1y2OH.png)

Heapsort is preferable to Mergesort in many cases as it does not take any extra space.

Counting-Sort
-------------

Counting-sort is a sorting algorithm that doesn't actually use any comparisons. It counts how many times each possible value is encountered in A.

#### Algorithm

*Input*: An array A with the values and a value k that is an upper bound on the values to be sorted.

*Output*: An array B of sorted values.

```java
CountingSort(A,B,k)
	for i <-- 0 to k do 
		C[i] <-- 0 //C is called the count array

	for j <-- 1 to length[A] do
		C[A[j]] <-- C[A[j]]+1
	//C[i] now contains the number of elements equal to i.

	for i <-- 1 to k
		do C[i] <-- C[i] + C[i-1]
	//C[i] now contains the number of elements less than or equal to i.
	//The new C contains the same information as the one obtained from the
	// previous step, but says 

	/*This satisfies what we call stability. This means that if we have two
	numbers with the same value in our input, they will be in the same order
	in our output.
	We want this algorithm to have this property to combine it into another
	algorithm that requires stability to function. */

	//COPYING A[] INTO B[]

	for j <-- length[A] downto 1 do
		B[C[A[j]]] <-- A[j]
		C[A[j]] <-- C[A[j]] - 1

	/*Why B[C[A[j]]]? C counts the number of elements that are smaller or
	equal to a value A. If we place A[j] at the C[A[j]]-th index, then it's
	exactly where it belongs in the sorted output.*/

```

This works in linear time, but the major downside to it because the counting array C[] gets crazy if *k* has very high values.

Radix Sort
----------

Radix sort was developed by IBM even before personal computers were a thing, back when punchcards were used for calculations.

*Key idea*: Sort least significant digits first. By doing so, we don't mess up the order of previously sorted digits as we continue. This is why *stability* is so important in this case, because when we meet identical values on a certain digit, they keep their order from previous digits.


#### Algorithm
```java
radixSort(A,d)
	for i <-- 1 to d do
		stable sort array A on digit i

```
#### Proof

* Assume digits 1,2,...,i-1 are sorted.
* Show that a stable sort on digit i leaves digits 1,...,i sorted:
	- If 2 digits in position i are different, ordering by position i is correct, and positions 1,...,i-1 are irrelevant.
	- If 2 digits in position i are equal, numbers are already in the right order (by inductive hypothesis). The stable sort on digit i leaves them in the right order.

#### Runtime Analysis

Assuming that Counting Sort is the stable sort used as an intermediate:
* Θ(n+k) per pass (digits range in range 0...k)
* d passes
* Θ(d(n+k)) total
* If k = O(n), time = Θ(dn)

#### How to break each key into digits?

* *n* words
* *b* bits per word
* Break each word into *r*-bit digits. Have *d* = ceil(*b*/*r*)
* Use Counting Sort, *k* = 2<sup>*r*</sup>-1
* Time: Θ(b/r * (n + 2<sup>r</sup>))

How to choose r? Balance b/r and n + 2<sup>r</sup>. Choosing r ~ lg n gives us Θ(bn/lgn).

* If we choose r < lg n, then b/r > b/lg n, and n + 2<sup>r</sup> doesn't improve.
* If we choose r > lg n, then n + 2<sup>r</sup> gets big.

Thus, to sort 2<sup>16</sup> 32-bit numbers, use r = lg 2<sup>16</sup> = 16 bits.

#### Comparison to mergesort and quicksort

* 1 million (2<sup>20</sup>) 32-bit integers.
* Radix sort: ceil(32/20) = 2 passes.
* Mergesort/quicksort: lg (n) = 20 passes.

(Remember that each radix sort "pass" is really just two passses - one to take census, and one to move data.)

However, the downside of this is that it uses 65536 memory cells.

#### Breaking n log n limit

How does radix sort violate the ground rules for a comparison sort? It uses counting sort to gain information about keys rather than directly comparing 2 keys. Keys are used as array indices.


Quicksort
---------

Quicksort is a divide and conquer algorithm that is the fastest method used for comparison sorting. Its major drawback is that its worse case is quadratic. However, on average, it runs a lot faster than the other methods. It runs in O(n log n) time, but the hidden constant in front of quicksort is a lot smaller than other logarithmic methods (e.g. mergesort or heapsort).

#### Runtime analysis

* Worst case: Already sorted array (either increasing or decreasing)
* Recurrence relation: T(n) = T(n-1) + c*n+d


#### In-place algorithms

An algorithm is **in-place** if it uses only a constant amount of memory in addition to that used to store the input. These are important because if the data set to be sorted takes a lot of space already, we don't want an algorithm that takes up twice as much space.

* Mergesort isn't in-place because its merge procedure requires a temporary array.
* Selection sort and Insertion sort are in-place, because we're only moving elements within the input array.

#### Algorithm

**Partition step**


*Input*: An array A with indices start and stop

*Output*: Returns an index j and rearranges the elements of A such that for all i<j, A[i]≤A[j] and for all k>j, A[k]≥A[j].

```java
	pivot <-- A[stop]
	left <-- start
	right <-- stop - 1

	while left <= right do
		while (left <= right and A[left] <= pivot) do left <-- left + 1
		while (left <= right and A[right] <= pivot) do right <-- right - 1
		if (left < right) then exchange A[left] and A[right]
	exchange A[stop] and A[left]
	return left
```
![](http://i.imgur.com/ESWt07g.gif)

**Sorting step**


*Input*: An array A to sort indices start and stop.

*Output*: A[start...stop] is sorted.

```java
if (start<stop) then
	pivot <-- partition(A, start, stop)
	quickSort(A, start, pivot-1)
	quickSort(A, pivot+1, stop)

```

#### Improvements

Quicksort gets a lot slower if the array is already sorted.

A solution to this is to always find the median before sorting, but although that guarantees O(n log n) running time, it worsens the average case so that the algorithm becomes just another O(n log n) sort rather than the quickest.

To solve this, we can choose to randomize the pivot. To do so, we choose an element at a position *i* and swap it with the element at the last position before continuing with our normal Quicksort. The chances of reaching the worst-case are drastically decreased.


Strings & Pattern Matching
==========================

* The objective of string searching is to find the location of a specific text pattern within a larger body of text (e.g. a sentence, paragraph, book, etc.)
* As with most algorithms, the main considerations are speed and efficiency.

Brute Force
-----------

* The Brute Force algorithm compares the pattern to the text, one character at a time, until unmatching characters are found.
* The algorithm can be designed to stop on either the first occurrence of the pattern, or upon reaching the end of the text.

![](http://i.imgur.com/6lNMkP7.png)

#### Algorithm

```java
do
	if (text letter == pattern letter)
		compare next letter of pattern to next letter of text
	else
		move pattern down text by one letter
until (entire pattern found or end of text)

```

#### Complexity

* Given a pattern with M characters in length, and a text N characters in length...
* Worst case: compares the pattern to each substring of text of length M.
	- Total number of comparisons: M(N-M+1)
	- Worst case time complexity: O(MN)
![](http://i.imgur.com/4SPRuJ8.png)
* Best case if pattern: Finds pattern in the first M positions of text.
	- Total number of comparisons: M
	- Best case time complexity: O(M)
![](http://i.imgur.com/30IsoLw.png)
* Best case if pattern not found: Always mismatches on the first character.
	- Total number of comparisons: N
	- Best case time complexity: O(N)
![](http://i.imgur.com/30IsoLw.png)

Rabin-Karp
----------

* The Rabin-Karp string searching algorithm calculates a hash value for the pattern, and for each M-character subsequence of text to be compared.
* If the hash values are unequal, the algorithm will calculate the hash value for next M-character sequence.
* If the hash values are equal, the algorithm will do a Brute-Force comparison between the pattern and the M-character sequence.
* In this way, there is only one comparison per text subsequence, and Brute Force is only needed when hash values match.

#### Algorithm

```java

//pattern is M characters long.

hash_p //hash value of pattern
hash_t //hash value of first M letters in body of text

do
	if (hash_p == hash_t)
		brute force comparison of pattern and selected section of text

	hash_t <-- hash value of next section of text, one character over

until (end of text or brute force comparison == true)

```


#### Math

* Consider an M-character sequence as an M-digit number in base b, where b is the number of letters in the alphabet. The text subsequence t[i..i+M-1] is mapped to the number **x(i) = t[i]⋅b<sup>M-1</sup> + t[i+1]⋅b<sup>M-2</sup> +...+ t[i+M-1]**
* Furthermore, given x(i) we can compute x(i+1) for the next subsequence t[i+1...i+M] in constant time, as follows:
![](http://i.imgur.com/CO0afvM.png)
* In this way, we never explicitly compute a new value. We simply adjust the existing value as we move over one character.
* If M is large, the resulting value will be enormous. For this reason, we hash the value by taking its modulo over a prime number Q.
* The mod function (% in Java) is particularly useful in this case due to several of its inherent properties:
	- [(x mod q) + (y mod q)] = (x+y) mod q
	- (x mod q) mod q = x mod q
![](http://i.imgur.com/P09GvHT.png)
* If a sufficiently large prime number is used for the hash function, the hashed values of two different patterns will usually be distinct.
* If this is the case, searching takes O(N) time, where N is the number of characters in the larger body of text.
* It is always possible to construct a scenario with a worst case complexity of O(MN). However, this is likely to happen only if the prime number used for hashing is small.

Knuth-Morris-Pratt
------------------

* The KMP string searching algorithm differs from the brute-force algorithm by keeping track of information gained from previous comparisons.
* A **failure function** (*f*) is computed that indicates how much of the last comparison can be reused if it fails.
* Specifically, *f* is defined to be the longest prefix of the pattern P[0...j] that is also a suffix of P[1...j]. **N.B. Not a suffix of P[0...j]**.


#### Example

The value of the KMP failure function:

| j    | 0 | 1 | 2 | 3 | 4 | 5 |
|------|---|---|---|---|---|---|
| P[j] | a | b | a | b | a | c |
| f(j) | 0 | 0 | 1 | 2 | 3 | 0 |

* This shows how much of the beginning of the string matches up to the portion immediately preceding a failed comparison.
* If the comparison fails at (4), we know the a,b in positions 2,3 is identical to positions 0,1.

#### Algorithm

**Input**: Strings *T* (text) with *n* characters and *P* pattern with *m* characters.

**Output**: Starting index of the first substring of *T* matching *P*, or an indication that *P* is not a substring of *T*.

```java
KMPMatch(T,P)
	f <-- KMPFailureFunction(P) //build failure function
	i <-- 0
	j <-- 0

	while i<n do
		if P[j] == T[i] then
			if j == m-1 then
				return i - m - 1 //a match
			i <-- i+1
			j <-- j+1
		else if j>0 then //no match but we have advanced
			j <-- f(j-1) //j indexes just after matching prefix in P
		else
			i <-- i+1

	return "There is no substring of T matching P!"

```


**Input**: String *P* (pattern) with *m* characters.

**Output**: The failure function *f* for *P*, which maps *j* to the length of the longest prefix of *P* that is a suffix of P[i...j].

```java

KMPFailureFunction(P)

	i <-- 1
	j <-- 0

	while i <= m-1 do
		if P[j] == P[i] then //we have matched j+1 characters
			f(i) <-- j+1
			i <-- i+1
			j <-- j+1
		else if j>0 then //j indexes just after a prefix of P that matches
			j <-- f(j-1)
		else //there is no match
			f(i) <-- 0
			i <-- i + 1

```

#### Graphical representation

![](http://i.imgur.com/1aE96z3.png)


#### Time Complexity Analysis

* define *k = i - j*
* In every iteration through the while loop, one of three things happens:
	- if T[i] = P[j], *i* increases by 1, as does *j*. *k* remains the same.
	- if T[i] != P[j] and *j* > 0, *i* does not change and *k* increases by at least 1, since *k* changes from *i - j* to *i - f(j-1)*.
	- If T[i] != P[j] and *j* = 0, *i* increases by 1 and *j* remains the same, so *k* increases by 1.
* In each iteration of the loop, either *i* or *k* increase by at least 1, so the greatest possible number of loops is *2n*.
* This is assuming *f* has already been computed.
* However, *f* is computed in much the same manner as **KMPMatch**, so its time complexity argument is analogous. **KMPFailureFunction** is O(m).
* **Total time complexity: O(n+m).**


Regular Expressions
-------------------

Regular expressions (RegEx) is notation for describing a set of strings, possible of infinite size.

#### Notation

* **ε** denotes the empty string.
* **ab+c** denotes the set {ab,c}
* a* denotes the set {ε,a,aa,aaa,...}

#### Examples

* (a+b)&#42;: all the strings from the alphabet {a,b}
* b&#42;(ab&#42;a)&#42;b&#42; strings with an even number of a's
* (a+b)&#42;sun(a+b)&#42; strings containing the pattern "sun"
* (a+b)(a+b)(a+b)a 4-letter strings ending in a



#### Finite State Automatons

Regular expressions are computed using Finite State Automatons. FSA can be combined to create OR or AND logic. The creation of the automaton and its subsequent checking process both run in linear time.


Tries
-----

A trie is a tree-based data structure for storing strings in order to make pattern matching faster.

Tries can be used to perform prefix queries for information retrieval. Prefix queries search for the longest prefix of a given string X that matches a prefix of some string in the trie.

A trie supports the following operations on a set S of strings:
* **insert(X):** Insert the string X into S
* **remove(X):** Remove string X from S
* **prefixes(X):** Return all the strings in S that have a longest prefix of X.

Let *S* be a set of strings from the alphabet ∑ such that no String in *S* is a prefix to another string. A **standard trie** for *S* is an ordered tree *T* such that:
* Each edge of *T* is labeled with a character from ∑.
* The ordering of edges out of an internal node is determined by the alphabet ∑.
* The path from the root of *T* to any node represents a prefix in that is equal to the concantenation of the characters encountered while traversing the path.
* For example, the standard trie over the alphabet {a,b} for the set {aabab,abaab,babbb,bbaaa,bbbab}.
* ![](http://i.imgur.com/j3lzKnP.png)

An internal node can have 1 to *d* children when d is the size of the alphabet. Our example is essentially a binary tree.

A path from the root *T* to an internal node *v* at depth *i* corresponds to an *i*-character prefix of the string of S.

We can implement a trie with an ordered tree by storing the character associated with an edge at the child node below it.

#### Compressed Tries

* A **compressed trie** is like a standard trie but makes sure that each trie has a degree of at least 2. Single child nodes are compressed into a single edge.
* A **critical node** is a node v such that v is labeled with a string from S, v has at least 2 children, or v is the root.
* To convert a standard trie to a compressed trie, we replace an edge (v<sub>0</sub>, v<sub>1</sub>) by a chain of nodes (v<sub>0</sub>, v<sub>1</sub> ... v<sub>k</sub>) for k ≥ 2 such that:
	- v<sub>0</sub> and v<sub>1</sub> are critical but v<sub>i</sub> is not critical for 0 < i < k.
	- each v<sub>i</sub> only has one child.
* Each internal node in a compressed trie has at least two children and each external is associated with a string. The compression reduces the total space for the trie from O(m) where *m* is the sum of the lengths of strings in *S* to O(n) where *n* is the number of strings in S.

![Example](http://i.imgur.com/mkP2OM6.png)

#### Prefix Queries on a Trie

**Input**: Trie *T* for a set *S* of strings and a query string *X*.

**Output**: The node v of T such that the labeled nodes of the subtree of T rooted at v store the strings of S witha  longest prefix in common with X.

```java

prefixQuery(T,X)

	v <-- T.root()
	i <-- 0 //i is an index into the string X

	repeat
		for each child w of v do
			let e be the edge (w,v)
			Y <-- string(e) //Y is the substring associated with e)
			l <-- Y.length() //l=1 if T is the standard trie
			Z <-- X.substring(i,i+l-1) //Z holds the next l characters of X

			if Z == Y then
				v <-- w
				i <-- i + 1 //move to W, incrementing i past Z
				break out of for loop
			else if a proper prefix of Z matched a proper prefix of Y then
				v <-- w
				break out of the repeat loop
	until v is external or v != w

	return v
```

#### Insertion and deletion

* Insertion: We first perform a prefix query for string X. There are two ways a prefix query may end in terms of insertion:
	- The query terminates at node v. Let X<sub>1</sub> be the prefix of X that matched in the trie up to node v and X<sub>2</sub> be the rest of X. If X<sub>2</sub> is an empty string, we label v with X. Otherwise, we create a new external node w and label it with X.

	![](http://i.imgur.com/hiEWrNg.png)

	- The query terminates at an edge e=(v,w) because a prefix of X matches prefix(v) and a proper prefix of Y associated with e. Let Y<sub>1</sub> be the part of Y that X matched to and Y<sub>2</sub> the rest of Y. Likewise for X<sub>1</sub> and X<sub>2</sub>. Then X = X<sub>1</sub> + X<sub>2</sub> = prefix(v) + Y<sub>1</sub> + X<sub>2</sub>. We create a new node *u* and split the edges (v,u) and (u,w). If X<sub>2</sub> is empty, we label *u* with X. Otherwise, we create a node *z* which is external and label it X.

	![](http://i.imgur.com/sMXGqrJ.png)

* Insertion is O(dn) where *d* is the size of the alphabet and *n* is the length of the string to insert.


#### Lempel Ziv Encoding

This is an efficient way to represent strings such that the encoded strings are smaller than the strings they came from. This works in any language with a fixed alphabet, because certain characters and combinations appear much more frequently than others. (e.g. "t-h-e" will appear much more frequently than "z-z-z")

* Constructing the trie:
	- Let phrase 0 be the null string.
	- Scan through the text.
	- If you come across a letter you haven't seen before, add it to the top level of the trie.
	- If you come across a letter you've already seen, scan down the trieuntil you can't match any more characters, then add a node to the trie representing the new string.
	- Insert the pair (nodeIndex,lastChar) into the compressed string.
* Reconstructing the string:
	- Every time you see a '0' in the compressed string, add the next hcaracter in the compressed string directly to the new string.
	- For each non-zero nodeIndex, put the substring corresponding to that node into the new string, followed by the next character in the compressed string.

**Graphical representation**

![](http://i.imgur.com/zrpQJCI.png)


File compression
----------------

* Text files are usually stored by representing each chracter with an 8-bit ASCII code.
* The ASCII encoding is an example of fixed-length encoding, where each chracter is represented with the same number of bits.
* In order to reduce the space required to store a text file, we can exploit the fact that some characters are more likely to occur than others/
* **Variable-length encoding** uses binary codes of different lenths for different characters; thus, we can assign fewer bits to frequently used chracters, and more bits to rarely used characters.

#### Example of encoding

* Text: *java*
* Encoding: *a = "0", j = "11", v = "10"*
* Encoded text: *110100* (6 bits)

However, how do would decode this?
* Encoding: *a = "0", j = "01", v = "0"*
* Encoded text: *010000* (6 bits)
* is this: *java*,*jvv*,*jaaaa*... ???

We see that if we aren't careful, we can't guarantee a one-to-one mapping of our encoding, leading to multiple interpretations for  We have to guarantee that no string in our encoding is a prefix for another string.

* To prevent ambiguities in decoding, we require that the encoding satisfies the **prefix rule**, that is, no code is a prefix of another code.
	- *a = "0", j = "01", v = "0"* does not satisfy the prefix rule.
	- *a = "0", j = "11", v = "10"* satisfies the prefix rule.

* We use an encoding trie to define an encoding that satisfies the prefix rule.
	- The characters are stored at the external nodes
	- A left edge means 0
	- A right edge means 1







