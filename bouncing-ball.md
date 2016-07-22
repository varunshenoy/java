# Bouncing Balls

In this lab, you will transition from procedural to *object-oriented* programming by creating a bouncing ball simulator.

![]({{url}}/img/lab/bouncing-balls/multiradius-ball-challenge.gif)

## Prerequisites

-[**object-oriented programming**]({{url}}/chapter/3/section/1) - an *object* is a virtual representation of something.
-[**classes**]({{url}}/chapter/3/section/1/#classes) - a *class* is a virtual description used to create objects.

## Getting started

Create a Java class named `BouncingBalls`.
```java
public class BouncingBalls {
	public static void main(String[] args) {

	}
}
```

Start by creating a single ball in a random position on the screen. Since the ball needs to move, we will store its position with an `x` and `y` variable.

```java
public class BouncingBalls {
	public static void main(String[] args) {
		int x = Window.rollDice(500);
		int y = Window.rollDice(500);

		Window.out.circle(x, y, 10);
	}
}
```

To animate the ball's motion, we can put the drawing of each frame in an animation loop, change the `x` and `y` position of the ball after drawing each frame, and use the `Window.frame` method to control the speed and eliminate any flickering.
```java
int x = Window.rollDice(500);
int y = Window.rollDice(500);

while (true) {
	Window.out.background("white");

	// Draw the ball.
	Window.out.color("red");
	Window.out.circle(x, y, 10);

	// Move the ball.
	x = x + 3;
	y = y + 1;

	Window.frame();
}
```

To have the ball move with a random initial speed, we can create two more variables `xspeed` and `yspeed`, and use them to store the rate of change for `x` and `y`. To get a random number between -5 and 5 for each value, we can roll an 11-sided dice and subtract 6 from the result.  
```java
int x = Window.rollDice(500);
int y = Window.rollDice(500);

// Adjusts the range from [1, 11] to [-5, 5]
int xspeed = Window.rollDice(11) - 6;	 
int yspeed = Window.rollDice(11) - 6;

while (true) {
	Window.out.background("white");
	Window.out.color("red");
	Window.out.circle(x, y, 10);

	// Notice that the position now changes by the corresponding speed.
	x = x + xspeed;
	y = y + yspeed;

	Window.frame();
}
```

Eventually, the ball will have an (`x`, `y`) position that is off the screen. To ensure the ball stays in the window, you can *invert* `xspeed` and `yspeed` so the ball position always stays within bounds. In the example below, the "x" key inverts `xspeed`, and "y" inverts `yspeed`.  

```java
int x = Window.rollDice(500);
int y = Window.rollDice(500);

// Adjusts the range from [1, 11] to [-5, 5]
int xspeed = Window.rollDice(11) - 6;	 
int yspeed = Window.rollDice(11) - 6;

while (true) {
	Window.out.background("white");
	Window.out.color("red");
	Window.out.circle(x, y, 10);

	// Notice that the position now changes by the corresponding speed.
	x = x + xspeed;
	y = y + yspeed;

	if (Window.key.pressed("x")) {
		xspeed = xspeed * -1;
	}

	if (Window.key.pressed("y")) {
		yspeed = yspeed * -1;
	}

	Window.frame();
}
```

<div class="challenge row">
	<div class="col-sm-6 image">
		<img src="{{url}}/img/lab/bouncing-balls/single-ball-example.gif">
	</div>
	<div class="col-sm-6 text">
		<strong>Bouncing off edges</strong><br>
		How would you make the ball bounce automatically when it reaches the edge of the window?
		<button class="solution"><i class="fa fa-lightbulb-o"></i> show hint</button>  
	</div>
	<div class="col-sm-12 solution text">
<p>Check if the <code>x</code> and <code>y</code> positions of the ball are "too big" or "too small". For example, if <code>x</code> has a negative value, then it is being drawn off the screen.</p>
		<pre><code class="java">
if (x < 0) {
	xspeed = xspeed * -1;
}
		</code></pre>
	</div>
</div>

## Creating multiple balls

If you were to make another ball in the window, you would have to make four additional variables to store the position and speed of that ball. This means that a simulation like the one from the example would require hundreds of variables and thousands of lines of code to draw and move each one.

Instead, lets use a *class* to describe a single ball, and create any number of ball *objects* from that class. [Create a Java class]({{url}}/chapter/1/section/1#creating-a-java-file) named `Ball`, and for this class, **don't check the `public static void main`** box.

In `Ball.java`:
```java
public class Ball {

}
```

We're now going to move the code to move a single ball into our general description for how balls should work. In Eclipse, you can easily work with two files side-by-side by clicking on a tab and dragging it to form a new panel.

![]({{url}}/img/lab/bouncing-balls/eclipse-multi-window.gif)

In a class, we have to describe what *data* each object stores and what *methods* each object can perform. In this case, each ball needs to store its own copy of `x`, `y`, `xspeed`, and `yspeed`, and needs to be able to draw and move itself.

First, let's add data to the `Ball` class.

```java
public class Ball {
	// Every ball has these four variables.
	int x = Window.rollDice(500);
	int y = Window.rollDice(500);
	int xspeed = Window.rollDice(11) - 6;
	int yspeed = Window.rollDice(11) - 6;
}
```

Every time we make a `Ball` object from this description, the object will get its own set of four variables named `x`, `y`, `xspeed`, and `yspeed`.

We now need to add methods to `draw` and `move` the `Ball` object.

```java
public class Ball {
	// Every ball has these four variables.
	int x = Window.rollDice(500);
	int y = Window.rollDice(500);
	int xspeed = Window.rollDice(11) - 6;
	int yspeed = Window.rollDice(11) - 6;

	// When the object draws itself, it sets the color to red, then
	// draws a circle at its own x, y coordinate.
	public void draw() {
		Window.out.color("red");
		Window.out.circle(x, y, 10);
	}

	// When the object moves itself, it changes its x, y coordinate
	// by xspeed and yspeed.
	public void move() {
		x = x + xspeed;
		y = y + yspeed;

		if (x < 0) {
			xspeed = xspeed * -1;
		}
		// check other edges for bouncing ...
	}
}
```

From the class to describe a single ball, you can create a `Ball` object to work exactly like the procedurally-programmed ball currently in `BouncingBalls.java`.

First, you will need to [create an object]({{url}}/chapter/3/section/1#creating-an-object) from the `Ball` class.

```java
Ball b = new Ball();
```  

In the `while` loop, this ball needs to `draw` itself and `move` itself by [calling the respective class method]({{url}}/chapter/3/section/1#class-methods).

```java
// ...
while (true) {
	b.draw();
	b.move();
	// ...
}
```

So the single bouncing ball in `BouncingBalls.java` could be created by either writing a procedural program, or an object-oriented one.

<div class="row">
	<div class="col-md-6">
<pre><code class="java">
int x = Window.rollDice(500);
int y = Window.rollDice(500);
int xspeed = Window.rollDice(11) - 6;	 
int yspeed = Window.rollDice(11) - 6;

while (true) {
	Window.out.background("white");
	Window.out.color("red");
	Window.out.circle(x, y, 10);

	x = x + xspeed;
	y = y + yspeed;

	if (x < 0) {
		xspeed = xspeed * -1;
	}
	// check other edges ...

	Window.frame();
}
</code></pre>
	</div>
	<div class="col-md-6">
<pre><code class="java">
Ball b = new Ball();

while (true) {
	Window.out.background("white");

	b.draw();
	b.move();

	Window.frame();
}
</code></pre>
	</div>
</div>

The object-oriented program actually requires *more* code, because it now consists of the program on the right along with all the code in the `Ball.java` file. However, it will gradually require less code than the procedural program on the right if we add more balls to the simulation.

```java
Ball b = new Ball();
Ball b2 = new Ball();
Ball b3 = new Ball();

while (true) {
	Window.out.background("white");

	b.draw();
	b.move();

	b2.draw();
	b2.move();

	b3.draw();
	b3.move();

	Window.frame();
}
```

However, this approach still requires three lines of code for each additional ball.

## Making a list of balls

Instead of creating a new variable for each ball, we can make a list of balls and tell each ball in the list to `draw` and `move` itself. The *data type* for a list of `Ball` objects is `Ball[]`, and we can create a list to store 100 `Ball` objects with
```java
Ball[] list = new Ball[100];
```

The `list` variable stores a list of 100 references to `Ball` objects, like a phonebook that can contain references of up to 100 different people. Right now, it just contains 100 empty spaces.

You can refer to the first space in the list with `list[0]`, the second space with `list[1]`, and so on. Unlike humans, who start counting at 1, computers start counting from 0. One consequence of starting from 0 is that the last space in a list of 100 items is `list[99]`, **not** `list[100]`.

For the sake of clarity, here is a tedious way to put `Ball` objects in each space and tell each object to `draw` and `move`.
```java
Ball[] list = new Ball[100];
list[0] = new Ball();
list[1] = new Ball();
list[2] = new Ball();
// ...

while (true) {
	Window.out.background("white");
	list[0].draw();
	list[0].move();
	list[1].draw();
	list[1].move();
	// ...

	Window.frame();
}
```

A shorter way to put a `Ball` object into each space in the list would be to create a variable named `i` that goes from `0` to `99`, and put a new `Ball` object in the list at the position `i`. Recall that a number can *iterate* over a range of values with a `while` loop, as shown below.
```java
int i;						// Create an integer for iterating

i = 0;						// Start i at 0
while (i < 100) {			// While i is between 0 and 99
	// use variable i
	i = i + 1;				// increase i by 1
}
```

Here is how to apply this idea to the bouncing ball simulation.
```java
int i;
Ball[] list = new Ball[100];

i = 0;
while (i < 100) {
	list[i] = new Ball();
	i = i + 1;
}

while (true) {
	Window.out.background("white");

	i = 0;
	while (i < 100) {
		list[i].draw();
		list[i].move();
		i = i + 1;
	}

	Window.frame();
}
```

At this point, you should be able to simulate multiple red balls bouncing around in your window.

![]({{url}}/img/lab/bouncing-balls/multi-ball-example.gif)

Suppose you want to adjust the number of balls in the simulation from `100` to `200`. You would first have to change `Ball[] list = new Ball[200]`, and both instances of `while (i < 100)` to `while (i < 200)`. One way to simplify this is to use `list.length`, so instead of writing `while (i < 200)`, you could write `while (i < list.length)`.
```java
i = 0;
while (i < list.length) {
	list[i] = new Ball();
	i = i + 1;
}
```

<div class="challenge row">
	<div class="col-sm-6 image">
		<img src="{{url}}/img/lab/bouncing-balls/multicolor-ball-challenge.gif">
	</div>
	<div class="col-sm-6 text">
		<strong>Random color</strong><br>
		Each ball picks a random color from a set of possible colors.
		<button class="solution"><i class="fa fa-lightbulb-o"></i> show hint</button>  
	</div>
	<div class="col-sm-12 solution text">
<p>Each ball gets a new random <code>int</code> value named <code>color</code> that is set to the result of rolling an <em>n</em>-sided dice. In the <code>draw</code> method, pick the drawing color based on the numeric value of <code>color</code> (e.g. <code>1</code> corresponds to red, <code>2</code> to green, and so on).</p>
	</div>
</div>

<div class="challenge row">
	<div class="col-sm-6 image">
		<img src="{{url}}/img/lab/bouncing-balls/multiradius-ball-challenge.gif">
	</div>
	<div class="col-sm-6 text">
		<strong>Random radius</strong><br>
		Each ball picks a random radius, and only bounces when its edge is exactly on the border of the window.  
		<button class="solution"><i class="fa fa-lightbulb-o"></i> show hint</button>  
	</div>
	<div class="col-sm-12 solution text">
<p>Pick a <code>radius</code> value using a dice roll, and modify the edge-detection in the <code>move</code> method to bounce when the center of the circle is less than <code>radius</code> pixels from the edge of the screen. You may also need to modify the code that randomizes the starting <code>x</code> and <code>y</code> to ensure that the balls start off in the center of the screen, otherwise a few may start off stuck on the edge.</p>
	</div>
</div>

<div class="challenge row">
	<div class="col-sm-6 image">
		<img src="{{url}}/img/lab/bouncing-balls/customizable.png">
	</div>
	<div class="col-sm-6 text">
		<strong>Easily customizable</strong><br>
		Make it easy to change the size of the window and number of balls in the simulation.  
		<button class="solution"><i class="fa fa-lightbulb-o"></i> show hint</button>  
	</div>
	<div class="col-sm-12 solution text">
<p>Use <code>Window.size(width, height)</code> to adjust the size of the window, and use <code>Window.width()</code> and <code>Window.height()</code> in the edge detection conditions for the <code>move</code> method in the <code>Ball</code> class.</p>
	</div>
</div>

## Balls detect each other

What you currently have is not a complete bouncing ball simulator, because the balls can't bounce off each other. The easiest way to make balls bounce off each other is to loop through   
