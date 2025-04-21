# Part 1: Python Fundamentals Refresher

## Introduction

Welcome to Part 1 of our practical Python course! Before diving into more advanced topics like classes, callbacks, and asyncio, we need to ensure you have a solid grasp of Python fundamentals. This section serves as a refresher, focusing on the building blocks you'll need for the real-world applications we'll develop later.

Each module includes explanations, small code examples, and hands-on exercises to reinforce your learning. Remember, the best way to learn programming is by writing code yourself!

## Module 1.1: Variables, Data Types, and Basic Operations

### Variables and Assignment

In Python, variables are names that refer to values. You create a variable by assigning a value to it using the `=` operator:

```python
# Variable assignment
name = "Alice"
age = 30
is_student = False
```

Python is dynamically typed, meaning you don't need to declare a variable's type before using it, and you can change its type later:

```python
# Changing variable types is allowed
x = 10        # x is an integer
x = "hello"   # Now x is a string
```

### Core Data Types

Python has several built-in data types:

1. **Numeric Types**:
    - **int**: Whole numbers like `42`, `-7`, `0`
    - **float**: Decimal numbers like `3.14`, `-0.001`, `2.0`
    - **complex**: Complex numbers like `1+2j`

2. **Sequence Types**:
    - **str**: Text strings like `"hello"`, `'world'`
    - **list**: Ordered, mutable collections like `[1, 2, 3]`
    - **tuple**: Ordered, immutable collections like `(1, 2, 3)`

3. **Mapping Type**:
    - **dict**: Key-value pairs like `{"name": "Alice", "age": 30}`

4. **Set Types**:
    - **set**: Unordered collections of unique items like `{1, 2, 3}`
    - **frozenset**: Immutable sets

5. **Boolean Type**:
    - **bool**: `True` or `False`

6. **None Type**:
    - **NoneType**: The value `None` representing absence of value

### Basic Operations

Python provides standard operations for different data types:

**Arithmetic Operations**:
```python
# Addition
sum_result = 5 + 3     # 8

# Subtraction
diff_result = 10 - 4   # 6

# Multiplication
product = 3 * 4        # 12

# Division (returns float)
quotient = 8 / 4       # 2.0

# Integer division (returns int)
int_quotient = 7 // 3  # 2

# Modulus (remainder)
remainder = 7 % 3      # 1

# Exponentiation
power = 2 ** 3         # 8
```

**String Operations**:
```python
# Concatenation
greeting = "Hello" + " " + "World"  # "Hello World"

# Repetition
repeated = "echo " * 3              # "echo echo echo "

# Indexing (starts at 0)
first_char = "Python"[0]            # "P"

# Slicing
substring = "Python"[1:4]           # "yth"
```

**List Operations**:
```python
# Creating a list
fruits = ["apple", "banana", "cherry"]

# Accessing elements (indexing)
first_fruit = fruits[0]             # "apple"

# Modifying elements
fruits[1] = "blueberry"             # ["apple", "blueberry", "cherry"]

# Adding elements
fruits.append("date")               # ["apple", "blueberry", "cherry", "date"]

# List concatenation
more_fruits = fruits + ["elderberry"]  # Adds "elderberry" to the end
```

### Type Conversion

Python allows converting between types:

```python
# String to int
num_str = "42"
num = int(num_str)     # 42 (as integer)

# Int to string
count = 10
count_str = str(count) # "10" (as string)

# String to float
pi_str = "3.14159"
pi = float(pi_str)     # 3.14159 (as float)

# Bool conversions
bool(0)      # False
bool(1)      # True
bool("")     # False
bool("text") # True
```

### Exercise 1: Calculator

Create a simple calculator that:
1. Asks the user for two numbers
2. Performs addition, subtraction, multiplication, and division
3. Prints the results with appropriate labels

**Hint**: Use `input()` to get user input, but remember it returns strings! Convert to numbers before calculating.

## Module 1.2: Control Flow (if/else, loops)

### Conditional Statements

Conditional statements allow your code to make decisions:

```python
# Basic if statement
age = 18
if age >= 18:
    print("You are an adult")

# if-else statement
temperature = 15
if temperature > 20:
    print("It's warm")
else:
    print("It's cool")

# if-elif-else for multiple conditions
score = 85
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"
```

### Comparison and Logical Operators

Python provides several operators for comparisons and logical operations:

**Comparison Operators**:
- `==` Equal to
- `!=` Not equal to
- `>` Greater than
- `<` Less than
- `>=` Greater than or equal to
- `<=` Less than or equal to

**Logical Operators**:
- `and` True if both conditions are true
- `or` True if at least one condition is true
- `not` Inverts a boolean value

```python
# Using comparison and logical operators
age = 25
has_license = True

if age >= 18 and has_license:
    print("You can drive")

if not has_license:
    print("You need a license first")

if age < 18 or not has_license:
    print("You cannot drive")
```

