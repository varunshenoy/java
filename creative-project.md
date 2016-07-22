# Build your own game

In this lab, you will create a unique game. We will only outline the general process we want you to follow, and would like you to take this opportunity to express your own creativity through programming.

## Come up with an idea

Pick an idea that is feasible. We simply cannot help you if you decide to make a 3D shooting game or the next Minecraft, as we will end up doing most of the coding for you. The intention of this lab is for *you* to do the coding, and for us to help you by keeping you pointed in the right direction.

One common question we get is "can I make a game where there are a lot of something", like a game where zombies keep appearing from the sides of the screen. We will show you how to build such a game when we cover object-oriented programming in the next chapter. For now, pick a game idea that doesn't require you to have many moving parts.

A good way to judge the feasibility of a game is to think about how many variables you would need to implement the game. A character that moves around the screen requires at least two variables to store it's `x` and `y` position. An object that bounces around the screen requires more variables to describe its speed and acceleration.

## Common game mechanics

Below, we've listed some common elements in many games, along with some example code to create them.

### Player controlled by arrow keys

To have a key-controlled player, save their position in an `x` and `y` variable, and change the position when any arrow key is pressed.

```java
int x = 250;
int y = 250;

while (true) {
	Window.out.background("white");

	Window.out.color("red");
	Window.out.circle(x, y, 20);

	if (Window.key.pressed("up")) {
		y = y - 5;
	}
	if (Window.key.pressed("down")) {
		y = y + 5;
	}
	if (Window.key.pressed("left")) {
		x = x - 5;
	}
	if (Window.key.pressed("right")) {
		x = x + 5;
	}

	Window.frame();
}
```

### Collisions

To check if an object is colliding with another object, you can use the distance formula. Given two points (`x1`, `y1`) and (`x2`, `y2`), the distance between is calculated by summing the squares of the x and y difference, and square rooting the total.

![](http://intmstat.com/plane-analytic-geometry/Image1401.gif)

The distance formula is derived from the Pythagorean theorem - to find the distance, simply solve for the hypotenuse of the right triangle. To implement this in Java, you can use the `Math.sqrt` method for finding the square root.

```java
int dx = x2 - x1;
int dy = y2 - y1;

if (Math.sqrt(dx * dx + dy * dy) < 10) {
	// The objects are colliding
}
```

You can also implement a simpler, faster means of collision detection by simply checking if the absolute value of the x and y difference is small.
```java
int dx = x2 - x1;
int dy = y2 - y1;

if (Math.abs(dx) < 7 && Math.abs(dy) < 7) {
	// The objects are colliding
}
```

### Following

You can make one object follow another by changing the follower's `x` and `y` position by a fraction of the difference between it's own position and the target position it is trying to reach.

```java
// Follower
int a = 50;
int b = 100;

// Following this object.
int x = 400;
int y = 450;

while (true) {
	Window.out.background("white");

	// Draw the follower and the object it is following.
	Window.out.color("blue");
	Window.out.circle(a, b, 10);

	Window.out.color("red");
	Window.out.circle(x, y, 30);

	int dx = x - a;
	int dy = y - b;

	a = a + dx / 10;
	b = b + dy / 10;
}
```

We've combined this example with the arrow key motion example to create a simple game where a blue circle is constantly in pursuit of the red circle.

![](java/creative-project/following.gif)
