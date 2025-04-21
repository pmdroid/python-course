# Part 3: Functional Programming Concepts

## Introduction

Welcome to Part 3 of our practical Python course! In this section, we'll explore functional programming concepts in Python, with special focus on callbacks and event-driven programming. While Python isn't a purely functional language, it incorporates many functional programming features that can make your code more concise, readable, and maintainable.

Functional programming treats computation as the evaluation of mathematical functions and avoids changing state and mutable data. Let's discover how to apply these concepts in Python through practical examples.

## Module 3.1: Functions as First-Class Objects

### Understanding First-Class Functions

In Python, functions are "first-class citizens," meaning they can be:
- Assigned to variables
- Passed as arguments to other functions
- Returned from other functions
- Stored in data structures

This flexibility is fundamental to functional programming.

### Assigning Functions to Variables

You can assign a function to a variable and call it through that variable:

```python
def greet(name):
    return f"Hello, {name}!"

# Assign function to a variable
say_hello = greet

# Call the function through the variable
result = say_hello("Alice")  # "Hello, Alice!"
```

### Storing Functions in Data Structures

Functions can be stored in lists, dictionaries, and other data structures:

```python
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y

def multiply(x, y):
    return x * y

def divide(x, y):
    return x / y if y != 0 else "Error: Division by zero"

# Store functions in a dictionary
operations = {
    "add": add,
    "subtract": subtract,
    "multiply": multiply,
    "divide": divide
}

# Use the dictionary to call functions
a, b = 10, 5
for op_name, op_func in operations.items():
    result = op_func(a, b)
    print(f"{op_name}: {a} and {b} = {result}")
```

### Passing Functions as Arguments

Functions can be passed as arguments to other functions:

```python
def apply_operation(x, y, operation):
    """Apply a binary operation to x and y."""
    return operation(x, y)

# Pass different functions as the operation
result1 = apply_operation(5, 3, add)        # 8
result2 = apply_operation(5, 3, subtract)   # 2
result3 = apply_operation(5, 3, multiply)   # 15
```

### Returning Functions from Functions

Functions can create and return other functions:

```python
def create_multiplier(factor):
    """Create a function that multiplies its input by factor."""
    def multiplier(x):
        return x * factor
    return multiplier

# Create specific multiplier functions
double = create_multiplier(2)
triple = create_multiplier(3)

# Use the created functions
print(double(5))  # 10
print(triple(5))  # 15
```

### Exercise 1: Function Sorter

Create a function called `sort_by_method` that:
1. Takes a list of numbers and a function as input
2. Sorts the numbers using the function as the sort key
3. Returns the sorted list

Then, create three different sorting methods:
- Sort by absolute value
- Sort by reciprocal value (1/x)
- Sort by last digit

**Hint**: Use the `sorted()` function with its `key` parameter.

## Module 3.2: Lambda Functions and Higher-Order Functions

### Lambda Functions

Lambda functions are small, anonymous functions defined with the `lambda` keyword:

```python
# Regular function
def add(x, y):
    return x + y

# Equivalent lambda function
add_lambda = lambda x, y: x + y

print(add(5, 3))        # 8
print(add_lambda(5, 3)) # 8
```

Lambdas are useful for short, simple functions that you use only once:

```python
# Sort a list of tuples by the second element
data = [(1, 'c'), (3, 'a'), (2, 'b')]
sorted_data = sorted(data, key=lambda item: item[1])
print(sorted_data)  # [(3, 'a'), (2, 'b'), (1, 'c')]

# Find all even numbers in a list
numbers = [1, 2, 3, 4, 5, 6]
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # [2, 4, 6]
```

### Higher-Order Functions

Higher-order functions are functions that either:
- Take one or more functions as arguments, or
- Return a function as a result

Python provides several built-in higher-order functions:

#### `map(function, iterable, ...)`

Applies a function to each item in an iterable:

```python
numbers = [1, 2, 3, 4, 5]

# Square each number
squared = map(lambda x: x ** 2, numbers)
squared_list = list(squared)  # [1, 4, 9, 16, 25]

# Convert each number to a string
strings = map(str, numbers)
strings_list = list(strings)  # ['1', '2', '3', '4', '5']

# With multiple iterables
first = [1, 2, 3]
second = [4, 5, 6]
sums = map(lambda x, y: x + y, first, second)
sums_list = list(sums)  # [5, 7, 9]
```