### Loops

Loops allow you to repeat code multiple times:

**For Loops**:
```python
# Looping through a range of numbers
for i in range(5):  # 0, 1, 2, 3, 4
    print(i)

# Looping through a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Looping with index using enumerate
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
```

**While Loops**:
```python
# Basic while loop
count = 0
while count < 5:
    print(count)
    count += 1  # Don't forget to increment!

# While loop with break
while True:
    response = input("Continue? (y/n): ")
    if response.lower() == 'n':
        break  # Exit the loop
```

**Loop Control**:
- `break` exits the loop completely
- `continue` skips to the next iteration
- `else` clause executes after the loop completes (unless broken)

```python
# Using continue
for num in range(10):
    if num % 2 == 0:
        continue  # Skip even numbers
    print(num)    # Print only odd numbers

# Using else with for loop
for i in range(5):
    print(i)
else:
    print("Loop completed successfully")
```

### Exercise 2: Number Guessing Game

Create a number guessing game that:
1. Generates a random number between 1 and 100
2. Asks the user to guess the number
3. Tells them if their guess is too high or too low
4. Continues until they guess correctly
5. Congratulates them and tells them how many guesses it took

**Hint**: Use `import random` and `random.randint(1, 100)` to generate a random number.

## Module 1.3: Functions and Scope

### Defining Functions

Functions are reusable blocks of code:

```python
# Basic function definition
def greet(name):
    """This is a docstring describing what the function does."""
    return f"Hello, {name}!"

# Calling the function
message = greet("Alice")  # "Hello, Alice!"
```

### Parameters and Arguments

Functions can accept different kinds of parameters:

```python
# Default parameters
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Bob")            # "Hello, Bob!"
greet("Bob", "Hi")      # "Hi, Bob!"

# Keyword arguments
greet(greeting="Hey", name="Charlie")  # "Hey, Charlie!"

# Variable number of arguments
def sum_all(*numbers):
    return sum(numbers)

sum_all(1, 2, 3, 4)  # 10

# Variable keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Dave", age=30, country="Canada")
```

### Return Values

Functions can return values to the caller:

```python
# Returning a single value
def square(x):
    return x * x

result = square(4)  # 16

# Returning multiple values (as a tuple)
def min_max(numbers):
    return min(numbers), max(numbers)

minimum, maximum = min_max([5, 2, 8, 1, 3])  # minimum=1, maximum=8

# Early returns
def absolute(x):
    if x >= 0:
        return x
    return -x  # Only executed if x is negative
```

### Variable Scope

Python has different scopes for variables:

```python
# Global scope
global_var = "I'm global"

def my_function():
    # Local scope
    local_var = "I'm local"
    print(global_var)  # Can access global variables
    print(local_var)   # Can access local variables

my_function()
# print(local_var)  # This would cause an error - local_var is not accessible here

# Modifying global variables
def modify_global():
    global global_var
    global_var = "Modified global"

modify_global()
print(global_var)  # "Modified global"
```

### Lambda Functions

Lambda functions are small, anonymous functions defined with the `lambda` keyword:

```python
# Basic lambda function
square = lambda x: x * x
print(square(5))  # 25

# Lambda with multiple parameters
add = lambda x, y: x + y
print(add(3, 4))  # 7

# Lambda in higher-order functions
numbers = [1, 5, 3, 2, 4]
sorted_numbers = sorted(numbers, key=lambda x: -x)  # Sort in descending order
print(sorted_numbers)  # [5, 4, 3, 2, 1]
```

### Exercise 3: Temperature Converter

Create a module with functions that:
1. Convert Celsius to Fahrenheit
2. Convert Fahrenheit to Celsius
3. Convert Celsius to Kelvin
4. Allow the user to choose which conversion to perform

**Hint**: The formulas are:
- F = C * 9/5 + 32
- C = (F - 32) * 5/9
- K = C + 273.15

## Module 1.4: Working with Standard Data Structures

### Lists in Depth

Lists are versatile, ordered collections:

```python
# Creating lists
empty_list = []
numbers = [1, 2, 3, 4, 5]
mixed = [1, "two", 3.0, [4, 5]]

# Common list operations
len(numbers)           # 5
numbers + [6, 7]       # [1, 2, 3, 4, 5, 6, 7] (concatenation)
3 in numbers           # True (membership test)
numbers.index(4)       # 3 (find index of value)
numbers.count(2)       # 1 (count occurrences)

# List methods
numbers.append(6)      # Add to end: [1, 2, 3, 4, 5, 6]
numbers.insert(0, 0)   # Insert at position: [0, 1, 2, 3, 4, 5, 6]
numbers.remove(3)      # Remove first occurrence: [0, 1, 2, 4, 5, 6]
popped = numbers.pop() # Remove and return last item: 6
numbers.sort()         # Sort in place: [0, 1, 2, 4, 5]
numbers.reverse()      # Reverse in place: [5, 4, 2, 1, 0]

# List comprehensions
squares = [x**2 for x in range(1, 6)]  # [1, 4, 9, 16, 25]
even_squares = [x**2 for x in range(1, 11) if x % 2 == 0]  # [4, 16, 36, 64, 100]
```

