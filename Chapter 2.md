Meaningful Names
# Introduction :
• This chapter will take you through best naming conventions practices owing that you
already know java by heart.
# Use Intention-revealing names :
• Try as much as you can to avoid vague names that don’t clarify your intentions, for
example method names that don’t clarify or show what do these methods actually do.
• Use the names that reveal the intention and the use of the proposed context, examples :
▪ Recall making a `Date` class that displays today’s date in a text view, so when
examining the class, there found to be a variable named `double t;` which
should stand for `time`, so instead of using a letter to describe a time, it would
be more obvious to use `time` instead.

# Time to practice :
- Q : In a context of a Mine-Sweeper game, you are recalled to build a function that gets the
flagged cells or tiles and add them to a collection that would be the return for this function,
<b>What would be your best approach ?<b/>

### Bad Code : 

```java 
public List<Cell> get() {
    List<Cell> flaggedCells = new List<>();
    for (Cell cell : cellsList) {
          if (cell == 0) {
               flaggedCells.add(cell);
           }
     }
    return flaggedCells;
}
```
Well, this code doesn’t reveal what it
should really do by just looking on the
function name, you need to dig deeper
into the function’s body to know what
does it really do and also using bare-
bone constant `0` in comparison (cell
== 0) doesn’t indicate what the task
really want to do.

### Good Code : 
```java
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new List<>();
    for (Cell cell : cells) {
          if (cell == 0) {
               flaggedCells.add(cell);
           }
     }
    return flaggedCells;
} 
```
In this code, we have changed the
function name to give it a more clear
meaning of what it really does, which
is obviously collecting the flagged
cells of a Game Board. 

### Even better : 

```java
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new List<>();
    for (Cell cell : cells) {
          if (cell.isFlagged()) {
               flaggedCells.add(cell);
           }
     }
    return flaggedCells;
}
```
In this code, we have even introduced
a better approach to the `Cell` class by
encapsulating the boolean `flagged`
inside the accessor `isFlagged()` and
using it instead of the vague non-
informative constants. 

### Best Code :

```java
public List<Cell> getFlaggedCells() {
    final List<Cell> flaggedCells = new List<>();
    for (final Cell cell : cells) {
          if (cell.isFlagged()) {
               flaggedCells.add(cell);
           }
     }
    return flaggedCells;
}
```
Finally, we finishes our work by using
the `final` keyword with the non-
changing variables and objects, this
tells readers our intention about these
variables and objects that we are not
going to reassign them.

# Avoid Disinformation :
- Avoid words whose meanings vary from a context to another.
- Avoid using letters as variables names, this is too annoying to the reader and too vague to read, maintain and understand the code, for example : 
	### Bad taste :
	```java
	final Letter l = new Letter(a);
	// you really don’t know what you are trying to get here
	final String s = l.get();
	```
	### Good taste :
	```java
	final Letter letter = new Letter(address);
	// a self-explainable case
	final String sender = letter.getData().getSender();
	```
- Avoid using reserved keywords such as `final` `const` `namespace` as a variable or an instance name or even a class name, for example :
	### Bad taste : 
	```java
	final Station finalStation = Stations.getFinalStation();
	```
	### Good taste : 
	```java
	final Station lastStation = Stations.getLastStation();
	```
- Don’t code the container type (instance type) into the instance name, for example :
	### Bad taste : 
	```java
	final List<Accounts> accountsList = new List<>();
	```
	### Good taste :
	```java
	final List<Accounts> accounts = new List<>();
	```
- Never use variables names that vary in some letters or even words, for example :
	### Bad taste :
	```java
	XYZControllerForEfficientHandlingOfStrings xyzHandler;
	```
	```java
	XYZControllerForEfficientStorageOfStrings xyzString;
	```
	#### As you can see even you cannot compare between them, you need to have a deeper look to confirm the differences and some more time to know what they do !!

	### Better Approach (in terms of plain language) :
	```java
	XYZStringHandler handler;
	```
	```java
	XYZStringStorage storage;
	```
	### Best Approach (in terms of physics/vector spaces) :
	```java
	Vector3StringHandler handler;
	```
	```java
	Vector3StringStorage storage;
	```


