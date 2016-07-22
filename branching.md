# Branching

In the [previous lab](java/drawing), you used `while` to repeat a code block. In this lesson, you will learn about other statements like `while`, which control how and when a certain group of instructions is performed. We will refer to instructions that control the flow of other instructions as **statements**, and will visually represent their behavior by using a **flowchart**, as shown below.

## While statement

Before performing the code block, the value of the `while` statement's **condition** is determined. If the condition is true, the code block is performed, after which the condition is evaluated again. As long as the condition evaluates to true, the code block is repeated.

![](java/diagram/while-statement.png)

The while statement can be represented by the flowchart below.

![](java/diagram/while-diagram.png)

In the [drawing lab](java/drawing/#repeating-instructions), the condition we used `1 < 2`, which is always true, so the code block was repeated endlessly. However, there are many situations where the condition in the `while` may also be `false` - for example, you may want to stop repeating a code block when the mouse's x-position gets too close to the right side of the window.

```java
while (Window.mouse.getX() < 450) {
	Window.out.background("white");
}

System.out.println("You moved your mouse too far right.");
Window.out.background("red");
```

A single repetition of the code block is called an **iteration**. When we refer to "each iteration of the loop", we are talking about each time the given code block is run.

## If statement

An `if` statement will perform a code block one time if the condition in the statement is true. If the condition is false, it moves on to the next instruction.

![](java/diagram/if-statement.png)

There are many situations when writing programs that you will need to *conditionally* perform some set of instructions.

![](java/diagram/if-diagram.png)

For example, you may want to extend the previous program to test if you can move your mouse across the window in less than a second.

```java
while (Window.mouse.getX() < 450) {
	Window.out.background("white");
}

System.out.println("Move your mouse all the way to the left.");
Window.out.background("red");

// Wait one second.
Window.sleep(1000);

// Check if your mouse is on the left side of the screen.
if (Window.mouse.getX() < 50) {
	Window.out.background("green");
	System.out.println("You made it!");
}
```

You can also repeatedly check an `if` statement by adding it to the code block for a `while` statement.
```java
while (1 < 2) {
	// If the mouse is on the left side of the screen
	if (Window.mouse.getX() < 100) {
		Window.out.background("red");
	}

	// If the mouse is on the right side of the screen
	if (Window.mouse.getX() > 400) {
		Window.out.background("blue");
	}
}
```


## Else statement

You can add an `else` statement after an `if` statement to provide a code block to perform if the condition in the `if` statement is `false`.

![](java/diagram/if-else-statement.png)

This is equivalent to choosing between two code blocks based on the value of a condition - if it is true, run the code block for the `if`, and otherwise (if it is false) run the code block for the `else`.

![](java/diagram/if-else-diagram.png)

Here is a simple example where the background color of the window changes based on whether the mouse is on the left or right side.
```java
while (1 < 2) {
	if (Window.mouse.getX() < 250) {
		Window.out.background("red");
	}
	else {
		Window.out.background("blue");
	}
}
```

Notice that any code block can contain `if` and `else` statements, so we could even change the background based on which corner of the window the mouse is closest to.

```java
while (1 < 2) {
	// If the mouse is in the left half of the screen, choose red.
	if (Window.mouse.getX() < 250) {
		Window.out.background("red");
	}
	// Otherwise, if it's in the right half,
	else {
		// ... if the mouse is in the top half, choose green,
		if (Window.mouse.getY() < 250) {
			Window.out.background("green");
		}
		// otherwise, it's in the bottom half, so choose blue.
		else {
			Window.out.background("blue");
		}
	}
}
```

## Else if statement

An `else if` statement can be inserted after an `if` statement to provide another condition to check *only if the `if` statement's condition is false*. An `else if` must come before an `else` condition if there is one.

![](java/diagram/if-elseif-else-statement.png)

Adding `else if` statements lets you provide any additional conditions to check and code blocks to perform if the additional condition is true. The `else if` statements are checked in the order they are listed, and once a true condition is found, the `else if` code block is performed, and no subsequent `else if` or `else` is checked.

![](java/diagram/if-elseif-else-diagram.png)

Consider the following program, which changes the background based on the y-position of the mouse.
```java
while (1 < 2) {
	if (Window.mouse.getY() < 100) {
		Window.out.background("red");
	}
	if (Window.mouse.getY() < 250) {
		Window.out.background("green");
	}
	if (Window.mouse.getY() < 400) {
		Window.out.background("blue");
	}
}
```
If you run this program, you will see the background flash between colors as you move the mouse around the screen. For certain y-positions, more than one of the `if` statements has a `true` condition, so the background is changed multiple times each time the `while` statement's code block is repeated.

Compare the program above to the one below, which uses `if`, `else if`, and `else` statements.  
```java
while (1 < 2) {
	if (Window.mouse.getY() < 100) {
		Window.out.background("red");
	}
	else if (Window.mouse.getY() < 250) {
		Window.out.background("green");
	}
	else if (Window.mouse.getY() < 400) {
		Window.out.background("blue");
	}
	else {
		Window.out.background("yellow");
	}
}
```

In the program above, there is an implicit guarantee that only one of the code blocks from the `if`-`else if`-`else` statement will be performed. Once the y position of the mouse moves past `100`, the first `if` condition becomes false, but the first `else if` condition is still true. Similarly, when the y position moves past 250, the second `else if` is the first true condition in the `if`-`else if`-`else` block, so it's code block is performed and all subsequent statements (the `else`) are ignored. Finally, when the y position moves past `400`, every condition is false, so the `else` code block is performed.

## true and false

The words `true` and `false` are special values in Java, and are produced by yes/no questions like the inequality test (`<`). It is therefore equivalent to write `while (1 < 2)` and `while (true)` - both will repeat their code block endlessly. We will refer to a statement that repeats instructions this way as an *infinite loop*.

```java
// We'll be writing infinite loops this way from now on.
while (true) {
	//...
}
```