### Dictionaries in Depth

Dictionaries store key-value pairs:

```python
# Creating dictionaries
empty_dict = {}
person = {"name": "Alice", "age": 30, "is_student": False}
grades = dict(Alice=90, Bob=85, Charlie=95)  # Alternative creation

# Accessing values
person["name"]             # "Alice"
person.get("address")      # None (key doesn't exist)
person.get("address", "Unknown")  # "Unknown" (default value)

# Modifying dictionaries
person["address"] = "123 Main St"  # Add new key-value pair
person["age"] = 31                 # Modify existing value
del person["is_student"]           # Remove key-value pair

# Dictionary methods
person.keys()              # dict_keys(['name', 'age', 'address'])
person.values()            # dict_values(['Alice', 31, '123 Main St'])
person.items()             # dict_items([('name', 'Alice'), ('age', 31), ('address', '123 Main St')])
person.update({"phone": "555-1234", "age": 32})  # Add/update multiple items

# Dictionary comprehensions
square_dict = {x: x**2 for x in range(1, 6)}  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

### Sets in Depth

Sets are unordered collections of unique items:

```python
# Creating sets
empty_set = set()  # Note: {} creates an empty dict, not a set
fruits = {"apple", "banana", "cherry"}
numbers = set([1, 2, 2, 3, 3, 3])  # Creates {1, 2, 3} (duplicates removed)

# Set operations
len(fruits)                    # 3
"apple" in fruits              # True
fruits.add("date")             # Add an item
fruits.remove("banana")        # Remove an item (raises error if not found)
fruits.discard("nonexistent")  # Remove if present (no error if not found)

# Mathematical set operations
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}
set1 | set2  # Union: {1, 2, 3, 4, 5, 6}
set1 & set2  # Intersection: {3, 4}
set1 - set2  # Difference: {1, 2}
set1 ^ set2  # Symmetric difference: {1, 2, 5, 6}

# Set comprehensions
even_set = {x for x in range(10) if x % 2 == 0}  # {0, 2, 4, 6, 8}
```

### Working with Tuples

Tuples are immutable sequences:

```python
# Creating tuples
empty_tuple = ()
single_item = (1,)  # Note the comma
coordinates = (10, 20)
mixed = (1, "two", 3.0)

# Tuple operations
len(coordinates)        # 2
coordinates[0]          # 10
coordinates + (30,)     # (10, 20, 30)
3 in mixed              # False

# Tuple unpacking
x, y = coordinates      # x = 10, y = 20
first, *rest = (1, 2, 3, 4)  # first = 1, rest = [2, 3, 4]

# Named tuples (more readable)
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
p.x  # 10
p.y  # 20
```

### Exercise 4: Contacts Manager

Create a simple contacts manager that:
1. Stores contacts as dictionaries in a list
2. Allows adding new contacts with name, phone, and email
3. Allows searching for contacts by name
4. Allows deleting contacts
5. Displays all contacts in alphabetical order

**Hint**: Each contact could be a dictionary like `{"name": "Alice", "phone": "555-1234", "email": "alice@example.com"}`.

## Project: Command-line Calculator

Now let's put it all together in a small project - a command-line calculator that:

1. Provides basic arithmetic operations (add, subtract, multiply, divide)
2. Includes memory functions (store, recall, clear)
3. Supports a history of calculations
4. Allows user to exit gracefully
5. Handles errors (like division by zero) properly

Requirements:
- Use functions to organize your code
- Use appropriate data structures to store history and memory
- Implement a menu-driven interface
- Make use of loops for continuous operation
- Use conditional statements for decision making

This project will reinforce all the fundamental concepts we've covered in Part 1.

### Getting Started:

Here's a skeleton to help you begin:

```python
def add(x, y):
    """Add two numbers and return the result."""
    return x + y

def subtract(x, y):
    """Subtract y from x and return the result."""
    # Your code here

# Implement multiply and divide functions

def calculator():
    """Main calculator function."""
    history = []
    memory = 0
    
    while True:
        print("\nCalculator Menu:")
        print("1. Add")
        print("2. Subtract")
        # Add more menu options
        print("0. Exit")
        
        choice = input("Enter your choice: ")
        
        # Add your menu handling logic here
        
        # Don't forget error handling!

if __name__ == "__main__":
    calculator()
```

Work on this project step by step, testing each feature as you implement it. Remember that the goal is to apply what you've learned about Python fundamentals in a practical context.

Good luck!