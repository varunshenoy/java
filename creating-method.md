# Calling methods

A **program** is a sequence of instructions, where each **instruction** is the most basic unit of work that a computer can perform. For **Charles Babbage**'s **Analytical Engine** (the first concept of a general-purpose computer), instructions could either perform a simple mathematical operation, store a result in the computer's memory, retrieve information from the memory, or jump to a different instruction. By writing a sequence of instructions that the Analytical Engine could understand, **Ada Lovelace** created a program to calculate [Bernoulli numbers](https://en.wikipedia.org/wiki/Bernoulli_number) in the first known example of **programming**.

In this section, you will learn a few simple Java instructions that a modern digital computer can perform, and apply them in the [drawing lab](java/drawing) to create your first Java program.

## Method call

A common instruction in Java is the **method call**, which instructs the computer to perform a sequence of instructions that it already knows. These predetermined sequences are called **methods**, and telling a computer to perform a method is known as *calling the method*.

Here is an example where we instruct the computer to write `Hello, world!` onto the screen.

![](java/diagram/method-print.png)

The method above is informally called the **print method**, where *print* means "display a line of text". The print method requires us to specify exactly what is to be printed out - the information it requires is called the method's **arguments**, but for our purposes we will use the more obvious term **input**. Inputs are given in parentheses after the method name.

It is important to notice that every instruction ends with a **semicolon**, similar to how every English sentence ends with a period. The semicolon indicates the end of the instruction - many programming languages use the semicolon as the **delimiter** between instructions.

Here is a method for drawing a circle.

![](java/diagram/methods.png)

This method takes multiple inputs separated by commas, and each input corresponds to a different metric of how the circle is drawn. The first number is the circle's x position, the second input is the y position, and the third input is the radius. So the instruction `Window.out.circle(50, 100, 20)` will produce a circle at position (50, 100) with radius 20.

![](java/diagram/method-inputs.png)

## Method return value

Along with taking inputs, a method can produce an output. Consider the following [pseudocode](cs/thinking) example:

```
define square of number as
	return number * number

print square of 4
draw circle at (square of 5, square of 6) with radius square of 7
```

In this pseudocode, I define a method named `square`, which takes `number` as an input. The method *returns* the product of `number` and itself - the square of the number. Once I've defined the method by which an input is converted to an output, the computer can calculate that `square of 4` means `16`, and therefore print the number `16` instead of the literal text "square of 4", and make a circle at the position (25, 36) with radius 49. In both cases, you are using the **return value** of a method as the input to another method.

Here is another pseudocode example with a method that takes two inputs.

```
define greatest of x, y as
	if x is greater than y, then
		return x
	otherwise,
		return y


print greatest of 10, 7
```

Given two inputs, the `greatest` method will return the first input `x` if `x` is greater than the second input `y`, and return `y` otherwise. When the instructions above are followed by a computer, the expression `greatest of 10, 7` will be replaced by the number `10`, simplifying the instruction to `print 10`.

The methods `Window.mouse.getX` and `Window.mouse.getY` return the x and y position of the mouse. Notice that the y-axis is **inverted**, so the y-position is measured from the top of the screen and increases as you move the mouse *down*.

![](java/diagram/window-mouse.png)

## Code blocks

A **code block** is a group of instructions. Recall the pseudocode example from the previous chapter for "99 Bottles of Beer".
```
set n to 99
while n is greater than 0,
    sing n
    ...
    reduce n by 1
```

The instructions from `sing n` to `reduce n by 1` are a code block, and pseudocode indicates this by indenting them with a tab. In Java, the curly braces `{` and `}` are used to indicate the start and end of a code block.

```
while n is greater than 0,
{
    sing n
    ...
    reduce n by 1
}
```

Notice how there is an open curly brace `{` before the first instruction of the code block, and a closed curly brace `}` after the last instruction.

## Comments

A **comment** is any text in a program that is ignored by the computer. In Java, a comment begins after two slashes (`//`). Any code written after the `//` will be ignored, and is primarily for the purpose of annotating code with human-readable comments.

```java
// Draw a small circle in the center of the screen.
Window.out.circle(250, 250, 40);
```  
