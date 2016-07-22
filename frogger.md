# Frogger

Frogger is a classic arcade game where a player directs a frog across a road while avoiding oncoming traffic.

[ frogger demo ]

## Prerequisites


## Getting started

- Draw the background of frogger with the road and grass on each side
- Create a frog object that can move around the screen (`draw` and `move` method)

Challenges:
- debounce the frog keys so it can only move one lane at a time (can't hold down key to move multiple lanes)
- when the frog reaches the end, give a point and reset it to the beginning

## Adding traffic

Create a vehicle class that randomly picks a y position and an x position off the screen to the left. In draw, it makes a rectangle, and in move it increases its x position

Challenges:
- randomize the color of the vehicles
- randomize the configuration of vehicles (truck, car, motorcycle)

## Collisions

Every vehicle object has a `touching` method which takes in a `Frog` object, and returns `true` if the frog is touching the vehicle. In the loop, continuously check every vehicle to find one that is colliding

## Adjusting difficulty

Difficulty correlates to the number of cars on the road, so as the score goes up, add more cars to the arraylist of cars

## Multiple car types

Creating multiple classes for `Car`, `Truck`, and `Motorcycle` that extend a base `Vehicle` class. 
