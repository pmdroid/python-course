# Part 2: Object-Oriented Programming with Python

## Introduction

Welcome to Part 2 of our practical Python course! In this section, we'll explore Object-Oriented Programming (OOP) - a powerful paradigm that allows you to organize your code around objects that combine data and functionality. By the end of this part, you'll understand how to use classes and objects to build more maintainable, reusable, and organized code.

Remember, learning programming requires practice. Don't just read the examples - type them out, experiment with them, and complete the exercises!

## Module 2.1: Introduction to Classes and Objects

### What Are Classes and Objects?

A class is a blueprint for creating objects. Objects are instances of classes that bundle related data (attributes) and functionality (methods) together. Think of a class as a cookie cutter and objects as the cookies you make with it.

```python
# Define a simple class
class Dog:
    pass  # Empty class for now

# Create an object (instance of the class)
my_dog = Dog()  # my_dog is an object of class Dog
```

### Creating Classes with Attributes

Let's add attributes to our class:

```python
class Dog:
    # Class attribute - shared by all instances
    species = "Canis familiaris"
    
    # Initializer method (__init__)
    def __init__(self, name, age):
        # Instance attributes - unique to each instance
        self.name = name
        self.age = age
```

The `__init__` method is a special method called when you create a new object. It initializes the object's attributes.

The `self` parameter refers to the instance being created. It must be the first parameter of any method that operates on the instance.

### Creating and Using Objects

Now let's create some dog objects:

```python
# Create dog objects
buddy = Dog("Buddy", 5)
miles = Dog("Miles", 2)

# Access attributes
print(buddy.name)        # "Buddy"
print(miles.age)         # 2
print(buddy.species)     # "Canis familiaris"
print(miles.species)     # "Canis familiaris"

# Modify attributes
buddy.age = 6
print(buddy.age)         # 6
```

### Adding Methods to Classes

Methods are functions defined inside a class. They define what an object can do:

```python
class Dog:
    species = "Canis familiaris"
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    # Instance method
    def description(self):
        return f"{self.name} is {self.age} years old"
    
    # Another instance method
    def speak(self, sound):
        return f"{self.name} says {sound}"
```

Let's use these methods:

```python
buddy = Dog("Buddy", 5)

# Call instance methods
print(buddy.description())      # "Buddy is 5 years old"
print(buddy.speak("Woof"))      # "Buddy says Woof"
```

### String Representation with `__str__`

The `__str__` method controls how an object is represented as a string:

```python
class Dog:
    species = "Canis familiaris"
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def __str__(self):
        return f"{self.name}, {self.age} years old"
    
    def speak(self, sound):
        return f"{self.name} says {sound}"
```

Now when you print a Dog object:

```python
buddy = Dog("Buddy", 5)
print(buddy)  # "Buddy, 5 years old"
```

### Exercise 1: Create a Circle Class

Create a `Circle` class that:
1. Initializes with a radius
2. Has methods to calculate area and circumference
3. Has a string representation that shows the radius and area

**Hint**: Use `math.pi` from the math module for π (pi).

## Module 2.2: Methods, Attributes, and Encapsulation

### Instance Methods vs. Class Methods

Python has different types of methods:

**Instance Methods** operate on an instance (object) and can access/modify instance attributes:

```python
class Dog:
    def __init__(self, name):
        self.name = name
    
    # Instance method (uses self)
    def bark(self):
        return f"{self.name} says Woof!"
```

**Class Methods** operate on the class itself, not on instances. They receive the class as the first parameter, typically named `cls`:

```python
class Dog:
    count = 0  # Class attribute to track number of dogs
    
    def __init__(self, name):
        self.name = name
        Dog.count += 1
    
    # Instance method
    def bark(self):
        return f"{self.name} says Woof!"
    
    # Class method (uses cls instead of self)
    @classmethod
    def get_dog_count(cls):
        return f"There are {cls.count} dogs"
```

Using class methods:

```python
buddy = Dog("Buddy")
miles = Dog("Miles")

print(Dog.get_dog_count())  # "There are 2 dogs"
```

### Static Methods

Static methods are related to a class but don't operate on the class or instances. They don't receive a special first parameter:

