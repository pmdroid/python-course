# Part 5: Capstone Project - To-Do Application

## Introduction

Congratulations on reaching the final part of our practical Python course! It's time to put everything you've learned into practice by building a complete To-Do application. This capstone project will integrate all three core concepts: classes and methods, callbacks, and asynchronous programming with asyncio.

Throughout this section, we'll provide guidance and ideas rather than complete implementations. The goal is for you to think through the design and implementation challenges yourself, applying what you've learned in real-world context.

## Module 5.1: Requirements and Planning

### Project Overview

Your To-Do application should have these features:
- Create, view, update, and delete tasks
- Organize tasks by category and priority
- Set due dates for tasks and track completion status
- Store tasks persistently using file I/O
- Provide notifications for upcoming due dates
- Offer a simple command-line interface

### Technical Requirements

Your implementation must demonstrate these core concepts:

**Classes and Methods:**
- Design a clean object-oriented structure
- Use appropriate encapsulation and inheritance
- Apply proper class relationships

**Callbacks:**
- Implement an event system using callbacks
- Use callbacks for notifications and updates
- Create a decoupled, event-driven architecture

**Asyncio:**
- Use asyncio for file operations
- Implement background notification checking
- Handle user input without blocking

### Planning Your Application

Before writing any code, think about:

1. **Data Model**: What classes do you need? What properties and methods should they have?
2. **Component Interaction**: How will different parts of your application communicate?
3. **User Experience**: What commands will users enter? How will information be displayed?
4. **Persistence**: How will you store and load task data?
5. **Error Handling**: How will you validate input and handle errors?

### Exercise 1: Application Design

Before you start coding:

1. Sketch the application's structure
2. List the main classes and their responsibilities
3. Define the relationships between classes
4. Draft simple user stories for core functionality
5. Create a development plan with milestones

**Hint**: Start with a simple design and plan to add features incrementally.

## Module 5.2: Architecture and Design

### Class Structure

Consider these core classes for your application:

1. **Task Class**
    - Properties: title, description, due date, priority, category, completion status
    - Methods: check if due soon, mark as complete, convert to/from dictionary

2. **TaskManager Class**
    - Responsibilities: store tasks, add/update/delete tasks, filter tasks
    - Callback integration: notify when tasks change

3. **Storage Class**
    - Responsibilities: save tasks to file, load tasks from file
    - Asyncio integration: perform I/O operations asynchronously

4. **Notifier Class**
    - Responsibilities: check for due tasks, send notifications
    - Callback integration: call registered notification handlers
    - Asyncio integration: run checks in the background

5. **CommandLineInterface Class**
    - Responsibilities: parse commands, display results, handle user interaction
    - Asyncio integration: handle commands without blocking notifications

### Communication Architecture

Think about how these components should communicate:

1. **Event-Based Communication**:
    - TaskManager emits events when tasks are added, updated, or deleted
    - Other components subscribe to these events

2. **Direct Method Calls**:
    - CLI calls methods on TaskManager to manipulate tasks
    - TaskManager calls methods on Storage to save changes

3. **Callback Registration**:
    - Components register callbacks with the Notifier
    - Notifier calls these callbacks when notifications occur

### Data Format

Design a simple data format for storing tasks. Consider using JSON for easy serialization and deserialization. For example:

```json
{
  "tasks": [
    {
      "id": "task1",
      "title": "Complete project",
      "description": "Finish the to-do app",
      "created_at": "2023-10-15T14:30:00",
      "due_date": "2023-10-20T17:00:00",
      "priority": "high",
      "category": "work",
      "completed": false
    }
  ]
}
```

### Exercise 2: Component Design

For each main component of your application:

1. List the properties and methods it needs
2. Identify where callbacks should be used
3. Note which operations should be asynchronous
4. Sketch how it interacts with other components

**Hint**: Focus on designing clean interfaces between components.

## Module 5.3: Implementation

Now it's time to implement your To-Do application. We'll provide some guidance for each component, but you should write the actual code yourself.

### Task and TaskManager Implementation

Start with your core data model:

1. **Implement the Task class**:
    - Include properties for id, title, description, due date, etc.
    - Add methods for checking if a task is due soon
    - Include serialization methods (to_dict, from_dict)

2. **Implement the TaskManager class**:
    - Store tasks in a dictionary (id -> task)
    - Add methods for adding, updating, and completing tasks
    - Include callback lists for task events
    - Add filtering methods (by category, priority, completion)

