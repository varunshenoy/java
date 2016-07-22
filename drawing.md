# Drawing

In this lab, you will program a simple application for drawing lines and shapes with your mouse. The goal of this lab is to familiarize yourself with the `Window.out` drawing methods and the general ideas behind animating with the [APCS library](http://apcs.io).

![](java/drawing/example.gif)

## Getting started

[Create a Java class](java/start/#create-a-file) named `Drawing`. In these labs, the Java code we provide you is for the code block following `public static void main(String[] args) { }`, more commonly referred to as the **main method**.

```java
public class Drawing {
	public static void main(String[] args) {
		// Your code here
	}
}
```

Recall that a [method](java/calling-methods/#method-call) in Java is **called** by writing the name of the method with specific **arguments**, as shown below.

![](java/diagram/methods.png)

The **parameters** of the `Window.out.circle` method are `x`, `y`, and `radius` (in that order), where `x` and `y` give the center coordinate of the circle, and `radius` determines the size of the circle. Try using `Window.out.circle` to output a circle, and change the inputs to see the size and position of the circle change.

```java
public class Drawing {
	public static void main(String[] args) {
		Window.out.circle(100, 200, 50);
	}
}
```

Notice that the y-axis is *inverted* - that is, as you make the input for `y` larger, the circle is placed farther down the window.

#### Simple circle challenge

The default window size is 500 pixels by 500 pixels. What inputs would you need to give to <code>Window.out.circle</code> in order to create a circle that is inscribed perfectly within the window?

![challenge](java/drawing/circle-challenge.png)

## Changing colors

You can add multiple calls to `Window.out.circle` to output more than one circle, as shown below. You can also change the drawing color with `Window.out.color`, as shown below. When you change the drawing color, you are effectively picking a new color for all subsequently drawn shapes to be colored in. It's the same as when you draw in real life - pick up a marker of a certain color, draw whatever you want with it, then pick up a different marker and draw whatever you want in that other color.

Notice that the input to `Window.out.color` is a `String` and therefore in quotation marks, just like the input to `System.out.println`.

```java
public class Drawing {
	public static void main(String[] args) {
		Window.out.color("red");
		Window.out.circle(100, 100, 50);

		Window.out.color("yellow");
		Window.out.circle(200, 200, 100);

		Window.out.color("green");
		Window.out.circle(300, 300, 150);

		Window.out.color("blue");
		Window.out.circle(400, 400, 200);
	}
}
```

![](java/drawing/circle-demo.png)

You can also use `Window.out.background` to set the background color of the window, as shown below.
```java
Window.out.background("white");
```

Notice that the order in which you set colors and draw shapes determines how they layer on top of each other. Computer instructions are run in order, so the circles drawn later in your code show up on top of the circles drawn earlier.

![](java/drawing/rainbow-challenge.png)

Draw the rainbow as shown. After you draw a white background, create circles for each of the colors of the rainbow (red, orange, yellow, green, blue, indigo, and violet).

## Animating

Computer animations are created by displaying images separated by a small pause. You can instruct the computer to wait for a given period of time with the `Window.sleep` method, which takes a number of milliseconds as an input.

```java
// Draw a circle
Window.out.circle(100, 200, 50);
// Wait for 50 milliseconds
Window.sleep(50);
// Draw another circle
Window.out.circle(200, 300, 50);
```

For example, suppose you wanted to animate a red ball moving horizontally across the screen. Each individual image of the animation, called a **frame**, draws a red ball on a white background as shown below.   

```java
Window.out.background("white");
Window.out.color("red");
Window.out.circle(100, 100, 50);
```

We can animate the ball's motion by repeating the three instructions above for drawing a single frame with the ball, separated by a 100 millisecond pause. In each frame, the ball has a slightly larger `x` position.

```java
Window.out.background("white");
Window.out.color("red");
Window.out.circle(100, 100, 50);	// x coordinate starts at 100

Window.sleep(100);

Window.out.background("white");
Window.out.color("red");
Window.out.circle(120, 100, 50);	// x coordinate is now 120

Window.sleep(100);

// copy and paste code for more frames here ...

Window.out.background("white");
Window.out.color("red");
Window.out.circle(280, 100, 50);	// x coordinate is now 280

Window.sleep(100);

Window.out.background("white");
Window.out.color("red");
Window.out.circle(300, 100, 50);	// x coordinate is now 300
```

#### Heartbeat challenge

Animate a heartbeat, where a circle expands and contracts in a *lub dub* motion.

![](java/drawing/heartbeat.gif)

## Mouse input

Suppose we made an animation like the one from the previous section, but in the code to draw each frame, we used the mouse's x and y position as the x and y position of the red ball. Recall that [methods can return data](java/calling-methods#method-return-value), so we can use them as the inputs to other methods.

Recall from the reading that you can get the coordinates of the mouse with `Window.mouse.getX` and `Window.mouse.getY`, and that you can use the result of a method as the input to another method.

```java
Window.out.circle( Window.mouse.getX() , Window.mouse.getY() , 20);
```

![](java/diagram/window-mouse.png)

Notice the parentheses at the end of `Window.mouse.getX()`. The method `Window.mouse.getX` does not need any inputs, but you must still write the open and closed parentheses `()` to indicate that you are **calling** the method.

We can modify our code for drawing a frame to use the mouse position as the position of the red ball.
```java
Window.out.background("white");
Window.out.color("red");
Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);

Window.sleep(100);	// Wait 100 milliseconds between frames - try changing this!
```

Now, we can copy-and-paste the code to draw a frame as many times as we want.

```java
Window.out.background("white");
Window.out.color("red");
Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);

Window.sleep(100);

Window.out.background("white");
Window.out.color("red");
Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);

Window.sleep(100);

Window.out.background("white");
// copy and paste as many times as you want ...
```

## Repeating instructions

Instead of copying-and-pasting instructions to repeat them a fixed number of times, you can repeat a group of instructions [as long as a given condition is true](cs/thinking/#thinking-in-repetitions). The code that does this translates informally to "while some statement is true, draw a frame".

First, we need to create a **code block** for drawing a single frame, by enclosing the instructions in a set of open and closed curly braces (`{` and `}`).

```java
{
	Window.out.background("white");
	Window.out.color("red");
	Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
	Window.sleep(100);
}
```  

Before the open curly brace `{`, we can specify *when should the code block be performed*. It would be nice to be able to write `forever { ... }`, but in Java there are a limited number of words that can be used to describe when a code block should be performed, and `forever` is not one of them. One word that is allowed is `while`.

```java
while ( condition ) {
	// code
}
```
The **condition** has to be some true or false mathematical statement that Java can understand. For example, the condition could be "a certain number is less than a certain other number", because a number either is or isn't less than another one. With such a condition, we could represent the concept of "forever, do this code" as "while 1 is less than 2, do this code". One is always less than two, so the code will be repeated indefinitely.    

```java
while ( 1 < 2 ) {
	// code
}
```

So we could write the code to repeatedly draw frames as shown below. Notice the parentheses around `(1 < 2)` - the while loop's true or false statement must always be enclosed in parentheses.

```java
while (1 < 2) {
	Window.out.background("white");
	Window.out.color("red");
	Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
	Window.sleep(100);
}
```

Note that there are many ways we could write an always-true mathematical statement in Java.

```java
// while 1 is less than 2
while ( 1 < 2 ) { ... }
// while 1 plus 1 is 2
while ( 1 + 1 == 2) { ... }
// while 2 plus 3 is less than or equal to 5
while ( 2 + 3 <= 5 ) { ... }
// while 9 is greater than 8
while ( 9 > 8 ) { ... }
// while 1 is less than 2 and 2 is less than 3
while ( 1 < 2 && 2 < 3 ) { ... }
// while 5 is greater than 0 or 100 is less than 0
while ( 5 > 0 || 100 < 0 ) { ... }
```

Your entire program at this point should look something like this.

```java
import apcs.Window;

public class Drawing {
	public static void main(String[] args) {
		while (1 < 2) {
			Window.out.background("white");
			Window.out.color("red");
			Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
			Window.sleep(100);
		}
	}
}
```

#### Drawing challenge

How would you modify this code to draw a continuous line of overlapping circles, rather than erasing the red ball when redrawing the frame?

![](java/drawing/drawing-challenge.gif)

#### Drawing challenge solution

Perform the instruction for setting the background once, then indefinitely repeat the instructions for drawing the circle. Modify the radius of the circle you draw to adjust the size of the drawing pen, and the sleep interval to adjust the amount of time between redrawing the circle.

```
Window.out.background("white");

while (1 < 2) {
	Window.out.color("red");
	Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
	Window.sleep(10);
}
```

You could also perform `Window.out.color("red")` only once.

```
Window.out.background("white");
Window.out.color("red");

while (1 < 2) {
	Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
	Window.sleep(10);
}
```

To have the circle redraw as fast as possible, you could remove the `Window.sleep`, so the line is drawn as continuously as possible.

```
Window.out.background("white");
Window.out.color("red");

while (1 < 2) {
	Window.out.circle(Window.mouse.getX(), Window.mouse.getY(), 50);
}
```

#### Multiple pen challenge

Several colors draw simultaneously around the location of your mouse. Remember that increasing the x-position of a circle draws it farther to the right, and increasing the y-position draws it farther downwards.

![](java/drawing/multi-pen-challenge.png)