```python
class Dog:
    @staticmethod
    def is_adult(age):
        return age >= 2
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.is_adult_dog = Dog.is_adult(age)
```

Using static methods:

```python
print(Dog.is_adult(1))  # False
print(Dog.is_adult(3))  # True

puppy = Dog("Puppy", 1)
adult = Dog("Buddy", 3)

print(puppy.is_adult_dog)  # False
print(adult.is_adult_dog)  # True
```

### Property Decorators

Properties let you access methods as if they were attributes:

```python
class Dog:
    def __init__(self, name, age):
        self._name = name
        self._age = age
    
    # Getter property
    @property
    def name(self):
        return self._name
    
    # Setter property
    @name.setter
    def name(self, value):
        if not value:
            raise ValueError("Name cannot be empty")
        self._name = value
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value
    
    # Read-only property
    @property
    def human_age(self):
        return self._age * 7
```

Using properties:

```python
dog = Dog("Buddy", 5)

# Access properties like attributes
print(dog.name)       # "Buddy"
print(dog.age)        # 5
print(dog.human_age)  # 35

# Set properties
dog.name = "Max"
dog.age = 6

# This would raise an error:
# dog.human_age = 40  # Error: can't set attribute

# This would also raise an error:
# dog.age = -1  # Error: Age cannot be negative
```

### Encapsulation

Encapsulation is about hiding the internal details of an object and only exposing what's necessary. In Python, convention indicates private attributes with a leading underscore:

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner        # Public attribute
        self._balance = balance   # "Private" attribute (by convention)
    
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Amount must be positive")
        self._balance += amount
        return self._balance
    
    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Amount must be positive")
        if amount > self._balance:
            raise ValueError("Insufficient funds")
        self._balance -= amount
        return self._balance
    
    @property
    def balance(self):
        return self._balance
```

Using the encapsulated class:

```python
account = BankAccount("Alice", 1000)

# Access public attribute
print(account.owner)    # "Alice"

# Access through property
print(account.balance)  # 1000

# Modify through methods
account.deposit(500)
print(account.balance)  # 1500

account.withdraw(200)
print(account.balance)  # 1300

# Direct access to _balance is discouraged but possible
# print(account._balance)  # Not recommended
```

### Exercise 2: Temperature Class

Create a `Temperature` class that:
1. Stores a temperature value in Celsius
2. Provides properties for getting/setting the temperature in Celsius, Fahrenheit, and Kelvin
3. Ensures the temperature doesn't go below absolute zero (-273.15°C)

**Hint**: Use property decorators for the conversions.

## Module 2.3: Inheritance and Polymorphism

### Inheritance Basics

Inheritance allows a class (child/subclass) to inherit attributes and methods from another class (parent/superclass):

```python
# Parent class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"

# Child class inheriting from Animal
class Dog(Animal):
    def speak(self):  # Override the speak method
        return f"{self.name} says Woof!"

# Another child class
class Cat(Animal):
    def speak(self):  # Override the speak method
        return f"{self.name} says Meow!"
```

Using inheritance:

```python
dog = Dog("Buddy")
cat = Cat("Whiskers")

print(dog.name)    # "Buddy" (inherited attribute)
print(cat.name)    # "Whiskers" (inherited attribute)

print(dog.speak())  # "Buddy says Woof!" (overridden method)
print(cat.speak())  # "Whiskers says Meow!" (overridden method)
```

### Calling the Parent's Methods with `super()`

The `super()` function allows you to call methods from the parent class:

```python
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species
    
    def info(self):
        return f"{self.name} is a {self.species}"

class Dog(Animal):
    def __init__(self, name, breed):
        # Call the parent's __init__ method
        super().__init__(name, "Dog")
        self.breed = breed
    
    def info(self):
        # Get the basic info from the parent method
        basic_info = super().info()
        # Add additional info
        return f"{basic_info} of breed {self.breed}"
```

Using `super()`:

```python
dog = Dog("Buddy", "Golden Retriever")
print(dog.info())  # "Buddy is a Dog of breed Golden Retriever"
```

### Multiple Inheritance

Python supports inheriting from multiple classes:

```python
class Swimmer:
    def swim(self):
        return "Swimming"

class Flyer:
    def fly(self):
        return "Flying"

