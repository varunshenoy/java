# Conway's Game of Life

The Game of Life is a simulation devised by the mathematician **John Horton Conway**, where a two-dimensional grid of cells reproduce and die off according to a set of rules.

![]({{url}}/img/lab/game-of-life/example.gif)

The rules are listed below, and are applied to each cell in the grid simultaneously.

1. **The Underpopulation Rule**: A living cell with fewer than two live neighbors dies.
2. **The Survival Rule**: A living cell with two or three live neighbors survives to the next generation.
3. **The Overpopulation Rule**: A living cell with more than three neighbors dies.
4. **The Reproduction Rule**: A dead cell with exactly 3 neighbors comes to life in the next generation.

In this lab, you'll create a program to simulate the Game of Life, and add custom features to make it easier to explore the rich patterns and immense complexity that can be created from very simple rules.

## Prerequisites

 - **[if conditions]()** - you will need to write conditions to alter the state of a cell based on the state of the cells around it.
 - **[logic statements]()** - if conditions will require the `&&` and `||` operators.
 - **[writing methods]()** - there are three initial methods: `randomize`, `draw`, and `step`, as well as a method for each additional feature like mouse input and pausing.
 - **[multi-dimensional arrays]()** - the configuration of the board is stored in a two-dimensional array of `boolean` values.
 - **[iteration]()** - modifying the board involves iteration in two dimensions.

## Getting started

In the `lab` package, the outline of the animation is already set up in `GameOfLife.java`.

```java
import apcs.Window;

public class GameOfLife {
	// The grid array stores the current configuration of cells.
	static boolean[][] grid = new boolean[50][50];

	public static void main(String[] args) {
		Window.setFrameRate(10);
		randomize();

		while (true) {
			draw();
			move();
			Window.frame();
		}
	}

	public static void randomize() {
		// Randomize the state of cells in the grid.
	}

	public static void draw() {
		// Draw the grid.
	}

	public static void move() {
		// Compute the next configuration of the grid.
	}
}
```

The `grid` variable refers to a 50 x 50 grid of `boolean` values, where `true` represents a live cell and `false` represents a dead one.

## Randomizing the grid

The grid starts off with a random configuration of cells - let's start by saying there is an equal chance of a cell being alive or dead. `Window.flipCoin` returns a random boolean value, so all we need to do is *iterate every cell in the grid* and set it's value to the result of a coin flip.
```java
public static void randomize() {
	// For every x between 0 and 50, ...
	for(int x = 0 ; x < 50 ; x++) {
		// ... go to every y between 0 and 50, and ...
		for (int y = 0 ; y < 50 ; y++) {
			// ... set the cell at that (x, y) position to the result of a coin flip.
			grid[x][y] = Window.flipCoin();
		}
	}
}
```

## Drawing the grid

Drawing the grid involves drawing a white background, then drawing a black square for every live cell in the grid. Drawing every live cell involves iterating over every cell, and drawing a square at the cell's *corresponding location* on the screen if it's value is `true`.

The *corresponding location* is the (x, y) coordinate that the cell is drawn on the screen. For example, the cell at `grid[42][31]` would draw itself on the window as a square centered at (425, 315), because each cell is drawn with a width and height of 10.
```java
public static void draw() {
	// Clear the background.
	Window.out.background("white");
	// Draw everything past this point in black.
	Window.out.color("black");

	// For every x between 0 and 50, ...
	for(int x = 0 ; x < 50 ; x++) {
		// ... go to every y between 0 and 50, and ...
		for (int y = 0 ; y < 50 ; y++) {
			// ... if the cell at the (x, y) position is alive, ...
			if (grid[x][y] == true) {
				Window.out.square(x * 10 + 5, y * 10 + 5, 10);
			}
		}
	}
}
```

With `draw` and `randomize` implemented, you should be able to see the initial configuration of the grid.

![]({{url}}/img/lab/game-of-life/randomize.png)

## Moving the grid

