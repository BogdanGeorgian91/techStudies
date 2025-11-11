
Swift also heavily leans on the use of value types, which are types that are
copied instead of referenced—so you know your value won’t be modified
anywhere else when you’re in the midst of using it for something.

Swift’s objects can never be nil, which helps you prevent runtime crashes in
your programs. You can still use nil though, as and when you need it, via
optionals.

nil is the absence of a value. You might have seen it in other languages, or seen null. A big source of crashes in programming, no matter the language, is trying to access a value that isn’t there: those values are often nil.

In Swift, whole numbers are more properly known as “integers” and
decimal numbers as “doubles” or “floats.” Integers, doubles, and floats
are known as types.

# Variables

var is a variable
let is a constant

A variable can have its value changed, but its type can never change.
You cannot change a constant once it’s set.

If a variable or constant is declared with a type annotation but no data,
the data can be assigned later. The data must match the type
annotation.

Constants and variables need a type annotation, or a value that is
assigned at declaration. Or both.
When you declare a variable or constant, you can either supply a type
annotation or immediately assign a value.
If you assign a value, that’s all you need to do; the type system will use type
inference, and you’re done.
If you don’t want to assign a value at declaration, that’s fine, but you must
supply a type annotation so that Swift knows what type of data to expect
when some data is assigned.

You can only use the += operator to add to a string. You cannot use -=
to remove from a string.

Literals values are any value that appear directly in your code.
For example, if you define a variable named peopleComingToEatPizza and you assign the value 8 to it (because that’s how many people you have coming over to eat pizza), then the value
that appears directly in your source code, which is 8, is an integer literal:

var peopleComingToEatPizza = 8

If you define a variable to represent the percentage of your delicious pizza
pie that’s remaining and assign the value 3.14159 to it, then you’ve
assigned a floating-point literal to your variable:

var pieRemaining = 3.14159

Swift has a range of special types that can store collections of things.
Unsurprisingly, these are called collection types.


# Collection types

There are three main collection types you can use to store collections in
Swift: arrays, sets, and dictionaries. They all have differences, and
knowing when to use each is a fundamental Swift programming skill.

## Arrays

Arrays are ordered, and every element has a value of the same type and is automatically numbered with an index, starting from 0. This means you can access individual values using the indices. An array is an ordered collection of values of the same type.

We can add things to variable arrays in a variety of ways, including appending:
- var number:[Int] = [7, 15, 22, 343, 45]
- numbers.append(value)
- numbers.remove(at: 3)
- numbers.insert(11, at: 2)
- numbers[2] = 307

Every collection type in Swift has a count property available that lets you
get the number of elements in the collection, like this: 
```
numbers.count
```


## Sets

Sets are unordered, and every element has a value of the same type. There are no index values for sets, so you need to access them using features of the set, or by iterating through the whole set. Each value can only be stored in a set once.

Sets are similar to arrays and must also contain just one type, but sets are unordered, and there can only be one of each distinct value in a set.
You can create a set like this:

var evenNumbers = Set([2, 4, 6, 8])

Or like this:

var oddNumbers: Set = [1,3,5,7]

Sets are useful when you need to make sure each item only appears once.

## Dictionaries

Dictionaries are unordered collections of keys and values. Every key is the same type, and every value is the same type. A value can be accessed using its key, just like with indices in arrays. The keys can be different types than the values.

A dictionary collects things by mapping one thing to another...just like a real-world dictionary maps a word to a definition. Like sets, dictionaries in Swift are unordered. The data items that dictionaries map are referred to as keys and values.

We can also create an empty dictionary like this, but we have to specify the
types in advance, because (obviously) the compiler can’t figure that out
without any values present:

```
var scores: [String: Int] = [:]
```

Reading a value from a dictionary might look a little strange, compared
to what we’ve done so far.

To read a value from a dictionary, you can simply use the key:

```
print(scores["Paris"]!)
```

The exclamation mark afterward is used to directly access the value of the
key. This is called force unwrapping an optional.

once you’ve learned about force unwrapping, you should never actually do it. You can also add new values or change existing ones in the dictionary, like this:

```
scores.updateValue(17, forKey: "Bob")
```

Or like this:

```
scores["Josh"] = 4
```

## Tuples

Tuples let you store multiple values in a single variable or constant, but without the overhead of being, for example, an array.

A tuple can store as many values as you like, as long as their amount and type are defined up front. For example, you could use a tuple to represent coordinates, like x and y, and store them in one variable:

```
var point = (x: 10, y: 15)
```

Tuples can have names for each component or have no names at all. You
can update or modify tuples without using the names for the components, as
long as the types of the values match.

Type aliases don’t change anything about the type. They just give you a
new name to call the type. Type aliases are a great way to make your code more readable. But
be careful.

# Control flow statements

if statements, 
switch
for and for-in loops
while loops
repeat-while

# Range operators

Range operators are shortcuts for expressing a range of values.
These both represent the range 1, 2, 3, 4:

```
1...4
1..<5
```

The closed range operator defines a range that runs from the value on the
left side of the operator to the value on the right side.

The half-open range operator defines a range that runs from the value on
the left side of the operator to the value immediately before the value on the
right side.

There’s also a one-sided range operator that lets you consider a range that
continues as far as it can in one direction. For example, the following
constants define a range that starts at 5 and continues infinitely and a range
that goes from an infinitely negative number to 100, respectively:

```
let myRange = 5...
let myOtherRange= ...100
```

# Loops

Swift has two major loop statements. You can use for (and for-in) loops for running some code a certain number of times or iterating through every item in a collection type, and while (and repeat-while) loops for running some code over and over again until something becomes true or
false.

- for loops can run code repeatedly, usually based on some sort of numeric range.
- for-in loops can iterate over a sequence—most commonly items in a collection, but also ranges of numbers.
- while loops can run some code until a condition becomes true or false.
- You can put as many loops as you want inside other loops. Loops all the way down.

A while loop is particularly useful when you’re not sure how many iterations the loop will go through until it’s finished.

A repeat-while loop is just like a while loop, except that the condition is evaluated at the end of the loop, instead of at the beginning. It will always run at least once.

# Functions and Enums: Reusing Code on Demand

Functions in Swift allow you to package up a specific behavior or unit
of work into a single block of code that you can call from other parts of
your program. Functions can be standalone, or they can be defined as part
of a class, a structure, or an enumeration, where they are usually referred
to as methods. Functions let you break down complex tasks into smaller,
more manageable, and more testable units. They’re a core part of the way
you structure a program with Swift.

functions are called methods when they’re associated with a particular type: you can create a
function inside a class, structure, or enumeration, and that function (method) encapsulates some reusable code that relates to working with an instance of the type it’s inside.

A function is defined with the func keyword, followed by the desired
name of the function, any parameters the function has, and then the
function’s return type.

Function parameters are constants.
You cannot change the value of a function parameter from the body of a
function. For example, if you have a parameter named pizza, and
"Hawaiian" is passed in to the function via that parameter, the pizza
that exists within the function is a constant with the value
“"Hawaiian". It can’t be changed.

in Swift, it’s important to acknowledge that nothing you pass in to a
function will modify the original value unless you ask for it to be
modified

You can create a special kind of parameter that allows you to modify
the original value.
If you need to write a function that does modify the original value, you can
create an inout parameter.

If you wanted to write a function that takes a string variable as its sole
parameter, and turns that string variable into the string "Bob", you would
do it like this:

```
func makeBob(_ name: inout String) {
name = "Bob"
}
```

You could then use any string variable, like this one:

```
var name = "Tim"
```

And, by passing it in to the makeBob function, make it into a Bob:

```
makeBob(&name)
```

The "&" placed in front of the variable's name when you pass it into the function indicates that it can be modified by the function.
Now, the value inside name will be "Bob".
The use of an inout parameter means your parameter can never have a
constant or a literal value passed to it.
In function calls, inout arguments must be variables—they
can’t be expressions or constants.

Or you can make things even simpler with very simple functions, and take
advantage of Swift’s implicit return feature. If the entire body of a function
is a single expression, then it’s just returned for you:

```
func welcome(name: String) -> String {
	"Welcome to the Swift Pizza shop, \(name)!"
}
```

You can provide a default value for any parameter in a function. When you define your function, you can provide a default value for any of the parameters.
When a function has a default value, you can omit the parameter when you call the function if you like.

## A variable number of parameters

There are times when it is useful to write a function that can take a variable
number of parameters. For example, you could write a function that takes a
list of numbers, and calculates their average. That function would be far
more flexible if you could list as many or as few numbers as you wanted
every time you called it.

This feature is available in Swift—it’s called a variadic parameter. Here’s
how we could use it to implement a function to calculate the average of a
list of numbers:

```
func averate(_ numbers: Double...) -> Double {
	var total = 0.0
	for number in numbers {
		total += number
	}
	return total / Double(numbers.count)
}
```

A variadic can have zero elements.
A variadic can be “empty” at the call site, and you need to ensure your code can handle this correctly.

- Use an ellipsis to indicate that a parameter is a variadic.
- A function can only have one variadic parameter, but it can appear anywhere in the parameter list.
- Any parameters defined after a variadic parameter must have argument names.
- When you call a function with a variadic, all of its parameters have to be of the same type

## What can you pass to a function?

The short answer is: you can pass literally anything to a function. The
slightly longer answer is: you can pass anything you might need to a
function. Any Swift value can be passed as a parameter, including a
string, a Boolean, an integer, an array of strings...anything

But it’s not just values that are fair game as parameters for your functions.
Expressions work as well

## Every function has a type
When you define a function, you’re also defining a function type. A
function type is made up of the parameter types of the function as well as
the return type of the function.


# LAST PAGE LEFT

==pag 279