# Inherit from both classes
class Duck(Swimmer, Flyer):
    def __init__(self, name):
        self.name = name
    
    def info(self):
        return f"{self.name} can both {self.swim()} and {self.fly()}"
```

Using multiple inheritance:

```python
duck = Duck("Daffy")
print(duck.swim())  # "Swimming"
print(duck.fly())   # "Flying"
print(duck.info())  # "Daffy can both Swimming and Flying"
```

### Method Resolution Order (MRO)

When using multiple inheritance, Python uses the Method Resolution Order (MRO) to determine which method to call:

```python
class A:
    def method(self):
        return "Method from A"

class B(A):
    def method(self):
        return "Method from B"

class C(A):
    def method(self):
        return "Method from C"

class D(B, C):
    pass
```

The MRO determines which `method()` is called:

```python
d = D()
print(d.method())  # "Method from B"

# Check the MRO
print(D.__mro__)  # Shows the order: D, B, C, A, object
```

### Polymorphism

Polymorphism means different classes can be used with the same interface:

```python
def make_speak(animal):
    return animal.speak()

# Different classes, same interface
dog = Dog("Buddy")
cat = Cat("Whiskers")

print(make_speak(dog))  # "Buddy says Woof!"
print(make_speak(cat))  # "Whiskers says Meow!"
```

### Abstract Base Classes

Abstract base classes define interfaces that subclasses must implement:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius
```

Using abstract base classes:

```python
# This would fail with an error:
# shape = Shape()  # Error: Can't instantiate abstract class

rectangle = Rectangle(5, 4)
circle = Circle(3)

print(rectangle.area())      # 20
print(circle.area())         # 28.27431
print(rectangle.perimeter()) # 18
print(circle.perimeter())    # 18.84954
```

### Exercise 3: Vehicle Hierarchy

Create a vehicle class hierarchy:
1. Start with a `Vehicle` base class with attributes like make, model, and year
2. Create subclasses for `Car`, `Motorcycle`, and `Truck`
3. Add appropriate methods and attributes to each subclass
4. Implement a `display_info()` method in each class

**Hint**: Use inheritance and override methods as needed.

## Module 2.4: Building Class Hierarchies

### Designing Class Hierarchies

When designing class hierarchies, consider these principles:
- Put common attributes and methods in the base class
- Specialize behavior in derived classes
- Use inheritance for "is-a" relationships
- Use composition for "has-a" relationships

```python
# Base class (generalization)
class Employee:
    def __init__(self, name, employee_id):
        self.name = name
        self.employee_id = employee_id
    
    def display_info(self):
        return f"{self.name}, ID: {self.employee_id}"

# Derived classes (specialization)
class Manager(Employee):
    def __init__(self, name, employee_id, department):
        super().__init__(name, employee_id)
        self.department = department
        self.employees = []
    
    def add_employee(self, employee):
        self.employees.append(employee)
    
    def display_info(self):
        base_info = super().display_info()
        return f"{base_info}, Manager of {self.department} department"

class Developer(Employee):
    def __init__(self, name, employee_id, programming_languages):
        super().__init__(name, employee_id)
        self.programming_languages = programming_languages
    
    def display_info(self):
        base_info = super().display_info()
        return f"{base_info}, Developer, Languages: {', '.join(self.programming_languages)}"
```

Using the hierarchy:

```python
dev1 = Developer("Alice", "D001", ["Python", "JavaScript"])
dev2 = Developer("Bob", "D002", ["Java", "C++"])
manager = Manager("Charlie", "M001", "Engineering")

manager.add_employee(dev1)
manager.add_employee(dev2)

print(dev1.display_info())     # "Alice, ID: D001, Developer, Languages: Python, JavaScript"
print(manager.display_info())  # "Charlie, ID: M001, Manager of Engineering department"
```

### Composition vs. Inheritance

Inheritance represents an "is-a" relationship, while composition represents a "has-a" relationship:

```python
# Composition example
class Address:
    def __init__(self, street, city, state, zipcode):
        self.street = street
        self.city = city
        self.state = state
        self.zipcode = zipcode
    
    def __str__(self):
        return f"{self.street}, {self.city}, {self.state} {self.zipcode}"

class Person:
    def __init__(self, name, address):
        self.name = name
        self.address = address  # Person "has-a" Address
    
    def __str__(self):
        return f"{self.name} lives at {self.address}"
```