### Storage Implementation

For persisting your tasks:

1. **Design an async Storage class**:
    - Methods for saving to and loading from a file
    - Use asyncio to make file operations non-blocking
    - Handle errors gracefully

2. **Considerations**:
    - Use `async with` for file operations
    - Run blocking I/O in thread pools with `loop.run_in_executor()`
    - Include proper error handling and recovery

### Notification System

For alerting users about upcoming tasks:

1. **Create a Notifier class**:
    - Takes a TaskManager as input
    - Periodically checks for tasks due soon
    - Calls registered callbacks when notifications occur

2. **Implementation details**:
    - Use `asyncio.create_task()` for background checking
    - Use a callback list for notification handlers
    - Allow configuration of check frequency

### Command Line Interface

For user interaction:

1. **Build a CLI class**:
    - Parse user commands
    - Execute appropriate actions
    - Display results and notifications

2. **Command handling**:
    - Implement commands for: add, list, complete, update, delete
    - Support filtering in list command
    - Use a command loop with asyncio

### Main Application

Tie everything together:

1. **Create a main application class**:
    - Initialize all components
    - Connect events and callbacks
    - Start the command loop and background tasks

2. **Entry point**:
    - Set up the application
    - Handle clean shutdown

### Exercise 3: Incremental Implementation

Implement your application in these stages:

1. Basic task management (add, list, complete)
2. Persistent storage with asyncio
3. Task filtering and updates
4. Notifications for upcoming tasks
5. Additional features of your choice

**Hint**: Test each component individually before integrating.

## Module 5.4: Testing and Refinement

### Testing Your Application

Test your application to ensure it works correctly:

1. **Unit testing**:
    - Test Task class methods
    - Test TaskManager filtering and updates
    - Test command parsing

2. **Integration testing**:
    - Test storage operations
    - Test notification system
    - Test command execution

3. **Manual testing**:
    - Try adding, updating, and completing tasks
    - Check notifications for due tasks
    - Verify data persists between runs

### Refinement Ideas

Consider these enhancements:

1. **Better user experience**:
    - Colorful output
    - Interactive menus
    - Help documentation

2. **Additional features**:
    - Recurring tasks
    - Subtasks
    - Priority auto-adjustment
    - Task search
    - Data export

3. **Technical improvements**:
    - More efficient storage
    - Better error handling
    - Configuration options

### Exercise 4: Enhancement and Optimization

Choose at least two enhancements to implement:

1. A feature enhancement that improves functionality
2. A technical enhancement that improves code quality
3. A user experience enhancement
4. A new integration (e.g., email notifications)

**Hint**: Focus on enhancements that exercise the core concepts (classes, callbacks, asyncio).

## Project: Final To-Do Application

### Building Your Complete Application

Now it's time to put everything together into a polished application:

1. **Refine your code**:
    - Ensure consistent style
    - Add docstrings and comments
    - Clean up any rough edges

2. **Create a proper package**:
    - Organize files into modules
    - Add a setup.py for installation
    - Include a README with usage instructions

3. **Final testing**:
    - Perform a complete functionality check
    - Test error cases
    - Verify all requirements are met

### Project Requirements Checklist

Make sure your final application:

- [  ] Uses classes appropriately for data modeling
- [  ] Implements callbacks for events and notifications
- [  ] Utilizes asyncio for non-blocking operations
- [  ] Persists data between runs
- [  ] Provides a user-friendly interface
- [  ] Handles errors gracefully
- [  ] Is well-documented and maintainable

### Sample Project Structure

Consider organizing your project like this:

```
todo_app/
├── __init__.py
├── models/
│   ├── __init__.py
│   ├── task.py
│   └── category.py
├── managers/
│   ├── __init__.py
│   └── task_manager.py
├── storage/
│   ├── __init__.py
│   └── file_storage.py
├── ui/
│   ├── __init__.py
│   └── cli.py
├── utils/
│   ├── __init__.py
│   └── notifier.py
└── main.py
```

### Conclusion

Congratulations on completing this capstone project! By building this To-Do application, you've demonstrated your mastery of important Python concepts:

- Object-oriented programming with classes and methods
- Event-driven programming with callbacks
- Asynchronous programming with asyncio

These skills will serve you well in many real-world Python applications. Keep exploring, keep building, and keep improving your skills!

Remember that programming is a journey of continuous learning. Each project teaches you something new and helps you grow as a developer.

Happy coding!