#### `filter(function, iterable)`

Creates an iterator of elements for which the function returns True:

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Keep only even numbers
evens = filter(lambda x: x % 2 == 0, numbers)
evens_list = list(evens)  # [2, 4, 6, 8, 10]

# Keep only numbers greater than 5
greater_than_five = filter(lambda x: x > 5, numbers)
gt_five_list = list(greater_than_five)  # [6, 7, 8, 9, 10]
```

#### `functools.reduce(function, iterable[, initializer])`

Applies a function cumulatively to the items of an iterable:

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]

# Calculate the sum
total = reduce(lambda x, y: x + y, numbers)
print(total)  # 15 (1+2+3+4+5)

# Calculate the product
product = reduce(lambda x, y: x * y, numbers)
print(product)  # 120 (1*2*3*4*5)

# Find the maximum
maximum = reduce(lambda x, y: x if x > y else y, numbers)
print(maximum)  # 5
```

### Function Composition

Function composition means creating a new function by combining existing functions:

```python
def compose(f, g):
    """Compose functions f and g: compose(f, g)(x) = f(g(x))"""
    return lambda x: f(g(x))

# Example functions
def double(x):
    return x * 2

def increment(x):
    return x + 1

# Compose the functions in different orders
double_then_increment = compose(increment, double)
increment_then_double = compose(double, increment)

print(double_then_increment(5))  # 11 (5*2 + 1)
print(increment_then_double(5))  # 12 ((5+1) * 2)
```

### Exercise 2: Functional Data Processing

Given a list of dictionaries representing people:

```python
people = [
    {"name": "Alice", "age": 25, "city": "New York"},
    {"name": "Bob", "age": 30, "city": "Chicago"},
    {"name": "Charlie", "age": 35, "city": "New York"},
    {"name": "Diana", "age": 28, "city": "Chicago"},
    {"name": "Eve", "age": 22, "city": "Boston"}
]
```

Write functional-style code to:
1. Find all people from New York
2. Calculate the average age of people from Chicago
3. Get a list of names in alphabetical order
4. Group people by city

**Hint**: Use a combination of `map`, `filter`, and lambda functions.

## Module 3.3: Callbacks and Event-Driven Programming

### Understanding Callbacks

A callback is a function passed as an argument to another function, which is then invoked ("called back") during the execution of that function. Callbacks are fundamental to event-driven programming and asynchronous operations.

#### Basic Callback Example

```python
def process_data(data, success_callback, error_callback):
    """Process data and call appropriate callback based on success."""
    try:
        result = data * 2  # Simple processing
        success_callback(result)
    except Exception as e:
        error_callback(str(e))

# Define callback functions
def on_success(result):
    print(f"Success! Result: {result}")

def on_error(error_message):
    print(f"Error occurred: {error_message}")

# Use the function with callbacks
process_data(5, on_success, on_error)  # "Success! Result: 10"
process_data("5", on_success, on_error)  # "Error occurred: can't multiply sequence by non-int of type 'int'"
```

#### Callbacks for Iterative Processing

Callbacks are useful for processing collections of data:

```python
def process_items(items, process_callback, complete_callback=None):
    """Process each item using the process_callback, then call complete_callback."""
    results = []
    for item in items:
        result = process_callback(item)
        results.append(result)
    
    if complete_callback:
        complete_callback(results)
    
    return results

# Define callbacks
def double_number(x):
    return x * 2

def print_summary(results):
    print(f"Processed {len(results)} items. Results: {results}")

# Use the function
numbers = [1, 2, 3, 4, 5]
process_items(numbers, double_number, print_summary)
# Prints: "Processed 5 items. Results: [2, 4, 6, 8, 10]"
```

### Callbacks with Context (Closures)

Callbacks can capture and use values from their enclosing scope:

```python
def create_counter_callback(counter_name):
    count = 0
    
    def callback():
        nonlocal count
        count += 1
        print(f"{counter_name} count: {count}")
    
    return callback

# Create different counters
button_a_counter = create_counter_callback("Button A")
button_b_counter = create_counter_callback("Button B")

# Simulate button clicks
button_a_counter()  # "Button A count: 1"
button_a_counter()  # "Button A count: 2"
button_b_counter()  # "Button B count: 1"
button_a_counter()  # "Button A count: 3"
```

### Event-Driven Programming

Event-driven programming uses callbacks to respond to events:

```python
class EventEmitter:
    def __init__(self):
        self.listeners = {}
    
    def on(self, event_name, callback):
        """Register a callback for the given event."""
        if event_name not in self.listeners:
            self.listeners[event_name] = []
        self.listeners[event_name].append(callback)
    
    def emit(self, event_name, *args, **kwargs):
        """Emit an event, calling all registered callbacks."""
        if event_name in self.listeners:
            for callback in self.listeners[event_name]:
                callback(*args, **kwargs)

# Create an event emitter
button = EventEmitter()

# Register event listeners (callbacks)
def on_click(button_id):
    print(f"Button {button_id} was clicked!")

def log_event(button_id):
    print(f"Event logged: Button {button_id} click at {time.time()}")

import time
button.on('click', on_click)
button.on('click', log_event)

# Simulate a button click
button.emit('click', 'Submit')
# Prints:
# "Button Submit was clicked!"
# "Event logged: Button Submit click at 1234567890.123"
```

### Callbacks for Asynchronous Operations

Callbacks are commonly used for operations that take time to complete:

```python
def read_file(filename, callback):
    """Simulate reading a file asynchronously."""
    try:
        with open(filename, 'r') as file:
            data = file.read()
        callback(data, None)
    except Exception as e:
        callback(None, str(e))

def process_file_data(data, error):
    """Callback for handling file data or errors."""
    if error:
        print(f"Error reading file: {error}")
    else:
        print(f"File contents ({len(data)} characters):")
        print(data[:100] + "..." if len(data) > 100 else data)

# Read a file with a callback
read_file("example.txt", process_file_data)
```

### Callback Patterns

#### Sequential Callbacks (Waterfall)

Execute a series of operations in sequence, passing results forward:

```python
def step1(data, next_callback):
    result = data + 10
    print(f"Step 1: {data} -> {result}")
    next_callback(result)

def step2(data, next_callback):
    result = data * 2
    print(f"Step 2: {data} -> {result}")
    next_callback(result)

def step3(data, next_callback):
    result = data - 5
    print(f"Step 3: {data} -> {result}")
    next_callback(result)

def final_step(result):
    print(f"Final result: {result}")

# Execute steps in sequence
def process_data(initial_data):
    step1(initial_data, lambda result1:
          step2(result1, lambda result2:
                step3(result2, final_step)))

# Start the process
process_data(5)
# Prints:
# "Step 1: 5 -> 15"
# "Step 2: 15 -> 30"
# "Step 3: 30 -> 25"
# "Final result: 25"
```

#### Parallel Callbacks

Execute multiple operations simultaneously and collect results:

```python
def parallel_tasks(tasks, final_callback):
    """Execute multiple tasks and collect their results."""
    results = [None] * len(tasks)
    completed = [0]  # Use a list for mutable closure
    
    def task_callback(index, result):
        results[index] = result
        completed[0] += 1
        if completed[0] == len(tasks):
            final_callback(results)
    
    for i, task in enumerate(tasks):
        task(lambda result, i=i: task_callback(i, result))

# Example tasks
def task_a(callback):
    result = "Task A completed"
    callback(result)

def task_b(callback):
    result = "Task B completed"
    callback(result)

def task_c(callback):
    result = "Task C completed"
    callback(result)

# Execute tasks in parallel
tasks = [task_a, task_b, task_c]
parallel_tasks(tasks, lambda results: print(f"All tasks completed: {results}"))
# Prints: "All tasks completed: ['Task A completed', 'Task B completed', 'Task C completed']"
```

### Exercise 3: Task Queue with Callbacks

Create a task queue system that:
1. Allows registering tasks with callbacks
2. Executes tasks one after another
3. Handles success and error cases
4. Provides progress updates