Using composition:

```python
home_address = Address("123 Main St", "Anytown", "CA", "12345")
person = Person("Alice", home_address)

print(person)  # "Alice lives at 123 Main St, Anytown, CA 12345"
```

### Mixin Classes

Mixins are classes designed to provide additional functionality to other classes:

```python
# Mixin class
class JSONSerializableMixin:
    def to_json(self):
        import json
        return json.dumps(self.__dict__)

# Use the mixin with multiple classes
class Person(JSONSerializableMixin):
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Product(JSONSerializableMixin):
    def __init__(self, name, price):
        self.name = name
        self.price = price
```

Using mixins:

```python
person = Person("Alice", 30)
product = Product("Laptop", 999.99)

print(person.to_json())   # {"name": "Alice", "age": 30}
print(product.to_json())  # {"name": "Laptop", "price": 999.99}
```

### Interfaces and Protocols

In Python 3.8+, you can use Protocol classes to define interfaces:

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None:
        ...

# Classes that implement the protocol
class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def draw(self):
        print(f"Drawing a circle with radius {self.radius}")

class Square:
    def __init__(self, side):
        self.side = side
    
    def draw(self):
        print(f"Drawing a square with side {self.side}")

# Function that expects a Drawable
def render(item: Drawable):
    item.draw()
```

Using protocols:

```python
circle = Circle(5)
square = Square(4)

render(circle)  # "Drawing a circle with radius 5"
render(square)  # "Drawing a square with side 4"
```

### Exercise 4: Shape Hierarchy with Mixins

Create a shape hierarchy with:
1. A base `Shape` class
2. Derived classes for common shapes like `Rectangle`, `Circle`, etc.
3. A `ColorMixin` that adds color properties to shapes
4. A `StyleMixin` that adds style properties (filled, outline, etc.)

**Hint**: Use both inheritance and composition.

## Project: Simple Library Management System

Now, let's apply what we've learned about object-oriented programming to build a simple library management system.

### Project Requirements

Create a library management system with these features:
1. Track books, patrons, and loans
2. Allow books to be checked out and returned
3. Keep track of due dates
4. Apply late fees when necessary
5. Search for books by title, author, or ISBN

### Class Structure

Start with these classes:
- `Book`: Represents a book with title, author, ISBN, etc.
- `Patron`: Represents a library patron with name, ID, etc.
- `Loan`: Represents a book loan with book, patron, due date, etc.
- `Library`: Main class that manages books, patrons, and loans

### Implementation Guide

Here's a starting point for your implementation:

```python
from datetime import datetime, timedelta

class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.available = True
    
    def __str__(self):
        status = "Available" if self.available else "Checked Out"
        return f"'{self.title}' by {self.author} (ISBN: {self.isbn}) - {status}"

class Patron:
    def __init__(self, name, patron_id):
        self.name = name
        self.patron_id = patron_id
        self.books = []  # Books currently checked out
    
    def __str__(self):
        return f"{self.name} (ID: {self.patron_id}), Books checked out: {len(self.books)}"

class Loan:
    def __init__(self, book, patron, days=14):
        self.book = book
        self.patron = patron
        self.checkout_date = datetime.now()
        self.due_date = self.checkout_date + timedelta(days=days)
        self.returned = False
    
    def __str__(self):
        status = "Returned" if self.returned else f"Due on {self.due_date.strftime('%Y-%m-%d')}"
        return f"{self.book.title} borrowed by {self.patron.name} - {status}"

class Library:
    def __init__(self, name):
        self.name = name
        self.books = []
        self.patrons = []
        self.loans = []
    
    # Add methods for adding books, registering patrons, checking out books, etc.
```

### Extensions

Once you have the basic implementation working, consider adding:
- Book categories and searching by category
- Different loan periods for different types of books
- Reservation system for checked-out books
- Admin functions vs. patron functions
- Simple text-based user interface

This project will test your understanding of:
- Class design and relationships
- Inheritance and composition
- Properties and encapsulation
- Managing collections of objects

Take your time to design the system before you start coding, and remember to keep your methods focused and single-purpose.

Good luck!