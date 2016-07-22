# Physics simulation

In this lab, you will simulate objects falling due to gravity, and model other real-world behaviors like bouncing and deflection. The goal of this lab is to strengthen your understanding of animations, the `Window` library, `if` conditions, and variables.

## Bouncing ball

Create a Java file named `BouncingBall`, and draw a ball in the top left corner of the screen.

```java
public class BouncingBall {
	public static void main(String[] args) {
		Window.out.background("white");
		Window.out.color("red");
		Window.out.circle(100, 100, 20);
	}
}
```

The ball will move around the screen and bounce off the edges. The abstract goal that we have in mind is "move the ball around", and the concrete way to achieve this is to use variables to store the ball's position. In general, *whenever you need some value to change, use a variable*.

Create two integer variables named `x` and `y`, to represent the coordinates of the circle's center - both start with a value of `100`. Once you have created these two variables, you can use them as inputs to the `Window.out.circle` method.

```java
int x = 100;
int y = 100;

Window.out.background("white");
Window.out.color("red");
// Notice how this now uses x and y
Window.out.circle(x, y, 20);
```

## Animating change

With a `while` loop, we can repeat the steps to draw the circle, and wait for 30 milliseconds between each frame of the animation. After each frame has been drawn, we can move the position of objects in the frame (in this case, just the ball) by incrementing `x` and `y`.  
```java
int x = 100;
int y = 100;

while (1 < 2) {
	// Draw the frame.
	Window.out.background("white");
	Window.out.color("red");
	Window.out.circle(x, y, 20);

	// Move the ball by changing it's x and y position.
	x = x + 1;
	y = y + 1;

	// Wait for 30 milliseconds before starting the next frame.
	Window.sleep(30);
}
```

The ball moves like the one on the left, in a single direction. A ball that was falling down as if by gravity would fall more like the ball on the right.
![](java/physics/compare-motion.png)

To have an object fall as if it is being pulled by gravity, it is important to conceptually understand *what gravity is*. Gravity *accelerates* an object by increasing its velocity at a fixed rate, where *velocity* is the rate at which an object's position changes. In the code example above, `y` increases at a fixed rate of `1` *pixel per frame*. If the red ball were to accelerate, its speed (currently fixed at `1`) would need to be able to increase.

*When you need a value to change, use a variable.*
```java
int x = 100;
int y = 100;
int speed = 1;

while (1 < 2) {
	// Draw the frame.
	Window.out.background("white");
	Window.out.color("red");
	Window.out.circle(x, y, 20);

	// Move the ball by changing it's x and y position.
	x = x + 1;
	y = y + speed;

	// Gravity makes the falling speed increase in each frame.
	speed = speed + 1;

	// Wait for 30 milliseconds before starting the next frame.
	Window.sleep(30);
}
```

## Bouncing

Suppose you want the ball to bounce off the bottom of the screen. To bounce, we must check if the `y` position of the ball has exceeded the bottom of the screen, and *reverse* the downward speed of the ball if it has reached. Reversing the downward speed of the ball involves inverting its `y` position (i.e. multiplying it by `-1`).

```java
if (y > 500) {
	speed = speed * -1;
}
```  

<div class="challenge row">
	<div class="col-sm-6 image">
		<img src="java/physics/real-bouncing.gif">
	</div>
	<div class="col-sm-6 text">
		<strong>Realistic bouncing</strong><br>
		The ball loses some of its upward motion from each bounce.<br>
		<small style="font-size:16px"><em>You don't need to draw the red dots tracing the path of the ball.</em></small>
		<button class="solution"><i class="fa fa-lightbulb-o"></i> show hint</button>  
	</div>
	<div class="col-sm-12 solution text">
<p>After reversing the speed of the ball, <em>scale it down</em> by multiplying it by a number between 0 and 1. We haven't gone into decimal-place numbers yet, so you will have to multiply it by a fraction (e.g. speed = speed * 4 / 5).</p>
	</div>
</div>

## Colliding

Suppose you want to be able to tap the ball in a new direction with your mouse pointer. You would first have to check if the mouse position is *close* to the position of the ball.

```java
if (Window.mouse.getX() - x < 10) {
	// ...
}
```

However, when the mouse pointer is to the left of the ball, the difference is a negative number. In this case, we are more interested in the *magnitude* of the difference rather than the direction, so what we really want to check is the *absolute value* of the difference between the mouse position and ball position. The method for returning the absolute value of its input is `Math.abs`.

```java
if (Math.abs(Window.mouse.getX() - x) < 10) {
	// The mouse is close in the x direction
}
```

Only if the mouse is close to the ball in the x direction, should we check if the mouse is also close in the y direction.
```java
// If the x positions are close, then ...
if (Math.abs(Window.mouse.getX() - x) < 10) {
	// ... check the y positions
	if (Math.abs(Window.mouse.getY() - y) < 10) {
		// The mouse is touching the ball
	}
}
```

<div class="challenge row">
	<div class="col-sm-6 image">
		<img src="java/physics/mouse-paddle.gif">
	</div>
	<div class="col-sm-6 text">
		<strong>Mouse paddle</strong><br>
		A paddle that follows the mouse can hit the ball in a new direction.  
		<button class="solution"><i class="fa fa-lightbulb-o"></i> show hint</button>  
	</div>
	<div class="col-sm-12 solution text">
<p>Since the ball's horizontal speed needs to change, you first need to add another variable xspeed that changes the x position of the ball. When the mouse position is close to the ball position, calculate the difference in positions and change the xspeed and yspeed variables by a fraction of that difference.</p>
	</div>
</div>

## Displaying values

You can output text to the screen with the `Window.out.print` method. This is a great way to see the value of a variable and how it changes over time. The first input to `Window.out.print` is the actual value to print out, the second input input is the `x` position of the text, and the third input is the `y` position.

If you add these instructions to your `while` loop, you can see the position of the ball.
```java
Window.out.color("black");
Window.out.print(x, 400, 50);
Window.out.print(y, 450, 50);
```

You can also change the font size with `Window.out.fontSize`, which takes a numeric font size as an input.  
```java
Window.out.color("black");
Window.out.fontSize(30);
Window.out.print(x, 400, 50);
Window.out.print(y, 450, 50);
```

To change the font itself, use `Window.out.font`.
```java
Window.out.font("Arial", 30);
```

#### Paddle challenge

Keep the ball above the ground using the mouse as a paddle. Count how many times you hit the ball, and reset the counter when it hits the ground.

![](java/physics/paddle-game.gif)

You will need to make another variable `count` that starts at 0, and goes up by 1 every time it touches the mouse paddle. When the ball hits the ground, the `count` variable needs to be set back to zero.

#### Multiple balls bouncing

Create two balls that can bounce off each other.

![](java/physics/multi-ball-bouncing.gif)

You will need to make four more variables to store the position and speed of the second ball. Then, just like the mouse paddle exercise, you will need to check if the positions of the two balls get close to each other, and adjust the speed of *both balls* when that happens.

#### Putting green challenge

Create a golf ball and a mouse-controlled putter that can hit the golf ball into a hole.

![](java/physics/golf.gif)

Make a new class, and set up variables to describe the position of the golf ball and its speed. Notice how the golf ball slows down when it is hit - in every turn, it's x speed and y speed are scaling down, just like the first bouncing ball challenge. When the distance between the golf ball and the hole is small, the ball should disappear (i.e. the ball should only be drawn if its distance to the hole is large).
