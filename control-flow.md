# Control flow

In this lab, you will continue to build on the drawing application you developed in the [first lab](java/drawing) to add interactive features and user input.

![](java/control-flow/palette-challenge.gif)

## Getting started

[Create a new Java file](java/start/#creating-a-file) named `DrawingApp`, and add the basic drawing code from the [last lab](java/drawing) for continuously drawing a red circle at the mouse position.

```java
public class DrawingApp {
	public static void main(String[] args) {
		Window.out.background("white");

		while (1 < 2) {
			Window.out.color("red");
			Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
			Window.sleep(10);
		}
	}
}
```

Suppose you want to draw in red when you are drawing on the left side of the screen, and in blue when you are coloring on the right side of the screen. Instead of the instruction `Window.out.color("red")`, you can use an `if` and `else` to choose between `Window.out.color("red")` and `Window.out.color("blue")` based on the value of `Window.mouse.getX()`.

```java
// If the mouse x position is less than 250, then...
if (Window.mouse.getX() < 250) {
	// ... draw in red, ...
	Window.out.color("red");
}
// otherwise, ...
else {
	// ... draw in blue.
	Window.out.color("blue");
}
```

Think of this as replacing an instruction with a statement that selects which other instructions to run. You can try this application and modify it using the code shown below.

```java
public class DrawingApp {
	public static void main(String[] args) {
		Window.out.background("white");

		while (1 < 2) {
			// Pick color based on mouse position.
			if (Window.mouse.getX() < 250) {
				Window.out.color("red");
			}
			else {
				Window.out.color("blue");
			}
			// Draw a circle.
			Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
			// Wait for 10 milliseconds.
			Window.sleep(10);
		}
	}
}
```

#### Four color drawing challenge

Extend the code from the previous example to draw a different color for each *quadrant* of the window.

![](java/control-flow/example.gif)

One intuitive way to phrase this logic in pseudocode is shown below.

```
if mouse is on left half of screen,
	if mouse is on top half of screen,
		use color 1
	otherwise,
		use color 2
otherwise,
	if mouse is on top half of screen,
		use color 3
	otherwise,
		use color 4
```

## Mouse clicks

The method `Window.mouse.clicked` takes no inputs, and returns `true` or `false` based on whether the mouse is clicked. The return value of `Window.mouse.clicked` can be used as the condition to an `if` statement.

```java
if (Window.mouse.clicked()) {
	...
}
```

There are many ways to incorporate `Window.mouse.clicked` into your program - here's an example where the pen size becomes larger when the mouse is clicked.

```java
if (Window.mouse.clicked()) {
	Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
}
else {
	Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 10);
}
```

The only difference between the instruction in the `if` code block and the one in the `else` code block is the radius of the circle being drawn.

![](java/control-flow/pen-size-example.gif)

## Keyboard input

Just as `Window.mouse` methods return information about the mouse, `Window.key` methods return information about the keyboard. `Window.key.pressed` returns `true` or `false` based on whether a certain key is pressed. For example, `Window.key.pressed("a")` returns `true` if the "a" key is pressed, and `false` otherwise.

In the example program below, the pen draws in blue only if the "b" key is pressed, and in red otherwise.
```java
public class DrawingApp {
	public static void main(String[] args) {
		Window.out.background("white");

		while (1 < 2) {
			// Pick color based on whether the "b" key is pressed.
			if (Window.key.pressed("b")) {
				Window.out.color("blue");
			}
			else {
				Window.out.color("red");
			}
			// Draw a circle.
			Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
			// Wait for 10 milliseconds.
			Window.sleep(10);
		}
	}
}
```

## Drawing other shapes

You can draw a square with the `Window.out.square` method, which takes three inputs - the `x` and `y` position of the center, and the square's `side` length. So to draw a square at the same position as the circle, you can use
```java
Window.out.square(Window.mouse.getX(), Window.mouse.getY(), 20);
```

You can also draw rectangles by providing four inputs - an (`x`, `y`) position, a `width`, and a `height`. Notice that the `x` and `y` coordinate represent the center of the rectangle.
```java
Window.out.rectangle(x, y, width, height);
```

#### Keys to colors challenge

Map the keys on the keyboard to distinct colors, so pressing a certain key changes the drawing color to it's corresponding color.

![](java/control-flow/key-color-challenge.gif)

#### Rectangle challenge

Calculate the position/size of four rectangles to create the given animation that follows the mouse pointer.

![](java/control-flow/rectangle-challenge.gif)

*Hint: To draw the red rectangle shown in the animation, you can draw a rectangle centered at (`Window.mouse.getX() / 2`, `Window.mouse.getY() / 2`).*
```
Window.out.color("red");
Window.out.rectangle(Window.mouse.getX() / 2, Window.mouse.getY() / 2, Window.mouse.getX(), Window.mouse.getY());
```
How would you calculate the center, width, and height of the other three rectangles?

#### Color palette challenge

Clicking on certain colored areas in the screen will change the pen color to that color.

![](java/control-flow/palette-challenge.gif)

First, you will need to use `Window.out.square` and `Window.out.rectangle` to draw a color palette on the top of the window. Think about whether drawing the color palette should happen in the `while` loop's code block, or before it.

To change colors reliably, only call the `Window.out.color` method when the drawing color is to be changed. Changing the color should only happen when the mouse is clicked and if the mouse x and y position are within a certain boundary. For example, the code below only changes the color to red if the top left corner of the screen is being clicked.

```java
if (Window.mouse.clicked()) {
	if (Window.mouse.getX() < 100) {
		if (Window.mouse.getY() < 50) {
			Window.out.color("red");
		}
	}
}
```