Your task is to implement the `move` method, which needs to calculate the new configuration of the grid based on the current configuration. Since the rules are applied to every cell simultaneously, you can't apply the Game of Life rules to the cells in a particular order as we would have to do if we iterated over them. Instead,

## Updating the Grid

Create a new method, `step` the same way you created `draw`. In the `step` method, create a new 2-dimensional array of booleans that is the same size as the original grid. We will also need an integer variable, `count`.

```java
public static void main(String[] args) {
	grid = new boolean[50][50];
	while (true) {
		draw();
		step();
	}
}

private static void step() {
	boolean[][] new_grid = new boolean[50][50];
	int count;
}
```

To apply to rules of our game to the grid, we need to visit each cell and count how many neighbors it has. This will require two `for` loops and eight `if` statements, one for each neighbor. Rather than just checking if the cell is alive, however, we also need to check if the coordinates of the cell are valid. Trying to access a cell with a coordinate less than 0 or more than 49 will give us an error. Because of this, we need to perform the appropriate check before testing if the cell is alive. If the cell is alive, we add one to count.

If we are checking a cell with coordinates (x,y), the eight cells we need to check are

1. (x-1,y-1)
2. (x,y-1)
3. (x+1,y-1)
4. (x-1,y)
5. (x+1,y)
6. (x-1,y+1)
7. (x,y+1)
8. (x+1,y+1)


```java
private static void step() {
	boolean[][] new_grid = new boolean[50][50];
	int count;
	for (int x = 0; x < grid.length; x++) {
		for (int y = 0; y < grid[0].length; y++) {
			count = 0;
			if (x - 1 > 0 && y - 1 > 0 && grid[x - 1][y - 1]) {
				count = count + 1;
			}
			if (y - 1 > 0 && grid[x][y - 1]) {
				count = count + 1;
			}
			if (x + 1 < 49 && y - 1 > 0 && grid[x + 1][y - 1]) {
				count = count + 1;
			}
			if (x - 1 > 0 && grid[x - 1][y]) {
				count = count + 1;
			}
			if (x + 1 < 49 && grid[x + 1][y]) {
				count = count + 1;
			}
			if (x - 1 > 0 && y + 1 < 49 && grid[x - 1][y + 1]) {
				count = count + 1;
			}
			if (y + 1 < 49 && grid[x][y + 1]) {
				count = count + 1;
			}
			if (x + 1 < 49 && y + 1 < 49 && grid[x + 1][y + 1]) {
				count = count + 1;
			}
		}
	}
}
```

After the count has been determined, we use more `if` statements to apply the four rules. We check the values of count and whether the cell is alive or not, and based on this, update `new_grid`. Because each cell in `new_grid` has a defauly value of `false`, we only need to apply the rules that make a cell alive. These two `if` statements are still inside of our nested `for` loops. To check if a cell is dead, we use the NOT operator, `!`. Putting `!` in front of a boolean value flips the value from `true` to `false` and vice-versa.

```java
if ((count == 2 || count == 3) && grid[x][y]) {
	new_grid[x][y] = true;
}
if (count == 3 && !grid[x][y]) {
	new_grid[x][y] = true;
}
```

