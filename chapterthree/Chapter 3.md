# Functions 
In this chapter you are expected to learn how to write functions efficiently with an easy to read and maintaible format.

## Small functions
- Writing small functions is the key to writing good functions.
- A function should have no more than 10 - 15 lines of code.
- A function shouldn't ever exceed 20 lines of code.
- A function should do only one thing perse.
- A function shouldn't delegate work to other functions or classes, and if it would that should be visible from the function name and/or documentations.
- Practically speaking, programs with functions of just 4 - 5 lines of code are always the best.

## Avoid Nested block control of flow structures
- Avoid writing `nested statements` (conditional or loop statements) inside the functions block as it increases the size of the function.
- Writing `nested statements` inside a function block adds a burden documentary bear to the situation (you should never document the mess you write, rather clean it first).

## Do one thing
- Again, a function should do only one thing and it should state that thing in its self-descriptive name.
- If you ever thought of dividing a function into sections, it's far better to take your time to separate the tasks inside this fucntion into 
functions if they are dependant upon each other.
- If there are some tasks that need to be called together inside a function, they must be first distributed among several functions and then collect their call 
in an `initialize()` or `startTransaction()` functions if they are a beginning of operation, or put the call in a `Util` (which is a static final class) for quick call.

## One level of abstraction per function
- Avoid mixing between low level and high level computing instructions which indeed is confusing.
example : mixing between a `getter` of an abstract tree and a `String` concatenation procedure.
- If you have to say create a `url` before parsing data to another high level of abstraction function, you better include a function in the higher level
state that does so.

## The Stepdown Rule
- Inside your classes, as you read your code from top to bottom, you should write your functions in the order you will call them for instructions.
- A function should have a code that continues after the function before it and introduces the next function.
- Overriden functions should be in the beginning in the same order from the super class.
- Embedded interfaces and enums should be at the beginning and it would be better to be in their own respective files.

## 