**Hint**: Use a list to store pending tasks and execute them sequentially.

## Module 3.4: Decorators and Function Wrappers

### Understanding Decorators

Decorators are a powerful feature in Python that allows you to modify the behavior of functions or classes. A decorator is a function that takes another function as input and returns a new function that usually extends or modifies the behavior of the input function.

#### Basic Decorator Syntax

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

# Call the decorated function
say_hello()
# Prints:
# "Something is happening before the function is called."
# "Hello!"
# "Something is happening after the function is called."
```

The `@my_decorator` syntax is equivalent to:

```python
def say_hello():
    print("Hello!")

say_hello = my_decorator(say_hello)
```

#### Decorators with Arguments

To handle decorated functions with arguments:

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before the function call")
        result = func(*args, **kwargs)
        print("After the function call")
        return result
    return wrapper

@my_decorator
def add(a, b):
    print(f"Adding {a} + {b}")
    return a + b

result = add(3, 5)
# Prints:
# "Before the function call"
# "Adding 3 + 5"
# "After the function call"
print(result)  # 8
```

#### Decorators with Parameters

You can also create decorators that accept parameters:

```python
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(n):
                results.append(func(*args, **kwargs))
            return results
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    return f"Hello, {name}!"

result = greet("Alice")
print(result)  # ["Hello, Alice!", "Hello, Alice!", "Hello, Alice!"]
```

### Practical Decorator Examples

#### Timing Decorator

```python
import time

def timing_decorator(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function {func.__name__} took {end_time - start_time:.4f} seconds to run")
        return result
    return wrapper

@timing_decorator
def slow_function():
    """A deliberately slow function."""
    time.sleep(1)
    return "Function completed"

result = slow_function()
# Prints: "Function slow_function took 1.0012 seconds to run"
```

#### Caching/Memoization Decorator

```python
def memoize(func):
    cache = {}
    def wrapper(*args):
        if args in cache:
            print(f"Cache hit for {args}")
            return cache[args]
        print(f"Cache miss for {args}")
        result = func(*args)
        cache[args] = result
        return result
    return wrapper

@memoize
def fibonacci(n):
    """Calculate the nth Fibonacci number."""
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(10))  # First time: cache misses
print(fibonacci(10))  # Second time: cache hit
```

#### Logging Decorator

```python
def log_function_call(func):
    def wrapper(*args, **kwargs):
        args_repr = [repr(a) for a in args]
        kwargs_repr = [f"{k}={v!r}" for k, v in kwargs.items()]
        signature = ", ".join(args_repr + kwargs_repr)
        print(f"Calling {func.__name__}({signature})")
        try:
            result = func(*args, **kwargs)
            print(f"{func.__name__} returned {result!r}")
            return result
        except Exception as e:
            print(f"{func.__name__} raised {type(e).__name__}: {e}")
            raise
    return wrapper

@log_function_call
def divide(a, b):
    return a / b

result1 = divide(10, 2)
# Prints:
# "Calling divide(10, 2)"
# "divide returned 5.0"

try:
    result2 = divide(5, 0)
except ZeroDivisionError:
    pass
# Prints:
# "Calling divide(5, 0)"
# "divide raised ZeroDivisionError: division by zero"
```

#### Authentication Decorator

```python
def require_authentication(func):
    def wrapper(user, *args, **kwargs):
        if not user.get("authenticated", False):
            return "Authentication required"
        return func(user, *args, **kwargs)
    return wrapper

@require_authentication
def get_secure_data(user):
    return f"Secure data for user: {user['name']}"

# Test with authenticated user
authenticated_user = {"name": "Alice", "authenticated": True}
print(get_secure_data(authenticated_user))  # "Secure data for user: Alice"

# Test with unauthenticated user
unauthenticated_user = {"name": "Bob", "authenticated": False}
print(get_secure_data(unauthenticated_user))  # "Authentication required"
```

### Preserving Function Metadata

When you use decorators, the original function's metadata (name, docstring, etc.) is lost. The `functools.wraps` decorator helps preserve this information:

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)  # Preserves metadata of the decorated function
    def wrapper(*args, **kwargs):
        """Wrapper function."""
        print("Before the function call")
        result = func(*args, **kwargs)
        print("After the function call")
        return result
    return wrapper

