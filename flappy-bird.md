# Flappy Bird

[Flappy Bird](http://flappybird.io/) is an iconic game originally designed for smartphones, where a player directs a pixelized bird through oncoming obstacles.

![](java/flappy-bird/splash.gif)

Flappy Bird became a viral sensation in 2014. The creator of Flappy Bird, [Dong Nguyen](https://twitter.com/dongatory), created the original game for "people on the move", citing the need for something simpler than *Angry Birds* that could be played with only one hand. Since it's release, Flappy Bird has been rebuilt, extended, and parodied on almost every graphics library on every platform (even the [Commodore 64](http://sos.gd/flappy64/)). Nguyen ended up [pulling it out of the app store](http://www.macrumors.com/2014/03/11/flappy-bird-dong-nguyen-interview/) over regrets of how much time and attention his game had consumed.

## Getting Started

Create a Java class named `FlappyBird`. Since the game was originally made for phones, the first instruction uses the `Window.size` method to make the window phone-sized.
```java
public class FlappyBird {
	public static void main(String[] args) {
		Window.size(400, 600);
	}
}
```

## Creating the bird

Start by drawing the sky background and the bird in the center of the screen. For now, we will represent the bird as a yellow oval.

```java
Window.size(400, 600);

// Draw the background.
Window.out.background("light blue");

// Draw the bird.
Window.out.color("yellow");
Window.out.oval(200, 300, 50, 40);
```

Since you will be animating changes to the bird's position, the first thing you should do is set up a `while` loop that ends with `Window.frame()`.

```java
Window.size(400, 600);

while (true) {
	// Draw the background
	Window.out.background("light blue");

	// Draw the bird
	Window.out.color("yellow");
	Window.out.oval(200, 300, 50, 40);

	Window.frame();
}
```

The bird moves like the [bouncing ball from the previous lab](java/physics#bouncing-ball), with an `x` and `y` variable to describe its position. Don't forget to change the inputs to `Window.out.oval` to use `x` and `y` for the position of the oval.

```java
Window.size(400, 600);

int x = 200;
int y = 300;

while (true) {
	// Draw the background.
	Window.out.background("light blue");

	// Draw the bird - notice how this now uses x and y
	Window.out.color("yellow");
	Window.out.oval(x, y, 50, 40);

	Window.frame();
}
```

To make the bird accelerate to the ground as if by gravity, add a `speed` variable, change `y` by `speed` in each iteration of the loop, and increase `speed` by a fixed number in each iteration.

```java
int x = 200;
int y = 300;
int speed = 0;

while (true) {
	// draw the scene and the bird at (x, y)

	// Accelerate y position
	y = y + speed;
	speed = speed + 1;
}
```

## Flapping

When you press the space bar, the bird flaps its wings and starts moving upward. The easiest way to write this condition would be to simply set `speed` to some negative number when the space key is pressed.

```java
if (Window.key.pressed("space")) {
	speed = -10;
}
```

You could also have the upward speed depend on the downward speed, so the bird retains a percentage of its downward speed after a flap.
```java
if (Window.key.pressed("space")) {
	speed = -10 + speed / 3;
}
```

#### Debouncing challenge

Holding down the spacebar will make the bird keep moving up. Can you modify the code and the `if` condition to only allow one flap for each time the space key is pressed? This is called **debouncing** the key - making each press/release of the key only trigger the action once.

**Create 
![](java/flappy-bird/debounce-challenge.gif)

*Hint: Use a `boolean` to store whether the space key being pressed has already triggered the speed change action. When the space key is not pressed, set this boolean value back to `false`. In the code block for the space key `if` condition, set the value to `true`. Modify the condition to only run the speed change action if the space key is pressed and the boolean value is `false`.*

## Adding in the pipes

To create the illusion of the bird moving toward an opening between a pair of pipes, we can create a pair of pipes that moves horizontally across the screen in the other direction (toward the bird).

First create a variable to store the `x` position of the pipe, then use that `x` position to draw the rectangle for each pipe. In each frame, decrease the `x` position, so the frame draws farther left in the next iteration.

```java
int pipex = 500;

while (true) {
	// draw the background and bird
	// move the bird

	Window.out.color("green");
	Window.out.rectangle(pipex, 100, 50, 200);

	pipex = pipex - 3;
}
```

Notice that we only drew the top pipe in the previous example. To draw both pipes, we would need to call `Window.out.rectangle` twice with different inputs.

```java
Window.out.color("green");
Window.out.rectangle(pipex, 100, 50, 200);
Window.out.rectangle(pipex, 400, 50, 200);
```

The pipe will disappear when it's x position becomes negative. The pipe is still being drawn on the screen, just at a negative position that you can't see. You can fix this by resetting the pipe's x position to the right side of the screen whenever it goes too far to the left.

```java
if (pipex < -50) {
	pipex = 450;
}
```

You may have noticed that the safe gap to travel between the pipes is always in the same place, which doesn't make for a very challenging game. In the original game of flappy bird, the opening in the pipe is randomly determined. This is another example where *something needs to change*, and whenever that is the case, *make a variable*.

In the example below, I've made a variable called `gap`, which indicates the y-position of the center of the pipe opening.

```java
int pipex = 500;
int gap = 300;

while (true) {
	// draw the background and bird
	// move the bird

	Window.out.color("green");
	Window.out.rectangle(pipex, 100, 50, 200);
	Window.out.rectangle(pipex, 400, 50, 200);

	pipex = pipex - 3;
}
```

When the value of `gap` changes, the size and position of the two rectangles for the pipes needs to change as well. Specifically, the `y` position and the `height` of each pipe depends on the value of `gap`. The diagram below illustrates the relation between the value of `gap` and the size/positions of the rectangles in the pipe. The red values are the values that depend on `gap`, and the blue values are the ones you already know.

![](java/flappy-bird/pipe-size.png)

For example, the height of the the top pipe (`height1`) is 50 pixels less than the value of gap. Therefore, we can replace the `height` input for the first pipe to be `gap - 50`, so any value of `gap` will correctly size the height of the top rectangle.

```java
Window.out.color("green");
// Notice the height input is now gap - 50
Window.out.rectangle(pipex, 100, 50, gap - 50);
Window.out.rectangle(pipex, 400, 50, 200);
```
Let's do one more - the y position of the top pipe. The y position of the pipe is at the center of the rectangle, and we know the height of the rectangle to be `gap - 50`. In this case, we can divide the height by 2 to get the `y` position.

```java
Window.out.color("green");
// Notice the y input is now (gap - 50) / 2
Window.out.rectangle(pipex, (gap - 50) / 2, 50, gap - 50);		
Window.out.rectangle(pipex, 400, 50, 200);
```

There are 2 other values for you to find in sizing the pipe. Once you've done this, you should be able to assign any value to `gap`, and have the pipes automatically size
themselves to show a gap in that location.

To pick a random value for `gap`, you can use `Window.rollDice`, which takes a single input that determines how many sides are on the dice being rolled, and returns the number rolled by the dice. So `Window.rollDice(6)` will return a random number between 1 and 6, and `Window.rollDice(100)` will return a random number between 1 and 100.

Notice how in the example below, we roll a 400 sided dice, and add 50 to whatever the result is, to give a random number between 51 and 450.
```java
gap = 50 + Window.rollDice(400);
```

Just to wrap up everything so far, here is all the code we've created so far.

```java
Window.size(400, 600);

// Store the bird's position and speed
int x = 200;
int y = 300;
int speed = 0;

// Store the pipe's position and center gap
int pipex = 425;
int gap = 50 + Window.rollDice(400);

while (true) {
	// Draw the background
	Window.out.background("light blue");

	// Draw the bird
	Window.out.color("yellow");
	Window.out.oval(x, y, 50, 40);

	// Move the bird
	y = y + speed;
	speed = speed + 1;

	// Draw the pipe
	Window.out.color("green");
	Window.out.rectangle(pipex, (gap - 50) / 2, 50, gap - 50);

	// The bottom pipe still needs to change it's y and height based on gap!
	Window.out.rectangle(pipex, 400, 50, 200);

	// Move the pipe
	pipex = pipex - 3;

	// Reset the pipe if it passes the left side
	if (pipex < -25) {
		pipex = 425;
		gap = 50 + Window.rollDice(400);
	}

	Window.frame();
}
```

#### Pipe collision challenge

Detect collisions between the bird and the pipes, and reset the bird if it hits the pipe. Like the ball collisions from the [previous lab](java/physics#collisions), you will need to write an `if` condition to check whether the bird's `x` and `y` position is within the **bounding rectangle** of either pipe.

![](java/flappy-bird/collision-challenge.gif)

## Adding score

From the previous challenge, you should have added an `if` condition to your game loop that checks if the bird is colliding with the pipes. When the bird is colliding with the pipes, all the variables describing positions should be reset to their original values.

```
if ( bird colliding with top pipe or bird colliding with bottom pipe ) {
	// reset all variables to initial values
	x = 300;
	y = 200;
	pipex = 450;
}
```

One of the values you would also want to reset to zero when the bird hits the pipe is the player's score. First, make a variable `score` to store the player's score, and start it at zero. One strategy to make the player's score accurately reflect how many pipes they have crossed would be to simply increment it on every frame, and divide it by the number of frames between pipes.

```
int score = 0;
// other variables

while (true) {
	score = score + 1;
	System.out.print("Your score is ");
	System.out.println(score / 20);

	// frame drawing/moving ...
}
```

The number `20` was picked arbitrarily - you may have to make this larger or smaller depending on how close or far apart your pipes are. Every 20 frames, the displayed score (`score / 20`) increases by `1`. Recall that since `score` is an integer, dividing it can only produce an integer, so it takes at least 20 increments of score before `score / 20` is greater than `0`.   

Instead of printing the text to the console, you can print it to the window. `Window.out.print` is a method that takes the value you want to print as the first input, and the `x` and `y` position to print it on the window as the second and third input. The text will be drawn in whatever color has already been selected as the drawing color.

```
int score = 0;
// other variables

while (true) {
	score = score + 1;

	Window.out.color("black");
	Window.out.print(score / 20, 350, 40);

	// frame drawing/moving ...
}
```

## Using images

You can use `Window.out.image` to draw the image from the given file path on the window at a certain x, y coordinate. The first input to `Window.out.image` is the path of the image file, which in this case is `"flappy.png"`.


<div>
    <img src="{{url}}/image/java/flappy-bird/flappy.png" style="width:50px;display:block;margin:0 auto;">
</div>

You can download this image by saving the image above, or by [clicking this link]({{url}}/image/java/flappy-bird/flappy.png).

To add the image at the path `"flappy.png"` to your game centered at the position `(200, 300)`, you could write

```java
Window.out.image("flappy.png", 175, 280);
```

Notice that `25` was subtracted from the x position, and `20` from the y position, in order for the image to appear exactly centered on the screen. The second and third input is the x and y position of the *top left corner* of the image when it is displayed on the screen.

In order for the file to be read into the window correctly, it needs to be in the same folder as the `FlappyBird.java` file. If your files show up under "default package" (as shown below), then you need to drag your images into the `src` folder.

<div><img src="{{url}}/image/java/flappy-bird/file-path.png" style="width:400px;"></div>

If you are organizing your files by package, you can keep your images in the same package as your `.java` files, but you will need to specify the package as well in the call to `Window.out.image`. For example, if you are placing your image file in the `flappybird` package as shown below:

<div><img src="{{url}}/image/java/flappy-bird/file-path-2.png" style="width:400px;"></div>

Then in `FlappyBird.java`, you would output the image to the window with

```
Window.out.image("flappybird/flappy.png", 175, 280);
```

At this point, you should have a working framework of the game. The challenges below are to get you to think about user interface, and approach usability of your games with the eye of a designer.

#### Introduction screen challenge

Display a screen explaining the mechanics of the game before the actual game begins.

![](java/flappy-bird/start-screen.png)

Your infinite loop (`while (true)`) infinitely repeats the process of drawing a single frame. Before that loop begins, you want to
repeatedly draw the welcome screen until a certain key is pressed or interaction made. We can think of this as "while some event has not yet happened", or `while ( ! event )`.

```
// create variables ...

// First show welcome screen
while (! Window.key.pressed("space")) {
	Window.out.background("light blue");
	Window.out.color("white");
	Window.out.font("Arial", 40);
	Window.out.print("Welcome to Flappy Bird!", 150, 50);
	// draw the rest of the welcome screen...

	Window.frame();
}

while (true) {
	// regular game animation ...
}
```

#### Play again challenge

When the player collides with a pipe, ask them if they want to play again or not.

![](java/flappy-bird/game-over.gif)

#### Adjustable constants challenge

Make it easy to adjust the constant values in your code for gravity, speed changes, and initial speeds to find the optimal difficulty level.

![](java/flappy-bird/splash.gif)

#### Unique visual elements challenge

Adjust the size, appearance, and mechanics of your game to make it unique. You can improve the appearance, experiment with other ways of moving the bird, or even add more obstacles for the bird to avoid.  

![](java/flappy-bird/unique-design.gif)
