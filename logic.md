# Logic

The `boolean` data type is used for variables that store a `true` or `false` value. You can create variables to store `boolean` values just like you made `int` variables.

```java
boolean good = true;
boolean bad = false;
```

In this section, we'll cover the operators that are used to combine boolean values to produce useful results, similar to how we went over the operators that combine numbers. These operators are especially useful when writing the condition to a branching statement like `if` or `while`, since there are many cases where you will want something to happen based on the `true`/`false` value of multiple conditions.  

## Producing a boolean

A boolean is produced from operations that provide `true` or `false` answers, like whether a number is less than another number.

```java
int age = Window.askFor("your age");

boolean canVote = age > 17;
System.out.println(canVote);
```

It is more useful to use a boolean directly in an `if` statement, as shown.

```java
int age = Window.askFor("your age");

if (age > 17) {
	System.out.println("You can vote!");
}
else {
	System.out.println("You can't vote yet.");
}
```

## And

You can combine two boolean values with the and operator (`&&`), which produces a `true` value only if the boolean values on each side of the `&&` are `true`.

```java
boolean happy = true;
boolean energetic = true;
boolean sad = false;
boolean worried = true;

boolean goodDay = happy && energetic;
boolean okayDay = happy && worried;
boolean badDay = sad && worried;

if (happy && energetic) {
	System.out.println("Today is a good day!");
}
```

## Or

You can combine two boolean values with the or operator (`||`), which produces a `true` value if either of the boolean values on each side of the `||` are `true`.

```java
if (sad || worried) {
	System.out.println("Take a deep breath.");
}
```

## Not

The not operator `!` *inverts* the value of the boolean immediately after it (i.e. converts a `true` to `false`, or vice versa).

```java
if (! happy) {
	System.out.println("Hope you feel better!");
}
```
