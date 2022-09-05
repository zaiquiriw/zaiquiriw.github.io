---
share: true
category: digilog
tags: []
---


# Lecture 4: Logic Gates
> [!seealso] Howdy
> I recently set up github pages publishing so I can host my notes. I'm doing it mostly for [[Machine Learning|Machine Learning]]. These notes continue from [[digilog-lec-3]]

## Truth Tables

| #   | AND | OR  | NOT |
| --- | --- | --- | --- |
| 00  | 0   | 0   | 11  |
| 01  | 0   | 1   | 10  |
| 10  | 0   | 1   | 01  |
| 11  | 1   | 1   | 00  | 

![[digilog-lec-4 2022-08-31 17.46.55.excalidraw]]

## Multiple Variables and Mixed Gates

There is a way to simplify the study of logic gates, in general using Boolean algebra. If we wanted to study the function:$$F(X,Y,Z) = X Y + \overline Y Z$$
>[!seealso]  Latex Come On (Blame Mathjax)
> Well it looks like I can't make the bar look good in minimal clicks... [stackoverflow](https://tex.stackexchange.com/questions/357922/non-combining-overline)

## Waveform Behavior View
![[Logic+Gate+Symbols+and+Behavior.jpg|450]]

### Example
![[450|450]]

We can use a truth table to decide how the behavior of this algorithm works, but we can really just use some sort of algebraic function to determine this. For one, we could find an expression for each scenario:
- The Light is On If:
	- A and C are closed, OR
	- B And C are closed Or
	- A and B and C are closed
- $L = A\cdot C + B \cdot C + A \cdot B \cdot C$
- We need 6 gates for this at first (AND, OR, AND, OR, AND, AND)

We need less though, optimized it is just $C\cdot (A + B)$. The truth table should give the same behavior.

## Combinational Circuits
Logic functions like described before are implemented using circuits composed of:
- *Items:* An item is a circuit which implements some logical function
- *Nodes:* A node is a connection or wire that connects to external input, output, or between inner items

There are two classes of digital logic circuits:
- Combinational Circuits: the output depends only on the current values of the input (memoryless)
- [ ] TODO: Sequential Circuits: Idk I was on discord

Below is an anki example, just for showing off how the export to anki works
> Q: What is an Item in Combinational Circuits
> A: An item in a circuit which implements some logical function

Combinational circuits must meet three conditions
- Every circuit element is itself combinational (which means those logic thingies lol)
- Every node of the circuit is either designated as an input to the circuit or connects to exactly one output terminal of a circuit element
- The circuit contains no cyclic paths, every path through the circuit visits each circuit node at most once.

## Boolean Algebra
We have the ability to hold the values of 1 and 0 as True and False, and can use AND, OR, and NOT.

### Order Precedence 
- Parenthesis
- Not
- AND
- OR

Example:
$$\bar X + Y \cdot Z$$

### Boolean Expression
Boolean expressions are formed by applying operators to Boolean variables. They can be represented by equations, logic gate diagrams, and a truth table.


### Duality of Boolean Expressions
The dual of an algebraic expression is obtained by interchanging + and $\cdot$ and interchanging 0's and 1's. This is the definition then of the *dual*. Note that when you do this, you preserve the order of operations from the original.


