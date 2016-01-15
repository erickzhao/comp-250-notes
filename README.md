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
