*This session has hosted at 42 Lisbon by me (<a href ="https://github.com/sylvzzz">sylvzzz</a>) and Denys (<a href ="https://github.com/denysbp">Denys</a>)*

# Unlocking Python — Session Guide

*An introduction to Python for people getting ready for the Discovery Piscine, working through the Python modules, or just starting to program.*

*ps: credit for 42 Lisbon for teaching me these concepts and letting me be able to teach them

## Table of Contents

1. [What is Python](#1-what-is-python)
2. [Python vs C](#2-python-vs-c)
3. [How Python Works](#3-how-python-works)
4. [Syntax Basics](#4-syntax-basics)
5. [Data Types](#5-data-types)
6. `print()`, f-strings and `input()`
7. [Core Types: int, str, list, dict](#7-core-types-int-str-list-dict)
8. [Methods for Types](#8-methods-for-types)
9. [Built-in Functions](#9-built-in-functions)
10. [The Standard Library](#10-the-standard-library)
11. [`for` Loops](#11-for-loops)
12. [`if` / `elif` / `else` and Operators](#12-if--elif--else-and-operators)
13. [Defining Functions (`def`)](#13-defining-functions-def)
14. [Mutable vs Immutable Types](#14-mutable-vs-immutable-types)
15. [Exceptions / `try`-`except`](#15-exceptions--try-except)
16. [Unpacking / Multiple Assignment](#16-unpacking--multiple-assignment)
17. [Object-Oriented Programming (OOP)](#17-object-oriented-programming-oop)
18. [General Good Practices](#18-general-good-practices)


## 1. What is Python

Python is a **high-level, interpreted, general-purpose programming language** created by Guido van Rossum, first released in 1991. It's known for:

- **Readable syntax** — close to plain English, uses indentation instead of curly braces.
- **Dynamic typing** — you don't declare variable types explicitly.
- **Interpreted** — code runs line by line through an interpreter rather than being compiled to a standalone binary ahead of time.
- **Multi-paradigm** — supports procedural, object-oriented, and functional styles.
- **Huge ecosystem** — massive standard library plus third-party packages (via `pip`) for almost anything: web dev, data science, AI, scripting, automation.

**Talking point for the session:** Python is often called a great "first language" because it lets beginners focus on *logic and problem-solving* instead of fighting with memory management or verbose syntax, which is exactly why it's used to introduce programming in the Piscine.

## 2. Python vs C

This is a great section to anchor *why* Python feels so different if people come from (or will move to) C.

| Aspect | C | Python |
|---|---|---|
| Execution | Compiled to machine code (binary) | Interpreted (compiled to bytecode, run by a virtual machine) |
| Typing | Static typing — types declared and checked at compile time | Dynamic typing — types checked at runtime |
| Memory management | **Manual** — `malloc()` / `free()` | **Automatic** — garbage collector |
| Speed | Very fast, close to hardware | Slower — extra abstraction layers |
| Syntax | Verbose, explicit (braces, semicolons, type declarations) | Concise, indentation-based |
| Error handling | Return codes / manual checks | Exceptions (`try`/`except`) |

---

### Garbage Collection vs `malloc`/`free`

In **C**, you are responsible for memory:

```c
int *arr = malloc(5 * sizeof(int)); // you allocate it
free(arr); // you MUST free it, or it's a memory leak
```

If you forget `free()`, you leak memory. If you `free()` it twice, or use it after freeing, you get undefined behavior (crashes, security bugs).

In **Python**, memory is managed automatically by the interpreter:

```python
arr = [1, 2, 3, 4, 5]  # allocated automatically
arr = None             # once nothing references the list anymore,
                       # the garbage collector reclaims the memory
```

Key mechanism to mention:
- **Reference counting**: every Python object keeps a count of how many references point to it. When it hits zero, memory is freed immediately.
- **Cyclic garbage collector**: handles cases reference counting can't (e.g., two objects referencing each other), running periodically to clean up cycles.

**Trade-off to highlight:** Python trades raw performance and manual control for developer speed and safety — fewer memory bugs, but less control over *when exactly* memory is freed and generally slower execution.


## 3. How Python Works

A simple mental model to draw on a whiteboard:

```
your_code.py  →  [Python Interpreter]  →  Bytecode (.pyc)  →  [Python Virtual Machine]  →  Result
```

1. You write Python source code (`.py` file).
2. The **interpreter** compiles it to an intermediate form called **bytecode**.
3. The **Python Virtual Machine (PVM)** executes that bytecode line by line.
4. This is why Python is called "interpreted" even though there technically *is* a compilation step — it's just not to native machine code/assembly like C, and it happens automatically/transparently every time you run the script.


- Basicly, the reason Python is slower than C: there's an extra virtual machine layer interpreting instructions instead of the CPU running native machine code directly.


## 4. Syntax Basics

Core things a total beginner needs before anything else:

```python
# This is a comment

# Indentation defines code blocks (no { } like in C)
if True:
    print("Indented block")
    print("Still inside the if")

print("Outside the if")

# No semicolons needed
x = 5
y = 10

# Variables: no type declaration needed
name = "Alice"     # str
age = 25           # int
height = 1.70      # float
is_admin = False   # bool
```

Key points to emphasize:
- **Indentation is not a style choice, it's syntax.** Mixing tabs and spaces, or inconsistent indentation, causes errors.
- Python is **case-sensitive** (`Name` ≠ `name`).
- Variable naming convention: `snake_case` for variables and functions.
- No variable declaration keyword (no `int x;` like C) — a variable is created the moment you assign to it.


## 5. Data Types

Quick overview table before diving deeper:

| Type | Example | Mutable? |
|---|---|---|
| `int` | `42` | No |
| `float` | `3.14` | No |
| `str` | `"hello"` | No |
| `bool` | `True` / `False` | No |
| `list` | `[1, 2, 3]` | **Yes** |
| `tuple` | `(1, 2, 3)` | No |
| `dict` | `{"key": "value"}` | **Yes** |
| `set` | `{1, 2, 3}` | Yes |
| `None` | `None` | (represents "no value") |

Use `type(x)` to check the type of anything:

```python
print(type(42))        # <class 'int'>
print(type("hi"))      # <class 'str'>
print(type([1, 2]))    # <class 'list'>
```

**Mutable vs immutable is worth a slide of its own** — see [section 14](#14-mutable-vs-immutable-types).


## 6. `print()` / f-strings and `input()`

### `print()`

```python
print("Hello, world!")
print("Age:", 25)
print("A", "B", "C", sep="-")     # A-B-C
print("No newline", end="")       # stays on same line
```

### f-strings (`print(f"...")`) — the modern, preferred way to format strings

```python
name = "Marta"
age = 22

print(f"My name is {name} and I am {age} years old.")
print(f"Next year I'll be {age + 1}.")     # expressions work inside {}
print(f"Pi is approximately {3.14159:.2f}")  # formatting: 2 decimal places
```

Why f-strings over old methods (worth showing the contrast):

```python
# old / less readable ways
print("My name is " + name + " and I am " + str(age))
print("My name is {} and I am {}".format(name, age))
print("My name is %s and I am %d" % (name, age))

# f-string cleaner, faster, easiest to read
print(f"My name is {name} and I am {age}")
```

### `input()`

```python
name = input("What's your name? ")
print(f"Hello, {name}!")

# input() ALWAYS returns a string — must convert manually for numbers
age = input("How old are you? ")
age = int(age)          # convert str -> int
print(f"Next year you'll be {age + 1}")
```

⚠️ **Common beginner trap:** forgetting that `input()` returns a `str`, then trying to do math on it and getting a `TypeError`, use `int()` or `float()` if you are going to use them as numbers.


## 7. Core Types: `int`, `str`, `list`, `dict`

### `int`

```python
a = 10
b = 3

print(a + b)   # 13
print(a - b)   # 7
print(a * b)   # 30
print(a / b)   # 3.333... (float division)
print(a // b)  # 3 (integer/floor division)
print(a % b)   # 1 (modulo/remainder)
print(a ** b)  # 1000 (exponent)
```

### `str`

Strings are **immutable** sequences of characters.

```python
s = "Hello, Python!"

print(s[0])         # 'H'          -> indexing
print(s[-1])        # '!'          -> negative indexing (from the end)
print(s[::-1])      # '!'          -> reverse str in place, no loop/function
print(s[0:5])       # 'Hello'      -> slicing
print(s[:5])        # 'Hello'
print(s[7:])        # 'Python!'
print(len(s))       # 14
print(s.upper())    # "HELLO, PYTHON!"
print(s + " Bye!")  # concatenation
```

### `list`

Ordered, **mutable**, can hold mixed types.

```python
fruits = ["apple", "banana", "cherry"]

fruits.append("orange")     # add to end
fruits.insert(0, "kiwi")    # insert at index
fruits.remove("banana")     # remove by value
fruits.pop()                # remove & return last item
print(fruits[1])            # indexing
print(fruits[1:3])          # slicing
print(fruits[::-1])         # reversing the list
print(len(fruits))          # lenght of a list 
print("apple" in fruits)    # membership check -> True

# list comprehension, one line loop

guests = [guest for guest in people if guest.is_going is True]
# ["Denys", "Diogo", "Dinis"]  -> Pedro and Gabriel arent going
```

### `dict`

Key-value pairs, **mutable**.

Dicts are a very cool way of storing data, instead of indexes, we use the word/key to indetify what we want to select.

```python
student = {
    "name": "Alice",
    "age": 21,
    "courses": ["Math", "CS"]
}

print(student["name"])           # 'Alice'          -> direct access
print(student.get("age"))        # 21               -> safe access
print(student.get("gpa", "N/A")) # 'N/A'            -> default if key missing

student["age"] = 22             # update
student["gpa"] = 3.8            # add new key

for key, value in student.items():
    print(f"{key}: {value}")
```

## 8. Methods for Types

### String methods

```python
s = "  Hello, World!  "

s.strip()          # "Hello, World!"          -> removes leading/trailing whitespace
s.lower()          # "  hello, world!  "
s.upper()          # "  HELLO, WORLD!  "
s.replace("World", "Python")  # "  Hello, Python!  "
s.split(",")       # ['  Hello', ' World!  ']  -> splits into a list
s.find("World")    # index of substring, or -1 if not found
s.startswith("  He")  # True
s.endswith("!  ")     # True
s.isdigit()        # True if all characters are digits

words = ["Hello", "World"]
" ".join(words)    # "Hello World"  -> joins a list into a string
```

### List methods

```python
nums = [3, 1, 4, 1, 5]

nums.append(9)        # add to end
nums.sort()           # sort in place
nums.reverse()        # reverse in place
nums.count(1)         # how many times 1 appears
nums.index(4)         # index of first occurrence of 4
nums.extend([2, 6])   # append multiple items
```

### Dict methods

```python
d = {"a": 1, "b": 2}

d.get("a")          # 1
d.get("z", 0)       # 0 (default, avoids KeyError)
d.keys()            # dict_keys(['a', 'b'])
d.values()          # dict_values([1, 2])
d.items()           # dict_items([('a', 1), ('b', 2)])
d.pop("a")          # removes 'a' and returns its value
d.update({"c": 3})  # merge/add another dict
```

**Tip:** use `.get()` vs `d["key"]` use `.get()`, because if you use [] for a key that doesnt exists, it raises `KeyError`, while `.get()` returns `None` (or a default) instead.


## 9. Built-in Functions

Functions available without importing anything:

```python
len([1, 2, 3])          # 3
type(3.14)               # <class 'float'>
range(5)                 # 0,1,2,3,4  (used mostly in for loops)
sorted([3, 1, 2])         # [1, 2, 3]  -> returns NEW sorted list
sum([1, 2, 3])            # 6
min([4, 2, 9])            # 2
max([4, 2, 9])            # 9
abs(-7)                   # 7
enumerate(["a", "b"])     # gives (index, value) pairs -> great in for loops
map(str, [1, 2, 3])       # applies a function to every item
filter(lambda x: x > 2, [1, 2, 3, 4])  # filters items by condition
int("5"), str(5), float("3.14"), list("abc")  # type conversions
isinstance(5, int)        # True -> type checking
```

## 10. The Standard Library

Python arguably has the largest set of libraries. Worth showing a few:

```python
import math
print(math.sqrt(16))     # 4.0
print(math.pi)            # 3.14159...

import random
print(random.randint(1, 10))     # random int between 1 and 10
print(random.choice(["a", "b", "c"]))

import datetime
print(datetime.date.today())

import os
print(os.getcwd())        # current working directory

import sys
print(sys.argv)           # command-line arguments

import json
data = json.dumps({"a": 1})   # dict -> JSON string
```


## 11. `for` Loops

```python
# looping over a range of numbers
for i in range(5):
    print(i)             # 0 1 2 3 4

# looping over a list directly
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)     # apple, banana, cherry, 

# looping with index using enumerate
for index, fruit in enumerate(fruits):
    print(index, fruit)

# looping over a dict
student = {"name": "Alice", "age": 21}
for key, value in student.items():
    print(key, value)

# loop control
for i in range(10):
    if i == 3:
        continue    # skip this iteration
    if i == 6:
        break        # exit the loop entirely
    print(i)

# else on a for loop (runs if loop completes without break)
for i in range(3):
    print(i)
else:
    print("Loop finished without break")
```

Also worth a quick mention: **`while` loops** exist too (condition-based rather than sequence-based) but in python for loops are prefered:

```python
count = 0
while count < 5:
    print(count)
    count += 1
```


## 12. `if` / `elif` / `else` and Operators

Needed before loops and functions make full sense — worth covering early even if only briefly.

```python
age = 20

if age < 13:
    print("Child")
elif age < 20:
    print("Teenager")
else:
    print("Adult")
```
*Tip*: if you want if a value is null/empty:

```python

if !some_variable:   # C way ❌
    return

if !some_variable == None:  # C way ❌
    return

if some_variable is None:   # Python way ✅
    print("Child")


```

### Comparison operators

```python
a = 5
b = 10

print(a == b)   # False -> equal to
print(a != b)   # True  -> not equal to
print(a < b)    # True
print(a > b)    # False
print(a <= 5)   # True
print(a >= 10)  # False
```

### Logical operators

```python
age = 25
has_id = True

if age >= 18 and has_id:
    print("Can enter")

if age < 18 or not has_id:
    print("Cannot enter")
```


## 13. Defining Functions (`def`)

Arguably more foundational than OOP, and used constantly in the modules leading up to it.

```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))   # Hello, Alice!
```

```python
# default arguments
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Bob"))              # Hello, Bob!
print(greet("Bob", "Hi"))        # Hi, Bob!

# multiple parameters and return values
def add_and_multiply(a, b) -> int:
    return a + b, a * b

# a good practice for readable code, put type hints
# in your functions, since in python you dont need
# to specify your function return type for it to run

total, product = add_and_multiply(3, 4)
print(total, product)   # 7 12
```


## 14. Mutable vs Immutable Types

A classic beginner confusion point.

```python
a = [1, 2, 3]  # list a
b = a  # list b equals a, tricky
b.append(4)  # save 4 on b
print(a)   # [1, 2, 3, 4]  <- surprising for beginners! lists are mutable and on second line, when we say its equal to a, its passing the reference to a pointer (kinda), its not passing value, its the reference 
```

```c
# its like doing this in C
value_a = &value_b;
```

Compare with an immutable type, where this "surprise" doesn't happen:

```python
x = "hello"
y = x
y += " world"
print(x)   # "hello"        -> unaffected, strings are immutable
print(y)   # "hello world"
```

- **Immutable** (cannot be changed after creation): `int`, `float`, `str`, `bool`, `tuple`.
- **Mutable** (can be changed in place): `list`, `dict`, `set`.

If you actually want an independent copy of a mutable object, you need to copy it explicitly:

```python
a = [1, 2, 3]
b = a.copy()      # or list(a), or a[:]
b.append(4)
print(a)   # [1, 2, 3]       -> unaffected now
print(b)   # [1, 2, 3, 4]
```


## 15. Exceptions / `try`-`except`

How Python handles errors instead of crashing outright.

```python
try:
    num = int(input("Enter a number: "))
    print(10 / num)
except ValueError:
    print("That's not a valid number!")
except ZeroDivisionError:
    print("Can't divide by zero!")
```

Other useful pieces to show:

```python
try:
    risky_operation()
except Exception as e:
    print(f"Something went wrong: {e}")
else:
    print("This runs only if no exception occurred")
finally:
    print("This always runs, error or not")
```

**Tip:** this pairs well with the earlier `input()` example , most beginners will have already hit a crash from typing letters instead of numbers, so this section directly fixes a problem they've likely already seen.


## 16. Unpacking / Multiple Assignment

Small but very handy.

```python
a, b = 1, 2
print(a, b)   # 1 2

a, b = b, a   # swap without a temp variable
print(a, b)   # 2 1

# unpacking a list
coordinates = [10, 20, 30]
x, y, z = coordinates

# common pattern: unpacking in a for loop (already seen with dict.items() and enumerate())
pairs = [(1, "a"), (2, "b")]
for number, letter in pairs:
    print(number, letter)
```


## 17. Object-Oriented Programming (OOP)

### Creating a class

```python
class Dog:
    # class attribute (shared by all instances)
    species = "Canis familiaris"

    # constructor: runs when a new object is created
    def __init__(self, name, age):
        self.name = name    # instance attribute
        self.age = age

    # instance method
    def bark(self):
        print(f"{self.name} says Woof!")

    def get_age_in_dog_years(self):
        return self.age * 7

    # special method: controls what print(dog) shows
    def __str__(self):
        return f"Dog(name={self.name}, age={self.age})"
```

### Instantiating (creating objects from) a class

```python
my_dog = Dog("Rex", 3)       # creates an instance
other_dog = Dog("Bella", 5)

print(my_dog.name)            # Rex
my_dog.bark()                 # Rex says Woof!
print(my_dog.get_age_in_dog_years())  # 21
print(my_dog)                 # Dog(name=Rex, age=3)  (thanks to __str__)
```

### Key OOP vocabulary to define clearly

- **Class**: a blueprint/template.
- **Object / Instance**: a concrete thing built from that blueprint.
- **Attribute**: a variable that belongs to an object (`self.name`).
- **Method**: a function that belongs to a class (`def bark(self):`).
- **`self`**: refers to the specific instance calling the method — always the first parameter of instance methods.
- **`__init__`**: the constructor, automatically called when you create a new object.

## Inheritance

Inheritance is a mechanism that allows one class (called the child or subclass) to acquire the properties (fields/attributes) and behaviors (methods/functions) of another class (called the parent or superclass).

It promotes code reusability, extensibility, and a clear hierarchical structure.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Cat(Animal):          # Cat inherits from Animal
    def speak(self):         # method overriding
        print(f"{self.name} says Meow!")

c = Cat("Whiskers")
c.speak()   # Whiskers says Meow!
```

### Common uses of classes

- Modeling real-world entities (`User`, `Product`, `BankAccount`) with data + behavior bundled together.
- Grouping related functions and data instead of passing many loose variables around.
- Building reusable components (e.g., a `Animal` base class with `Dog`, `Cat` subclasses).
- Representing game entities (`Player`, `Enemy`), GUI elements, or data structures (`Node` for a linked list).
- Structuring larger programs so related logic doesn't live in scattered global functions.


## 18. General Good Practices

- **Naming variables**: `snake_case` for variables/functions, `PascalCase` for classes, `UPPER_CASE` for constants.
- **Write docstrings** for functions/classes to explain what they do:
  ```python
  def add(a, b):
      """Return the sum of a and b."""
      return a + b
  ```
- **Don't repeat yourself (DRY)** — if you copy-paste code, consider turning it into a function.
- **Use meaningful variable names** — `user_age` instead of `x`.
- **Handle errors properly** with `try`/`except` instead of letting the program crash (see section 15).
- **Avoid overusing global variables** — pass data through function parameters instead.
- **Keep functions small and focused** — one function should do one thing (separation of concerns).
- **Use virtual environments** (`venv`) per project so dependencies don't clash between projects.
- **Comment the "why", not the "what"** — code already shows *what* it does; comments should explain *why* a decision was made.
- **Test your code incrementally** — run small pieces as you write them instead of writing 200 lines blind.

<br>

# Thank you for your time!
#### By Diogo and Denys