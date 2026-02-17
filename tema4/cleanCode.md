# Clean Code Notes

From [https://github.com/JuanCrg90/Clean-Code-Notes](https://github.com/JuanCrg90/Clean-Code-Notes)

Refactoring methods in IntelliJ: [https://blog.jetbrains.com/idea/2020/12/3-ways-to-refactor-your-code-in-intellij-idea/](https://blog.jetbrains.com/idea/2020/12/3-ways-to-refactor-your-code-in-intellij-idea/)

## Table of contents

- [Chapter 1 - Clean Code](#chapter1)
- [Chapter 2 - Meaningful Names](#chapter2)
- [Chapter 3 - Functions](#chapter3)
- [Chapter 4 - Comments](#chapter4)
- [Chapter 6 - Objects and Data Structures](#chapter6)
- [Chapter 10 - Classes](#chapter10)
- [Chapter 17 - Smells and Heuristics](#chapter17)

<a name="chapter1">
<h1>Chapter 1 -  Clean Code</h1>
</a>
This Book is about good programming. It's about how to write good code, and how to transform bad code into good code.

The code represents the detail of the requirements and the details cannot be ignored or abstracted. We may create languages that are closer to the requirements. We can create tools that help us parse and assemble those requirements into formal structures. But we will never eliminate necessary precision.

### Why write bad code?

- Are you in a rush?
- Do you try to go "fast"?
- Do not you have time to do a good job?
- Are you tired of work in the same program/module?
- Does your Boss push you to finish soon?

The previous arguments could create a swamp of senseless code.

If you say "I will back to fix it later" you could fall in the [LeBlanc's law](https://en.wikipedia.org/wiki/Talk%3AList_of_eponymous_laws#Proposal_to_add_LeBlanc.27s_law) "Later equals never"

You are a professional and the code is your responsibility. Let's analyze the following anecdote:

> What if you were a doctor and had a patient who demanded that you stop all the silly hand-washing in preparation for surgery because it was taking too much time? Clearly the patient is the boss; and yet the doctor should absolutely refuse to comply. Why? Because the doctor knows more than the patient about the risks of disease and infection. It would be unprofessional (never mind criminal) for the doctor to comply with the patient.

So too it is unprofessional for programmers to bend to the will of managers who don’t understand the risks of making messes.

Maybe sometime you think in go fast to make the deadline. The only way to go fast is to keep the code as clean as possible at all times.

### What is Clean Code?

Each experienced programmer has his/her own definition of clean code, but something is clear, a clean code is a code that you can read easily. The clean code is code that has been taken care of.

In his book Uncle Bob says the next:

> Consider this book a description of the Object Mentor School of Clean Code. The techniques and teachings within are the way that we practice our art. We are willing to claim that if you follow these teachings, you will enjoy the benefits that we have enjoyed, and you will learn to write code that is clean and professional. But don’t make the mistake of thinking that we are somehow “right” in any absolute sense. There are other schools and other masters that have just as much claim to professionalism as we. It would behoove you to learn from them as well.

### The boy Scout Rule

It’s not enough to write the code well. The code has to be kept clean over time. We have all seen code rot and degrade as time passes. So we must take an active role in preventing this degradation.

It's a good practice apply the [Boy Scout Rule](http://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule)

> Always leave the campground cleaner than you found it.

<a name="chapter2">
<h1>Chapter 2 -  Meaningful Names</h1>
</a>

Names are everywhere in software. Files, directories, variables functions, etc. Because we do so much of it. We have better do it well.

### Use Intention-Revealing Names

It is easy to say that names reveal intent. Choosing good names takes time, but saves more than it takes. So take care with your names and change them when you find better ones.

The name of a variable, function or class, should answer all the big questions. It should tell you why it exists, what it does, and how is used. **If a name requires a comment, then the name does not reveals its intent**.

| Does not reveals intention       | Reveals intention       |
| -------------------------------- | ----------------------- |
| `int d; // elapsed time in days` | `int elapsedTimeInDays` |

Choosing names that reveal intent can make much easier to understand and change code. Example:

```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for (int[] x : theList)
    if (x[0] == 4)
      list1.add(x);
  return list1;
}
```

This code is simple, but create many questions:

1. What is the content of `theList`?
2. What is the significance of the item `x[0]` in the list?.
3. Why we compare `x[0]` vs `4`?
4. How would i use the returned list?

The answers to these questions are not present in the code sample, but they could have been. Say that we’re working in a mine sweeper game. We can refactor the previous code as follows:

```java
public List<int[]> getFlaggedCells() {
  List<int[]> flaggedCells = new ArrayList<int[]>();
  for (int[] cell : gameBoard)
    if (cell[STATUS_VALUE] == FLAGGED)
      flaggedCells.add(cell);
  return flaggedCells;
}
```

Now we know the next information:

1. `theList` represents the `gameBoard`
2. `x[0]` represents a cell in the board and `4` represents a flagged cell
3. The returned list represents the `flaggedCells`

Notice that the simplicity of the code has not changed. It still has exactly the same number of operators and constants, with exactly the same number of nesting levels. But the code has become much more explicit.

We can improve the code writing a simple class for cells instead of using an array of `ints`. It can include an **intention-revealing function** (called it `isFlagged`) to hide the magic numbers. It results in a new function of the function.

```java
public List<Cell> getFlaggedCells() {
  List<Cell> flaggedCells = new ArrayList<Cell>();
  for (Cell cell : gameBoard)
    if (cell.isFlagged())
      flaggedCells.add(cell);
  return flaggedCells;
}
```

### Avoid Disinformation

Programmers must avoid leaving false clues that obscure the meaning of code. We should avoid words whose entrenched meaning vary from our intended meaning.

Do not refer to a grouping of accounts as an `accountList` unless it's actually a `List`. The word `List` means something specific to programmers. If the container holding the accounts is not actually a List, it may lead to false conclusions. So `accountGroup` or `bunchOfAccounts` or just plain `accounts` would be better.

Beware of using names which vary in small ways. How long does it take to spot the subtle difference between a `XYZControllerForEfficientHandlingOfStrings` in one module and, somewhere a little more distant, `XYZControllerForEfficientStorageOfStrings`? The words have frightfully similar shapes

### Make Meaningful Distinctions

Programmers create problems for themselves when they write code solely to satisfy a compiler or interpreter. For example because you can't use the same name to refer two different things in the same scope, you might be tempted to change one name in an arbitrary way. Sometimes this is done by misspelling one, leading to the surprising situation where correcting spelling errors leads to an inability to compile. Example, you create the variable `klass`because the name `class` was used for something else.

In the next function, the arguments are noninformative, `a1` and `a2` doesn't provide clues to the author intention.

```java
public static void copyChars(char a1[], char a2[]) {
  for (int i = 0; i < a1.length; i++) {
    a2[i] = a1[i];
  }
}
```

We can improve the code selecting more explicit argument names:

```java
public static void copyChars(char source[], char destination[]) {
  for (int i = 0; i < source.length; i++) {
    destination[i] = source[i];
  }
}
```

Noise words are another meaningless distinction. Imagine that you have a Product class. If you have another called `ProductInfo` or `ProductData`, you have made the names different without making them mean anything different. Info and Data are indistinct noise words like a, an, and the.

Noise words are redundant. The word variable should never appear in a variable name. The word table should never appear in a table name.

### Use Pronounceable Names

Imagine you have the variable `genymdhms` (Generation date, year, month, day, hour, minute and second) and imagine a conversation where you need talk about this variable calling it "gen why emm dee aich emm ess". You can consider convert a class like this:

```java
class DtaRcrd102 {
  private Date genymdhms;
  private Date modymdhms;
  private final String pszqint = "102";
  /* ... */
};
```

To

```java
class Customer {
  private Date generationTimestamp;
  private Date modificationTimestamp;;
  private final String recordId = "102";
  /* ... */
};
```

### Use Searchable Names

Single-letter names and numeric constants have a particular problem in that they are not easy to locate across a body of text.

### Class Names

Classes and objects should have noun or noun phrase names like `Customer`, `WikiPage`, `Account`, and `AddressParser`. Avoid words like `Manager`,`Processor`, `Data`, or `Info` in the name of a class. A class name should not be a verb.

### Method Names

Methods should have verb or verb phrase names like `postPayment`, `deletePage` or `save`. Accessors, mutators, and predicates should be named for their value and prefixed with get, set, and is according to the javabean standard.

When constructors are overloaded, use static factory methods with names that describe the arguments. For example:

```java
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

Is generally better than

```java
Complex fulcrumPoint = new Complex(23.0);
```

Consider enforcing their use by making the corresponding constructors private.

### Don't Be Cute

| Cute name         | Clean name    |
| ----------------- | ------------- |
| `holyHandGranade` | `deleteItems` |
| `whack`           | `kill`        |
| `eatMyShorts`     | `abort`       |

### Pick one word per concept

Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have fetch, retrieve, and get as equivalent methods of different classes.

### Don’t Pun

Avoid using the same word for two purposes. Using the same term for two different ideas is essentially a pun.

Example: in a class use `add` for create a new value by adding or concatenating two existing values and in another class use `add` for put a simple parameter in a collection, it's a better options use a name like `insert` or `append` instead.

### Use Solution Domain Names

Remember that the people who read your code will be programmers. So go ahead and use computer science (CS) terms, algorithm names, pattern names, math terms, and so forth.

### Use Problem Domain Names

When there is no “programmer-eese” for what you’re doing, use the name from the problem domain. At least the programmer who maintains your code can ask a domain expert what it means.

### Add Meaningful context

There are a few names which are meaningful in and of themselves—most are not. Instead, you need to place names in context for your reader by enclosing them in well-named classes, functions, or namespaces. When all else fails, then prefixing the name may be necessary as a last resort

Variables like: `firstName`, `lastName`, `street`, `city`, `state`. Taken together it's pretty clear that they form an address, but, what if you saw the variable state being used alone in a method?, you could add context using prefixes like: `addrState` at least readers will understand that the variable is part of a large structure. Of course, a better solution is to create a class named `Address` then even the compiler knows that the variables belong to a bigger concept

### Don’t Add Gratuitous Context

In an imaginary application called “Gas Station Deluxe,” it is a bad idea to prefix every class with GSD. Example: `GSDAccountAddress`

Shorter names are generally better than longer ones, so long as they are clear. Add no more context to a name than is necessary.

<a name="chapter3">
<h1>Chapter 3 -  Functions</h1>
</a>

Functions are the first line of organization in any topic.

### Small!!

The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.

#### Blocks and Indenting

This implies that the blocks within `if` statements, `else` statements, `while` statements, and so on should be one line long. Probably that line should be a function call. Not only does this keep the enclosing function small, but also adds documentary value because the function called within the block can have a nicely descriptive name.

This also implies that functions should not be large enough to hold nested structures. Therefore, the indent level of a function should not be greater than one or two. This, of course, makes the functions easy to read and understand.

### Do One Thing

**FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY.**

#### Sections within Functions

If you have a function divided in sections like _declarations_, _initialization_ etc, it's a obvious symptom of the function is doing more than one thing. Functions that do one thing cannot be reasonably divided into sections.

### Use Descriptive Names

> You know you are working on clean code when each routine turns out to be pretty much what you expected

Half the battle to achieving that principle is choosing good names for small functions that do one thing. The smaller and more focused a function is, the easier it is to choose a descriptive name.

Don’t be afraid to make a name long. A long descriptive name is better than a short enigmatic name. A long descriptive name is better than a long descriptive comment. Use a naming convention that allows multiple words to be easily read in the function names, and then make use of those multiple words to give the function a name that says what it does.

Choosing descriptive names will clarify the design of the module in your mind and help you to improve it. It is not at all uncommon that hunting for a good name results in a favorable restructuring of the code.

### Function arguments

The ideal number of arguments for a function is zero (niladic). Next comes one (monadic), followed closely by two (dyadic). Three arguments (triadic) should be avoided where possible. More than three (polyadic) requires very special justification—and then shouldn’t be used anyway.

Arguments are even harder from a testing point of view. Imagine the difficulty of writing all the test cases to ensure that all the various combinations of arguments work properly. If there are no arguments, this is trivial. If there’s one argument, it’s not too hard. With two arguments the problem gets a bit more challenging. With more than two arguments, testing every combination of appropriate values can be daunting.

#### Common Monadic Forms

There are two very common reasons to pass a single argument into a function. You may be asking a question about that argument, as in `boolean fileExists(“MyFile”)` . Or you may be operating on that argument, transforming it into something else and returning it. For example, `InputStream fileOpen(“MyFile”)` transforms a file name `String` into an `InputStream` return value. These two uses are what readers expect when they see a function. You should choose names that make the distinction clear, and always use the two forms in a consistent context.

#### Flag Arguments

Flag arguments are ugly. Passing a boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. It does one thing if the flag is `true` and another if the flag is `false`!

#### Dyadic Functions

A function with two arguments is harder to understand than a monadic function. For example, `writeField(name)` is easier to understand than `writeField(output-Stream, name)`

There are times, of course, where two arguments are appropriate. For example, `Point p = new Point(0,0);` is perfectly reasonable. Cartesian points naturally take two arguments.

Even obvious dyadic functions like assertEquals(expected, actual) are problematic. How many times have you put the actual where the expected should be? The two arguments have no natural ordering. The expected, actual ordering is a convention that requires practice to learn.

Dyads aren’t evil, and you will certainly have to write them. However, you should be aware that they come at a cost and should take advantage of what mechanims may be available to you to convert them into monads. For example, you might make the writeField method a member of outputStream so that you can say outputStream. writeField(name) . Or you might make the outputStream a member variable of the current class so that you don’t have to pass it. Or you might extract a new class like FieldWriter that takes the outputStream in its constructor and has a write method.

#### Triads

Functions that take three arguments are significantly harder to understand than dyads. The issues of ordering, pausing, and ignoring are more than doubled. I suggest you think very carefully before creating a triad.

#### Argument Objects

Compare:

```java
Circle makeCircle(double x, double y, double radius);
```

vs

```java
Circle makeCircle(Point center, double radius);
```

#### Verbs and Keywords

Choosing good names for a function can go a long way toward explaining the intent of the function and the order and intent of the arguments. In the case of a monad, the function and argument should form a very nice verb/noun pair. For example, `write(name)` is very evocative. Whatever this “name” thing is, it is being “written.” An even better name might be `writeField(name)` , which tells us that the "name" thing is a "field".

This last is an example of the keyword form of a function name. Using this form we encode the names of the arguments into the function name. For example, `assertEquals` might be better written as `assertExpectedEqualsActual(expected, actual)`. This strongly mitigates the problem of having to remember the ordering of the arguments.

### Command Query Separation

Functions should either do something or answer something, but not both. Either your function should change the state of an object, or it should return some information about that object. Doing both often leads to confusion.

### Prefer Exceptions to Returning Error Codes

Returning error codes from command functions is a subtle violation of command query separation.

### Don't Repeat Yourself

Duplication may be the root of all evil in software. Many principles and practices have been created for the purpose of controlling or eliminating it.

<a name="chapter4">
<h1>Chapter 4 -  Comments</h1>
</a>

Nothing can be quite so helpful as a well-placed comment. Nothing can clutter up a module more than frivolous dogmatic comments. Nothing can be quite so damaging as an old comment that propagates lies and misinformation.

If our programming languages were expressive enough, or if we had the talent to subtly wield those languages to express our intent, we would not need comments very much—perhaps not at all.

### Comments Do Not Make Up for Bad Code

Clear and expressive code with few comments is far superior to cluttered and complex code with lots of comments. Rather than spend your time writing the comments that explain the mess you’ve made, spend it cleaning that mess.

### Explain Yourself in Code

```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```

vs

```java
if (employee.isEligibleForFullBenefits())
```

### Good Comments

Some comments are necessary or beneficial. However the only truly good comment is the comment you found a way not to write.

#### Legal Comments

Sometimes our corporate coding standards force us to write certain comments for legal reasons. For example, copyright and authorship statements are necessary and reasonable things to put into a comment at the start of each source file.

#### Informative Comments

It is sometimes useful to provide basic information with a comment. For example, consider this comment that explains the return value of an abstract method:

```java
// Returns an instance of the Responder being tested.
protected abstract Responder responderInstance();
```

A comment like this can sometimes be useful, but it is better to use the name of the function to convey the information where possible. For example, in this case the comment could be made redundant by renaming the function: `responderBeingTested`.

<a name="chapter6">
<h1>Chapter 6 -  Objects and Data Structures</h1>
</a>

### Data Abstraction

Hiding implementation is not just a matter of putting a layer of functions between the variables. Hiding implementation is about abstractions! A class does not simply push its variables out through getters and setters. Rather it exposes abstract interfaces that allow its users to manipulate the essence of the data, without having to know its implementation.

### Data/Object Anti-Symmetry

These two examples show the difference between objects and data structures. Objects hide their data behind abstractions and expose functions that operate on that data. Data structure expose their data and have no meaningful functions.

**Procedural Shape**

```java
public class Square {
  public Point topLeft;
  public double side;
}

public class Rectangle {
  public Point topLeft;
  public double height;
  public double width;
}

public class Circle {
  public Point center;
  public double radius;
}

public class Geometry {
  public final double PI = 3.141592653589793;

  public double area(Object shape) throws NoSuchShapeException {
    if (shape instanceof Square) {
      Square s = (Square)shape;
      return s.side * s.side;
    }
    else if (shape instanceof Rectangle) { Rectangle r = (Rectangle)shape; return r.height * r.width;
    }
    else if (shape instanceof Circle) {
      Circle c = (Circle)shape;
      return PI * c.radius * c.radius;
    }
    throw new NoSuchShapeException();
  }
}
```

**Polymorphic Shape**

```java
public class Square implements Shape {
  private Point topLeft;
  private double side;

  public double area() {
    return side*side;
  }
}

public class Rectangle implements Shape {
  private Point topLeft;
  private double height;
  private double width;

  public double area() {
    return height * width;
  }
}

public class Circle implements Shape {
  private Point center;
  private double radius;
  public final double PI = 3.141592653589793;

  public double area() {
    return PI * radius * radius;
  }
}
```

Again, we see the complimentary nature of these two definitions; they are virtual opposites! This exposes the fundamental dichotomy between objects and data structures:

> Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.

The complement is also true:

> Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.

Mature programmers know that the idea that everything is an object is a myth. Sometimes you really do want simple data structures with procedures operating on them.

### Data Transfer Objects

The quintessential form of a data structure is a class with public variables and no functions. This is sometimes called a data transfer object, or DTO. DTOs are very useful structures, especially when communicating with databases or parsing messages from sockets, and so on. They often become the first in a series of translation stages that convert raw data in a database into objects in the application code.

<a name="chapter10">
<h1>Chapter 10 -  Classes</h1>
</a>

## Class Organization

### Encapsulation

We like to keep our variables and utility functions small, but we're not fanatic about it. Sometimes we need to make a variable or utility function protected so that it can be accessed by a test.

## Classes Should be Small

- First Rule: Classes should be small
- Second Rule: **Classes should be smaller than the first rule**

### The Single Responsibility Principle

**Classes should have one responsibility - one reason to change**

SRP is one of the more important concept in OO design. It's also one of the simple concepts to understand and adhere to.

### Cohesion

Classes Should have a small number of instance variables. Each of the methods of a class should manipulate one or more of those variables. In general the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable is used by each method is maximally cohesive.

### Maintaining Cohesion Results in Many Small Classes

Just the act of breaking large functions into smaller functions causes a proliferation of classes.

## Organizing for change

For most systems, change is continual. Every change subjects us to the risk that the remainder of the system no longer works as intended. In a clean system we organize our classes so as to reduce the risk of change.

### Isolating from Change

Needs will change, therefore code will change. We learned in OO 101 that there are concrete classes, which contain implementation details (code), and abstract classes, which represent concepts only. A client class depending upon concrete details is at risk when those details change. We can introduce intefaces and abstract classes to help isolate the impact of those details.

<a name="chapter17">
<h1>Chapter 17 -  Smells and Heuristics</h1>
</a>

## Comments

#### C1: Inappropriate Information

Reserve comments for technical notes referring to code and design.

#### C2: Obsolete Comment

Update or delete obsolete comments.

#### C3: Redundant Comment

A redundant comment describes something able to sufficiently describe itself.

#### C4: Poorly Written Comment

Comments should be brief, concise, correctly spelled.

#### C5: Commented-Out Code

Ghost code. Delete it.

## Functions

#### F1: Too Many Arguments

Functions should have no arguments, then one, then two, then three. No more than three.

#### F2: Output Arguments

Arguments are inputs, not outputs. If somethings state must be changed, let it be the state of the called object.

#### F3: Flag Arguments

Eliminate boolean arguments.

#### F4: Dead Function

Discard uncalled methods. This is dead code.

## General

#### G1: Multiple Languages in One Source File

Minimize the number of languages in a source file. Ideally, only one.

#### G2: Obvious Behavior is Unimplemented

The result of a function or class should not be a surprise.

#### G3: Incorrect Behavior at the Boundaries

Write tests for every boundary condition.

#### G4: Overridden Safeties

Overriding safeties and exerting manual control leads to code melt down.

#### G5: Duplication

Practice abstraction on duplicate code. Replace repetitive functions with polymorphism.

#### G6: Code at Wrong Level of Abstraction

Make sure abstracted code is separated into different containers.

#### G7: Base Classes Depending on Their Derivatives

Practice modularity.

#### G8: Too Much Information

Do a lot with a little. Limit the amount of _things_ going on in a class or functions.

#### G9: Dead Code

Delete unexecuted code.

#### G10: Vertical Separation

Define variables and functions close to where they are called.

#### G11: Inconsistency

Choose a convention, and follow it. Remember no surprises.

#### G12: Clutter

Dead code.

#### G13: Artificial Coupling

Favor code that is clear, rather than convenient. Do not group code that favors mental mapping over clearness.

#### G14: Feature Envy

Methods of one class should not be interested with the methods of another class.

#### G15: Selector Arguments

Do not flaunt false arguments at the end of functions.

#### G16: Obscured Intent

Code should not be magic or obscure.

#### G17: Misplaced Responsibility

Use clear function name as waypoints for where to place your code.

#### G18: Inappropriate Static

Make your functions nonstatic.

#### G19: Use Explanatory Variables

Make explanatory variables, and lots of them.

#### G20: Function Names Should Say What They Do

#### G21: Understand the Algorithm

Understand how a function works. Passing tests is not enough. Refactoring a function can lead to a better understanding of it.

#### G22: Make Logical Dependencies Physical

Understand what your code is doing.

#### G23: Prefer Polymorphism to If/Else or Switch/Case

Avoid the brute force of switch/case.

#### G24: Follow Standard Conventions

It doesn't matter what your teams convention is. Just that you have on and everyone follows it.

#### G25: Replace Magic Numbers with Named Constants

Stop spelling out numbers.

#### G26: Be Precise

Don't be lazy. Think of possible results, then cover and test them.

#### G27: Structure Over Convention

Design decisions should have a structure rather than a dogma.

#### G28: Encapsulate Conditionals

Make your conditionals more precise.

#### G29: Avoid Negative Conditionals

Negative conditionals take more brain power to understand than a positive.

#### G31: Hidden Temporal Couplings

Use arguments that make temporal coupling explicit.

#### G32: Don’t Be Arbitrary

Your code's structure should communicate the reason for its structure.

#### G33: Encapsulate Boundary Conditions

Avoid leaking +1's and -1's into your code.

#### G34: Functions Should Descend Only One Level of Abstraction

The toughest heuristic to follow. One level of abstraction below the function's described operation can help clarify your code.

#### G35: Keep Configurable Data at High Levels

High level constants are easy to change.

#### G36: Avoid Transitive Navigation

Write shy code. Modules should only know about their neighbors, not their neighbor's neighbors.

## Names

#### N1: Choose Descriptive Names

Choose names that are descriptive and relevant.

#### N2: Choose Names at the Appropriate Level of Abstraction

Think of names that are still clear to the user when used in different programs.

#### N3: Use Standard Nomenclature Where Possible

Use names that express their task.

#### N4: Unambiguous Names

Favor clearness over curtness. A long, expressive name is better than a short, dull one.

#### N5: Use Long Names for Long Scopes

A name's length should relate to its scope.

#### N6: Avoid Encodings

Do not encode names with type or scope information.

#### N7: Names Should Describe Side-Effects

Consider the side-effects of your function, and include that in its name.

## Tests

#### T1: Insufficient Tests

Test everything that can possibly break

#### T2: Use a Coverage Tool!

Use your IDE as a coverage tool.

#### T3: Don’t Skip Trivial Tests

#### T4: An Ignored Test is a Question about an Ambiguity

If your test is ignored, the code is brought into question.

#### T5: Test Boundary Conditions

The middle is usually covered. Remember the boundaries.

#### T6: Exhaustively Test Near Bugs

Bugs are rarely alone. When you find one, look nearby for another.

#### T7: Patterns of Failure Are Revealing

Test cases ordered well will reveal patterns of failure.

#### T8: Test Coverage Patterns Can Be Revealing

Similarly, look at the code that is or is not passed in a failure.

#### T9: Tests Should Be Fast

Slow tests won't get run.
