# Gravitation

In this lab, you will simulate the motion of planets in space.

![](java/gravity/example.gif)

This lab will be your first exercise in implementing a simulation without a formal code specification. The [lab exercises](#exercises) focus on implementing additional physics concepts and setting up situations where the simulation can yield interesting results.

## Vector

First, you will have to implement a class to describe a *vector*, which stores an (x, y) coordinate. The objects in the simulation will record their position and the magnitude/direction of their motion with `Vector` objects.

*vector visual*

In the **class outline** below, we included a **constructor**, a method for **vector addition**, a method for calculating the **inverse vector**, and **getters** for returning the x and y **vector components**.

```java
public class Vector {
	// The only data the vector object needs to store.
	private double x, y;

	// Constructor
	public Vector(double x, double y) { ... }

	// Add another vector to this vector, setting this x, y to the resulting vector.
	public void add(Vector other) { ... }

	// Return a new vector that is the inverse of this one.
	public Vector inverse() { }

	// Return the x component of this vector.
	public double getX() { ... }

	// Return the y component of this vector.
	public double getY() { ... }
}
```

It should be straightforward to implement this class - we have included the [solution](java/gravity/Vector.java) if you get stuck.

## Mass

You will also need to implement a class to describe a **mass**. Recall from the [physics lab](java/physics/#motion) that an object in motion has a **position** and a **velocity** (speed and direction). One concept that the physics lab did not go into much depth about was **acceleration** - a change to the object's velocity. Every object has a position describing *where it is* and a velocity describing *where it is moving*. Unlike position and velocity, an object does not have acceleration, but rather it *is accelerated*.

For example, consider a car driving along a road. Before the car starts moving, it's position is the beginning of the road, it's velocity is 0 miles per hour, and it is not being accelerated (i.e. its speed is not being changed). When the driver steps on the gas pedal, the car accelerates, adding to its velocity *in the forward direction*. After the car reaches full speed, it has a constant velocity and no acceleration.

```java
public class Mass {
	private Vector position;
	private Vector velocity;

	private double mass;
	private double radius;
	private String color;

	public Mass(double x, double y, double mass, double radius, String color) { ... }

	// Draw the mass at the given x, y position
	public void draw() { ... }

	// Use the vector class to
	public void move() {
		position.add(velocity);
	}

	// Change the velocity by the acceleration vector.
	public void accelerate(Vector acceleration) {
		velocity.add(acceleration);
	}

	public boolean isTouching(Mass other) { ... }


	public double getX() { ... }
	public double getY() { ... }

	public double getMass() { ... }
}
```

You are encouraged to write the implementation for the `Mass` class yourself, but you can also [download the solution](java/gravity/Mass.java) to go straight to implementing gravity simulations.