Finally, add `grid = new_grid;` to the end of the `step` method and the Game of Life is complete. To have the game run, create a third method called `start` that only runs once, before the while loop. In this method, visit every cell in grid and rndomly determine if it is alive or not. We generate random numbers with `Math.random()` which returns a random number from 0 to 1, not including 1. Your final code should look like this.
```java
public class Game_of_Life {
	static boolean[][] grid;

	public static void main(String[] args) {
		grid = new boolean[50][50];
		start();
		while (true) {
			draw();
			step();
		}

	}

	private static void start() {
		for (int x = 0; x < grid.length; x++) {
			for (int y = 0; y < grid[0].length; y++) {
				if (Math.random() < 0.5) {
					grid[x][y] = true;
				}
			}
		}
	}

	private static void step() {
		boolean[][] new_grid = new boolean[50][50];
		int count;
		for (int x = 0; x < grid.length; x++) {
			for (int y = 0; y < grid[0].length; y++) {
				count = 0;
				if (x - 1 > 0 && y - 1 > 0 && grid[x - 1][y - 1]) {
					count = count + 1;
				}
				if (y - 1 > 0 && grid[x][y - 1]) {
					count = count + 1;
				}
				if (x + 1 < 49 && y - 1 > 0 && grid[x + 1][y - 1]) {
					count = count + 1;
				}
				if (x - 1 > 0 && grid[x - 1][y]) {
					count = count + 1;
				}
				if (x + 1 < 49 && grid[x + 1][y]) {
					count = count + 1;
				}
				if (x - 1 > 0 && y + 1 < 49 && grid[x - 1][y + 1]) {
					count = count + 1;
				}
				if (y + 1 < 49 && grid[x][y + 1]) {
					count = count + 1;
				}
				if (x + 1 < 49 && y + 1 < 49 && grid[x + 1][y + 1]) {
					count = count + 1;
				}
				if ((count == 2 || count == 3) && grid[x][y]) {
					new_grid[x][y] = true;
				}
				if (count == 3 && !grid[x][y]) {
					new_grid[x][y] = true;
				}
			}
		}
		grid = new_grid;
	}

	private static void draw() {
		Window.out.background("white");
		Window.out.color("black");
		for (int x = 10; x <= 490; x = x + 10) {
			Window.out.rectangle(x, 250, 1, 500);
		}
		for (int y = 10; y <= 490; y = y + 10) {
			Window.out.rectangle(250, y, 500, 1);
		}
		for (int x = 0; x < grid.length; x++) {
			for (int y = 0; y < grid[0].length; y++) {
				if (grid[x][y]) {
					Window.out.rectangle(x * 10 + 5, y * 10 + 5, 10, 10);
				}
			}
		}
		Window.frame();
	}

}

```

## Bonus: Adding Mouse Input

Right now, our cells only come alive randomly at the start of the program. Instead, we can add an option to manually set cells to be alive or dead, as well as clearing the whole board.

We want to able to pause our game, so we will need a variable that tracks if we are paused or not. Create `static boolean running` and set it to true in the main method. In the `while` loop, add an `if` so that the `step` method only runs if `running` is true. Then create a new method in the while loop, `input`. Make sure the call to `input` is between `draw` and `step` so that only the board is updated before it is paused.

The input function will allow us to pause and unpause our game, manually click on cells to make them alive or dead, and clear the board. The function `Window.key.pressed` will tell us if a certain key on the keyboard is pressed. To pause the game when spacebar is pressed, pass the function the string `"space"`. If it is pressed, we want to flip the value of `running`. We also want to add a half-second delay so that the program doesn't register a single press as multiple presses. We add a delay with `Window.sleep`.

To get mouse input, we use the `Window.mouse` library. `Window.mouse.clicked` tells us if the mouse is clicked or not, and `Window.mouse.getX` and `Window.mouse.getY` give us the coordinates. Using these functions, write an `if` statement that checks if the game is paused and the mouse is clicked, and if so, use the coordinates to flip the value of a cell. Because each cell is 10 by 10, the index of the cell will be the coordinates of the click divided by 10. Again, we want a small delay when this happens.

Finally, when the game is paused and "c" key is pressed, we want to clear the entire board. If this key is pressed, set every value in the grid to `false`. Your final `input` method should look like this.

```java
private static void input() {
	if (Window.key.pressed("space")) {
		running = !running;
		Window.sleep(250);
	}
	if (!running && Window.mouse.clicked()) {
		grid[Window.mouse.getX() / 10][Window.mouse.getY() / 10] = !grid[Window.mouse.getX() / 10][Window.mouse.getY() / 10];
		Window.sleep(250);
	}
	if (!running && Window.key.pressed("c")) {
		for (int x = 0; x < grid.length; x++) {
			for (int y = 0; y < grid[0].length; y++) {
				grid[x][y] = false;
			}
		}
	}
}
```

# Exercises

Conway's Game of Life is one of the most simple examples of a **cellular automaton**, where a two dimensional grid changes in **discrete time intervals** according to a set of **automaton rules**.
