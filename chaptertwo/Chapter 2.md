# Meaningful Names : 
- Designing variables, objects and classes with intention revealing names is a powerful concept in nowaday's code.

## Introduction :
• This chapter will take you through best naming conventions practices owing that you
already know java by heart.

## Use Intention-revealing names :
• Try as much as you can to avoid vague names that don’t clarify your intentions, for
example method names that don’t clarify or show what do these methods actually do.
• Use the names that reveal the intention and the use of the proposed context, examples :
▪ Recall making a `Date` class that displays today’s date in a text view, so when
examining the class, there found to be a variable named `double t;` which
should stand for `time`, so instead of using a letter to describe a time, it would
be more obvious to use `time` instead.

## Time to practice :
- Q : In a context of a Mine-Sweeper game, you are recalled to build a function that gets the
flagged cells or tiles and add them to a collection that would be the return for this function,
<b>What would be your best approach ?<b/>

#### Bad Code : 

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

#### Good Code : 
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

#### Even better : 

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

#### Best Code :

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

## Avoid Disinformation :
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
## Make Meaningful distinctions :
- Don't try to satisfy the compiler with a `just it runs` code.
	For example if we want to display to different samsung devices in the same code.
	### Bad taste :
	```java
	protected final Device samsung = Device.getByBrand("Samsung");
	protected final Device samsung2 = Device.getByBrand("Samsung");
	```
	Here we name our variables just to satisfy our compiler (just to run the code), so we add the noisy number to our name in `samsung2`.

	### Good taste : 
	```java
	protected final Device galaxyE5 = Device.getByBrand("Samsung");
	protected final Device galaxyS5 = Device.getByBrand("Samsung");
	```
	As you can see the second example is far better the first one, because we actually name our objects to satisfy our code readers and for maintainablity in other code sections.
- Don't use noise keywords when naming objects.
	### Bad taste : 
	```java
	protected final List<Device> devicesList = new ArrayList<>();
	```
	Here the noise keyword is `List` in `devicesList`.
	### Good taste :
	```java
	protected final List<Device> devices = new ArrayList<>();
	```
	After removal of the noise word `List`, so noise words are redundant and should be abandoned.
- Prefixes can act sometimes as noise keywords and wouldn't add anything new to the name :
	### Bad taste :
	```java
	protected final Device theDevice = Devices.getDeviceByName("Galaxy A5");
	```
	Here `the` prefix is redundant and doesn't add to the meaning.
	### Good taste :
	```java
	protected final Device device = Devices.getDeviceByName("Galaxy A5");
	```
	Here we've removed `the` prefix.
	### Even better :
	```java
	protected final Device galaxyA5 = Devices.getDeviceByName("Galaxy A5");
	```
	Here we've introduced an even better approach by using an intention revealing name.
## Use pronounceable names :
### Example 1 :
#### Bad taste : 
```java
protected final Device a5 = Devices.getDeviceByName("Galaxy A5");
```
#### Good taste : 
```java
protected final Device galaxyA5 = Devices.getDeviceByName("Galaxy A5");
```
#### Even better taste :
```java
public class Samsung {
	protected final Device galaxyA5 = Devices.getDeviceByName("Galaxy A5");
	public final Device getGalaxyA5() {
		return galaxyA5;
	}
	....
}
```
By looking at these different examples, we conclude that the first `a5` represents an unpronounceable name for a device, a better name would be `galaxyA5` and a more better way of naming objects is by enclosing them inside a scope and then instantiating it by creating an object of the scope : 
```java
void testExample() {
	final Device galaxyA5 = new Samsung().getGalaxyA5();
}
```
### Example 2 :
#### Bad taste : 
```java
class DtaRcrd102 {
	private Date genymdhms;
	private Date modymdhms;
	private final String pszqint = "102";
};
```
#### Good taste : 
```java
class Customer {
	private Date generationTimestamp;
	private Date modificationTimestamp;
	private final String recordId = "102";
};
```
As a code reader, in the first example you could see that the object names are non-pronounceable and they represent acronyms  :
	- genymdhms ->  generation date (year, month, day, hour, minute, and second).
	- modymdhms ->  modification date (year, month, day, hour, minute and second).
Which would be far more better to be replaced by `generationDate` and `modificationDate` or `generationTimestamp` and `modificationTimestamp`.
## Use Searchable names : 
- Avoid using single-letter names and numeric constants, because they aren't locatable and also they aren't intention revealing names.
	#### Bad taste :
	```java
	public final static String SG5 = "Galaxy A5";
	```
	#### Good taste : 
	```java
	public static final String GALAXY_A5 = "Galaxy A5";
	```
	#### Even Better : 
	```java
	public final class Devices {
		public static final class Samsung {
			public static final String GALAXY_A5 = "Galaxy A5";
			...
		}
		public static final class Xiaomi {
			public static final String REDMI_NOTE_6_PRO = "Redmi Note 6 pro";
			...
 		}
	}
	```
	##### To call : 
	```java
	void testExample() {
		final Devices.Samsung galaxyA5 = Devices.Samsung.GALAXY_A5;
	}
	```
- Single-letter names could be used in naming local variables inside looping blocks or functions for example.
	#### Bad taste : 
	```java 
	int s = 0;
	for (int i = 0; i < 34; i++) {
		s += t[i];
	}
	```
	#### Good taste : 
	```java
	int sumOfTasks = 0;
	for (int i = 0; i < Calendar.WORK_DAYS; i++) {
		sumOfTasks += task[i];
	}
	```
	Here, the (i) in the loop statements is a single-letter name and it corresponds to the size of the for loop parameters, while the other names are better not be short names.

## Avoid Encodings : 
- Encoding names to types/classes or scopes adds an extra burden of deciphering information while reading the code.
- Types' names should be simple and intention revealing.
- Encoded names are rarely pronounceable and easy to mis-type.
#### Old style code (Bad taste): 
- Old languages like `Fortran` forced encodings by making the first letter a code for the type of the variable.
- Hungarian notation in C takes this to a whole new levels where return and type pointers are prefixed by their type's first letter and since the gcc or C compiler didn't check for the type of pointers in the past, this approach of Hungarian notation has helped developers to keep track of pointers' types.
- Example : 
```java
protected final Device galaxyA5Device = Devices.getDeviceByName("Galaxy A5");
```
#### New style code (modern languages) :
- In modern languages, compilers are more sensitive to types and can check them explicitly without the need to encode the type into the pointer/variable name.
- Example : 
```java
protected final Device galaxyA5 = Devices.getDeviceByName("Galaxy A5");
```
-- No need to encode `device` (the object type) into the name `galaxyA5`.
## Avoid member prefixes : 