@my_decorator
def add(a, b):
    """Add two numbers and return the result."""
    return a + b

print(add.__name__)      # "add" (not "wrapper")
print(add.__doc__)       # "Add two numbers and return the result."
```

### Exercise 4: Custom Decorators

Create the following decorators:
1. A `retry` decorator that retries a function a specified number of times if it raises an exception
2. A `validate` decorator that validates function arguments based on provided criteria
3. A `rate_limit` decorator that limits how many times a function can be called in a given time period

**Hint**: Use closures to store state between calls.

## Project: Task Scheduler with Callbacks

Now, let's apply what we've learned about functional programming and callbacks to build a task scheduler application.

### Project Requirements

Create a task scheduler that:
1. Allows users to register tasks with priorities
2. Executes tasks based on their priority
3. Supports callbacks for task completion and errors
4. Provides task progress updates
5. Allows cancellation of pending tasks

### Task Structure

Each task should have:
- A name
- A function to execute
- Priority level
- Success callback
- Error callback
- Progress callback (optional)

### Implementation Guide

Here's a starting point for your implementation:

```python
class Task:
    def __init__(self, name, function, args=None, kwargs=None, priority=0,
                 on_success=None, on_error=None, on_progress=None):
        self.name = name
        self.function = function
        self.args = args or []
        self.kwargs = kwargs or {}
        self.priority = priority
        self.on_success = on_success
        self.on_error = on_error
        self.on_progress = on_progress
        self.status = "pending"  # pending, running, completed, failed, cancelled
    
    def __repr__(self):
        return f"Task({self.name}, priority={self.priority}, status={self.status})"

class TaskScheduler:
    def __init__(self):
        self.tasks = []
    
    def add_task(self, task):
        """Add a task to the scheduler."""
        self.tasks.append(task)
        # Sort tasks by priority (higher numbers = higher priority)
        self.tasks.sort(key=lambda t: t.priority, reverse=True)
        return task
    
    def cancel_task(self, task):
        """Cancel a pending task."""
        if task in self.tasks and task.status == "pending":
            task.status = "cancelled"
            self.tasks.remove(task)
            return True
        return False
    
    def run_next_task(self):
        """Run the next pending task with the highest priority."""
        for task in self.tasks:
            if task.status == "pending":
                self._execute_task(task)
                return task
        return None
    
    def run_all_tasks(self):
        """Run all pending tasks in priority order."""
        while self.has_pending_tasks():
            self.run_next_task()
    
    def has_pending_tasks(self):
        """Check if there are any pending tasks."""
        return any(task.status == "pending" for task in self.tasks)
    
    def _execute_task(self, task):
        """Execute a task with its callbacks."""
        task.status = "running"
        
        # Define a progress callback wrapper
        def progress_callback(percent):
            if task.on_progress:
                task.on_progress(task, percent)
        
        try:
            # Add progress_callback to kwargs if the function accepts it
            kwargs = task.kwargs.copy()
            if task.on_progress:
                kwargs["progress_callback"] = progress_callback
            
            # Execute the task function
            result = task.function(*task.args, **kwargs)
            
            # Call success callback if provided
            if task.on_success:
                task.on_success(task, result)
            
            task.status = "completed"
            self.tasks.remove(task)
            return result
        except Exception as e:
            # Call error callback if provided
            if task.on_error:
                task.on_error(task, e)
            
            task.status = "failed"
            self.tasks.remove(task)
            return None
```

### Extensions

Once you have the basic implementation working, consider adding:
- Task dependencies (a task can depend on other tasks)
- Periodic tasks that run on a schedule
- Task timeouts (cancel tasks that run too long)
- Asynchronous task execution using threads or asyncio
- Task priorities that change dynamically based on waiting time

This project will test your understanding of:
- Functions as first-class objects
- Callbacks and event-driven programming
- Higher-order functions
- Closures and state management

Take your time to design the system before you start coding, and remember that good functional programming emphasizes smaller, composable functions.

Good luck!