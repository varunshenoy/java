The term *object-oriented programming* was coined by **Dr. Alan Kay** in the late 1960s. Kay's background in biology inspired him to organize his programs as virtual cells (*objects*) that exchange data with each other. His idea to organize computer instructions by *"messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things"* evolved into the *object-oriented* organization patterns we see today.

In the previous chapter, you learned to write programs *procedurally* as a sequence of instructions. Some of those instructions controlled how other instructions would run, like `while (true) { ... }` making the code block `{ ... }` repeat forever. You also used instructions like `public static void methodName(int x)` to *define* a code block as the corresponding instructions to the method call `methodName(4)`. In this chapter, you will learn new instructions that define a code block `{ ... }` as a virtual description of something (a *class*).

### Memory model

<div class="row">
	<div class="col-sm-7 col-xs-8">
		<p>
			When Charles Babbage designed his [Analytical Engine]({{url}}/chapter/1/#charles-babbage),
		</p>
	</div>
	<div class="col-sm-5 col-xs-4">
		<img src="{{url}}/img/chapter/3/memory-0.png">
	</div>
</div>

### Why objects?

Object-oriented programming helps eliminate code repetition and provides an extensible organization pattern for larger projects. There is no correct way to do object-oriented programming, just coding patterns that you can choose to adhere to. Using the correct patterns will make your code more readable and make it easier for others to contribute new features to it.

But sometimes data is just data and methods are just methods. In many of the simple examples from the previous chapter, organizing them with the object-oriented patterns would be unnecessary. This chapter is about writing more complex programs that will need to be extended and modified over time.
