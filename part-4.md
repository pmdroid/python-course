# Part 4: Introduction to Asynchronous Programming

## Introduction

Welcome to Part 4 of our practical Python course! In this section, we'll explore asynchronous programming in Python using the `asyncio` library. Asynchronous programming is a powerful technique that allows your code to perform multiple operations concurrently without using multiple threads or processes.

By the end of this part, you'll understand how to write code that efficiently handles I/O-bound operations like network requests, file operations, and other tasks that involve waiting. This is a crucial skill for developing responsive applications that can handle many operations simultaneously.

## Module 4.1: Understanding Synchronous vs Asynchronous

### The Problem with Synchronous Code

In traditional synchronous programming, operations are executed one after another. When one operation is running, the program waits for it to complete before moving to the next operation:

```python
def read_file(filename):
    print(f"Reading {filename}...")
    # This operation blocks until complete
    with open(filename, 'r') as file:
        content = file.read()
    print(f"Finished reading {filename}")
    return content

def main():
    start_time = time.time()
    
    # Each operation blocks the next one
    content1 = read_file('file1.txt')  # Program waits here
    content2 = read_file('file2.txt')  # Then waits here
    content3 = read_file('file3.txt')  # Then waits here
    
    elapsed = time.time() - start_time
    print(f"Total time: {elapsed:.2f} seconds")

import time
main()
```

If each file operation takes 1 second, the total time will be at least 3 seconds. This is inefficient, especially for I/O operations where most of the time is spent waiting.

### The Asynchronous Approach

In asynchronous programming, when an operation would normally block (like waiting for a file to read or a network request to complete), the program can switch to doing other work instead:

```python
import asyncio

async def read_file(filename):
    print(f"Starting to read {filename}...")
    # Simulate file reading with a non-blocking sleep
    await asyncio.sleep(1)  # This doesn't block other operations
    print(f"Finished reading {filename}")
    return f"Content of {filename}"

async def main():
    start_time = time.time()
    
    # Create tasks for all operations
    task1 = asyncio.create_task(read_file('file1.txt'))
    task2 = asyncio.create_task(read_file('file2.txt'))
    task3 = asyncio.create_task(read_file('file3.txt'))
    
    # Await all tasks to complete
    content1 = await task1
    content2 = await task2
    content3 = await task3
    
    elapsed = time.time() - start_time
    print(f"Total time: {elapsed:.2f} seconds")

import time
asyncio.run(main())
```

In this example, all three file reading operations happen concurrently, so the total time is closer to 1 second (the duration of a single operation) rather than 3 seconds.

### Key Concepts in Asynchronous Programming

#### 1. Non-blocking I/O

Asynchronous programming is most beneficial for I/O-bound operations, where your code spends a lot of time waiting for external operations like:
- Network requests
- File operations
- Database queries
- User input

#### 2. Coroutines

In Python, asynchronous functions are called coroutines and are defined using `async def`:

```python
async def my_coroutine():
    # Asynchronous code here
    pass
```

Coroutines are special functions that can be paused and resumed. They use the `await` keyword to yield control back to the event loop when waiting for an operation to complete.

#### 3. The Event Loop

The event loop is the core of every asyncio application. It:
- Manages and distributes the execution of different tasks
- Handles I/O events
- Executes callbacks
- Schedules tasks

```python
import asyncio

async def main():
    # Your async code here
    print("Hello, asyncio!")

# Get the event loop and run the main coroutine
asyncio.run(main())  # This handles the event loop for you
```

#### 4. Tasks

Tasks are used to schedule coroutines concurrently:

```python
async def main():
    # Create a task
    task = asyncio.create_task(some_coroutine())
    
    # Do other work...
    
    # Wait for the task to complete
    await task

async def some_coroutine():
    await asyncio.sleep(1)
    return "Done!"
```

### When to Use Asynchronous Programming

Asynchronous programming is most beneficial when:
- You have many I/O-bound operations
- Operations can be performed concurrently
- Responsiveness is important

It's less useful for CPU-bound tasks, where multiprocessing might be a better choice.

### Exercise 1: Simulating Concurrent Operations

Create a program that simulates downloading multiple files asynchronously:
1. Create an async function `download_file(file_name, size)` that simulates downloading a file of a given size
2. Use `asyncio.sleep()` to simulate the download time (1 second per MB)
3. Run downloads for multiple files concurrently
4. Compare the total time with a synchronous approach

**Hint**: Use `asyncio.gather()` to run multiple coroutines concurrently.

## Module 4.2: Python's asyncio Library

### The asyncio Module

Python's `asyncio` library provides a complete framework for writing single-threaded concurrent code, including:
- An event loop for running asynchronous tasks
- Coroutine support
- Synchronization primitives
- Network I/O operations
- Various utilities for working with concurrency

Let's explore the core components of asyncio.

### Running Coroutines with asyncio.run()

The simplest way to run a coroutine is with `asyncio.run()`:

```python
import asyncio

async def hello_world():
    print("Hello")
    await asyncio.sleep(1)
    print("World")

# Run the coroutine
asyncio.run(hello_world())
```

`asyncio.run()` creates a new event loop, runs the coroutine until it completes, and then closes the loop.

### Creating and Awaiting Tasks

Tasks are used to schedule coroutines to run concurrently:

```python
import asyncio

async def say_after(delay, message):
    await asyncio.sleep(delay)
    print(message)

async def main():
    # Create tasks
    task1 = asyncio.create_task(say_after(1, "Hello"))
    task2 = asyncio.create_task(say_after(2, "World"))
    
    print("Started tasks")
    
    # Wait for tasks to complete
    await task1
    await task2
    
    print("Tasks completed")

asyncio.run(main())
```

### Running Tasks Concurrently with asyncio.gather()

`asyncio.gather()` runs multiple coroutines concurrently and waits for all of them to complete:

```python
import asyncio

async def fetch_data(name, delay):
    print(f"Fetching {name}...")
    await asyncio.sleep(delay)  # Simulate network delay
    print(f"Fetched {name}")
    return f"{name} result"

async def main():
    # Run all coroutines concurrently and gather results
    results = await asyncio.gather(
        fetch_data("users", 1),
        fetch_data("products", 2),
        fetch_data("orders", 1.5)
    )
    
    print(f"Results: {results}")

asyncio.run(main())
```

In this example, all three fetch operations run concurrently. The total time will be approximately 2 seconds (the duration of the longest operation).

### Timeouts with asyncio.wait_for()

Sometimes you need to set a timeout for an operation. `asyncio.wait_for()` allows you to specify a maximum wait time:

```python
import asyncio

async def long_operation():
    print("Starting long operation...")
    await asyncio.sleep(5)  # Simulate a long operation
    print("Long operation completed")
    return "Success"

async def main():
    try:
        # Wait for the operation with a timeout
        result = await asyncio.wait_for(long_operation(), timeout=2)
        print(f"Result: {result}")
    except asyncio.TimeoutError:
        print("Operation timed out")

asyncio.run(main())
```

This will print "Operation timed out" after 2 seconds, without waiting for the full 5 seconds.

### Handling Multiple Tasks with Different Completion Times

`asyncio.as_completed()` lets you process tasks as they complete, rather than waiting for all to finish:

```python
import asyncio
import random

async def random_delay(name):
    delay = random.uniform(0.5, 3)
    print(f"{name} starting (will take {delay:.2f}s)")
    await asyncio.sleep(delay)
    print(f"{name} completed")
    return (name, delay)

async def main():
    # Create a list of coroutines
    coroutines = [random_delay(f"Task {i}") for i in range(5)]
    
    # Process tasks as they complete
    for future in asyncio.as_completed(coroutines):
        name, delay = await future
        print(f"{name} result processed (took {delay:.2f}s)")

asyncio.run(main())
```

### Using asyncio with Context Managers

Asyncio provides asynchronous context managers with `async with`:

```python
import asyncio

class AsyncResource:
    async def __aenter__(self):
        print("Acquiring resource")
        await asyncio.sleep(1)  # Simulate resource acquisition
        print("Resource acquired")
        return self
    
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        print("Releasing resource")
        await asyncio.sleep(0.5)  # Simulate resource release
        print("Resource released")
    
    async def use_resource(self):
        print("Using resource")
        await asyncio.sleep(2)
        print("Done using resource")

async def main():
    # Use the async context manager
    async with AsyncResource() as resource:
        await resource.use_resource()

asyncio.run(main())
```

### Exercise 2: Async Web Crawler

Create a simple asynchronous web crawler that:
1. Takes a list of URLs
2. Fetches them concurrently
3. Extracts and returns the page titles
4. Implements a timeout for each request

**Hint**: Use the `aiohttp` library for async HTTP requests. You can simulate this with `asyncio.sleep()` if you prefer.

## Module 4.3: Async/Await Syntax

### The async/await Keywords

Python's async/await syntax provides a clean way to write asynchronous code:

- `async def` declares a coroutine function
- `await` pauses execution until the awaited coroutine completes

```python
async def main():
    print("Start")
    await asyncio.sleep(1)  # Pause execution for 1 second
    print("Middle")
    await asyncio.sleep(1)  # Pause again
    print("End")
```

### Awaitable Objects

You can `await` any "awaitable" object, which includes:
- Coroutines created with `async def`
- Tasks created with `asyncio.create_task()`
- Objects that implement the `__await__()` method

```python
async def coroutine_function():
    return "result"

async def main():
    # Await a coroutine
    result1 = await coroutine_function()
    
    # Await a task
    task = asyncio.create_task(coroutine_function())
    result2 = await task
    
    print(f"Results: {result1}, {result2}")
```

### Async For Loops

For iterating over asynchronous iterators, use `async for`:

```python
class AsyncCounter:
    def __init__(self, limit):
        self.limit = limit
        self.counter = 0
    
    def __aiter__(self):
        return self
    
    async def __anext__(self):
        if self.counter < self.limit:
            self.counter += 1
            await asyncio.sleep(0.1)  # Simulate async work
            return self.counter
        else:
            raise StopAsyncIteration

async def main():
    async for number in AsyncCounter(5):
        print(f"Got number: {number}")

asyncio.run(main())
```

### Async Comprehensions

Python supports async versions of list, set, and dict comprehensions:

```python
async def get_data(item):
    await asyncio.sleep(0.1)  # Simulate async work
    return item * 2

async def main():
    # Async list comprehension
    data = [1, 2, 3, 4, 5]
    results = [await get_data(item) for item in data]  # This is NOT concurrent
    print(f"Sequential results: {results}")
    
    # For concurrent execution, use asyncio.gather with a regular comprehension
    concurrent_results = await asyncio.gather(*(get_data(item) for item in data))
    print(f"Concurrent results: {concurrent_results}")
    
    # Async comprehension (requires Python 3.6+)
    async_results = [result async for result in AsyncCounter(5)]
    print(f"Async comprehension results: {async_results}")

asyncio.run(main())
```

### Rules for Using await

There are some important rules for using `await`:
1. You can only use `await` inside an `async def` function
2. You can't use `await` at the top level of your Python script (before Python 3.10)
3. You can only `await` an awaitable object

```python
# This is WRONG - await outside async function
# await asyncio.sleep(1)  

# This is WRONG - awaiting a non-awaitable
# async def wrong():
#     await 5  

# This is CORRECT
async def correct():
    await asyncio.sleep(1)
```

### Async Generators

Async generators allow you to produce a sequence of values asynchronously:

```python
async def async_range(start, stop):
    for i in range(start, stop):
        await asyncio.sleep(0.1)  # Simulate async work
        yield i

async def main():
    async for i in async_range(0, 5):
        print(i)

asyncio.run(main())
```

### Exercise 3: Async Data Processor

Create an async data processor that:
1. Takes a list of data items
2. Processes each item asynchronously with a given function
3. Supports different processing stages (like a pipeline)
4. Returns the final results

**Hint**: Use async generators to create a processing pipeline.

## Module 4.4: Practical Async Patterns

### Error Handling in Async Code

Error handling in asynchronous code is similar to synchronous code, but there are some important considerations:

```python
async def might_fail(succeed=True):
    await asyncio.sleep(1)
    if not succeed:
        raise ValueError("Operation failed")
    return "Success"

async def main():
    # Basic try/except
    try:
        result = await might_fail(succeed=False)
        print(f"Result: {result}")
    except ValueError as e:
        print(f"Caught error: {e}")
    
    # Handling errors in gathered tasks
    tasks = [
        might_fail(succeed=True),
        might_fail(succeed=False),
        might_fail(succeed=True)
    ]
    
    # Method 1: Let gather() raise the first error
    try:
        results = await asyncio.gather(*tasks)
        print(f"All succeeded: {results}")
    except ValueError as e:
        print(f"One task failed: {e}")
    
    # Method 2: Return exceptions instead of raising
    results = await asyncio.gather(*tasks, return_exceptions=True)
    for i, result in enumerate(results):
        if isinstance(result, Exception):
            print(f"Task {i} failed: {result}")
        else:
            print(f"Task {i} succeeded: {result}")

asyncio.run(main())
```

### Cancellation

Tasks can be cancelled while they're running:

```python
async def long_running_task():
    try:
        print("Task started")
        for i in range(10):
            await asyncio.sleep(0.5)
            print(f"Step {i+1}")
    except asyncio.CancelledError:
        print("Task was cancelled")
        raise  # Re-raise to properly mark as cancelled
    finally:
        print("Task cleanup")

async def main():
    # Start the task
    task = asyncio.create_task(long_running_task())
    
    # Let it run for a bit
    await asyncio.sleep(2.5)
    
    # Cancel the task
    print("Cancelling task...")
    task.cancel()
    
    try:
        await task
    except asyncio.CancelledError:
        print("Main: task was cancelled")

asyncio.run(main())
```

### Semaphores for Limiting Concurrency

When working with many concurrent tasks, you might want to limit how many run at once:

```python
async def worker(name, semaphore):
    async with semaphore:
        print(f"Worker {name} acquired the semaphore")
        await asyncio.sleep(1)  # Simulate work
        print(f"Worker {name} releasing the semaphore")
    return f"Worker {name} done"

async def main():
    # Limit to 3 concurrent tasks
    semaphore = asyncio.Semaphore(3)
    
    # Create 10 worker tasks
    tasks = [worker(i, semaphore) for i in range(10)]
    
    # Run all tasks
    results = await asyncio.gather(*tasks)
    print(f"All workers completed")

asyncio.run(main())
```

### Queues for Task Distribution

Asyncio queues are useful for distributing work among consumers:

```python
async def producer(queue, items):
    """Put items into the queue."""
    for item in items:
        await queue.put(item)
        print(f"Produced: {item}")
        await asyncio.sleep(0.5)  # Simulate production time
    
    # Signal that production is done
    await queue.put(None)

async def consumer(queue, name):
    """Consume items from the queue."""
    while True:
        item = await queue.get()
        if item is None:
            # Pass the signal to the next consumer
            await queue.put(None)
            break
        
        print(f"Consumer {name} got: {item}")
        await asyncio.sleep(1)  # Simulate processing time
        queue.task_done()
    
    print(f"Consumer {name} done")

async def main():
    # Create a queue
    queue = asyncio.Queue()
    
    # Create producer and consumers
    producer_task = asyncio.create_task(
        producer(queue, range(10))
    )
    
    consumer_tasks = [
        asyncio.create_task(consumer(queue, i))
        for i in range(3)
    ]
    
    # Wait for producer to finish
    await producer_task
    
    # Wait for all consumers to finish
    await asyncio.gather(*consumer_tasks)
    
    print("All done")

asyncio.run(main())
```

### Handling Background Tasks

Sometimes you need tasks that run in the background without explicitly waiting for them:

```python
async def background_task(name):
    """A task that runs in the background."""
    try:
        while True:
            print(f"Background task {name} is running")
            await asyncio.sleep(1)
    except asyncio.CancelledError:
        print(f"Background task {name} was cancelled")
        raise

async def main():
    # Start a background task
    task = asyncio.create_task(background_task("monitor"))
    
    # Do some other work
    print("Main task is doing work")
    await asyncio.sleep(3)
    
    # Cancel the background task before exiting
    print("Cancelling background task")
    task.cancel()
    try:
        await task
    except asyncio.CancelledError:
        pass
    
    print("Main task done")

asyncio.run(main())
```

### Combining Async and Sync Code

Sometimes you need to call synchronous code from asynchronous code, or vice versa:

```python
import asyncio
import time
import concurrent.futures

def cpu_bound_work(n):
    """Simulate CPU-bound work (synchronous)."""
    print(f"CPU work started with {n}")
    # Simulate CPU work with a calculation
    result = sum(i * i for i in range(n))
    print(f"CPU work finished")
    return result

async def io_bound_work(n):
    """Simulate IO-bound work (asynchronous)."""
    print(f"IO work started with {n}")
    await asyncio.sleep(1)  # Simulate IO operation
    print(f"IO work finished")
    return n * 2

async def main():
    # Option 1: Run CPU-bound work in a thread pool
    print("Running CPU work in a thread pool:")
    with concurrent.futures.ThreadPoolExecutor() as pool:
        result = await asyncio.get_event_loop().run_in_executor(
            pool, cpu_bound_work, 10_000_000
        )
        print(f"CPU result: {result}")
    
    # Option 2: Run CPU-bound work in a process pool (better for CPU-bound)
    print("\nRunning CPU work in a process pool:")
    with concurrent.futures.ProcessPoolExecutor() as pool:
        result = await asyncio.get_event_loop().run_in_executor(
            pool, cpu_bound_work, 10_000_000
        )
        print(f"CPU result: {result}")
    
    # Option 3: Mix async and sync work
    print("\nMixing async and sync work:")
    async_result = await io_bound_work(5)
    sync_result = cpu_bound_work(5_000_000)  # This blocks the event loop!
    print(f"Results: async={async_result}, sync={sync_result}")

asyncio.run(main())
```

### Exercise 4: Async File Processor

Create an async file processing system that:
1. Watches a directory for new files
2. Processes each file asynchronously when detected
3. Limits the number of concurrent file operations
4. Provides progress updates

**Hint**: Use a combination of semaphores, queues, and background tasks.

## Project: Asynchronous Web Scraper

Now, let's apply what we've learned about asynchronous programming to build a web scraper that can efficiently fetch and process web pages.

### Project Requirements

Create a web scraper that:
1. Takes a starting URL and crawling depth
2. Asynchronously fetches pages and extracts links
3. Follows links up to the specified depth
4. Extracts and saves relevant information from pages
5. Handles errors gracefully
6. Respects rate limits for websites
7. Provides progress updates

### Implementation Guide

Here's a starting point for your implementation:

```python
import asyncio
import aiohttp
import re
from urllib.parse import urljoin, urlparse
from bs4 import BeautifulSoup

class AsyncWebScraper:
    def __init__(self, max_concurrency=5, rate_limit=1):
        """Initialize the scraper with limits."""
        self.max_concurrency = max_concurrency  # Max concurrent requests
        self.rate_limit = rate_limit  # Seconds between requests to the same domain
        self.visited_urls = set()
        self.domain_last_accessed = {}
        self.semaphore = None
        self.session = None
    
    async def __aenter__(self):
        """Set up the scraper when used as a context manager."""
        self.semaphore = asyncio.Semaphore(self.max_concurrency)
        self.session = aiohttp.ClientSession()
        return self
    
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        """Clean up resources."""
        await self.session.close()
    
    async def fetch_url(self, url):
        """Fetch a URL with rate limiting."""
        # Extract domain for rate limiting
        domain = urlparse(url).netloc
        
        # Apply rate limiting
        if domain in self.domain_last_accessed:
            last_accessed = self.domain_last_accessed[domain]
            now = asyncio.get_event_loop().time()
            if now - last_accessed < self.rate_limit:
                delay = self.rate_limit - (now - last_accessed)
                await asyncio.sleep(delay)
        
        # Update last accessed time
        self.domain_last_accessed[domain] = asyncio.get_event_loop().time()
        
        try:
            async with self.semaphore:
                print(f"Fetching: {url}")
                async with self.session.get(url, timeout=10) as response:
                    if response.status == 200:
                        self.visited_urls.add(url)
                        return await response.text()
                    else:
                        print(f"Error {response.status} for {url}")
                        return None
        except Exception as e:
            print(f"Exception while fetching {url}: {e}")
            return None
    
    def extract_links(self, html, base_url):
        """Extract links from HTML content."""
        if not html:
            return []
        
        soup = BeautifulSoup(html, 'html.parser')
        links = []
        
        for a_tag in soup.find_all('a', href=True):
            href = a_tag['href']
            absolute_url = urljoin(base_url, href)
            
            # Filter URLs (same domain, http/https only, etc.)
            if self._should_follow(absolute_url, base_url):
                links.append(absolute_url)
        
        return links
    
    def _should_follow(self, url, base_url):
        """Determine if a URL should be followed."""
        # Skip already visited URLs
        if url in self.visited_urls:
            return False
        
        # Parse URLs
        parsed_url = urlparse(url)
        parsed_base = urlparse(base_url)
        
        # Check scheme
        if parsed_url.scheme not in ('http', 'https'):
            return False
        
        # Check if same domain (optional, remove for wider crawling)
        if parsed_url.netloc != parsed_base.netloc:
            return False
        
        # Skip common non-content URLs
        # Add your own patterns to this list
        skip_patterns = [
            r'\.(jpg|jpeg|png|gif|pdf|zip|mp3|mp4)$',
            r'javascript:',
            r'mailto:',
            r'#'
        ]
        
        for pattern in skip_patterns:
            if re.search(pattern, url, re.IGNORECASE):
                return False
        
        return True
    
    def extract_data(self, html, url):
        """Extract data from the HTML (customize this method)."""
        if not html:
            return None
        
        soup = BeautifulSoup(html, 'html.parser')
        data = {
            'url': url,
            'title': soup.title.text.strip() if soup.title else 'No title',
            # Add more data extraction as needed
        }
        
        return data
    
    async def crawl(self, start_url, max_depth=2):
        """Crawl from a starting URL up to a maximum depth."""
        results = []
        
        async def _crawl_recursive(url, depth):
            if depth > max_depth:
                return
            
            html = await self.fetch_url(url)
            if not html:
                return
            
            # Extract data from the page
            data = self.extract_data(html, url)
            if data:
                results.append(data)
            
            if depth < max_depth:
                # Extract links and follow them
                links = self.extract_links(html, url)
                tasks = [_crawl_recursive(link, depth + 1) for link in links]
                await asyncio.gather(*tasks)
        
        await _crawl_recursive(start_url, 1)
        return results

# Example usage
async def main():
    start_url = "https://example.com"
    
    async with AsyncWebScraper(max_concurrency=3, rate_limit=1) as scraper:
        results = await scraper.crawl(start_url, max_depth=2)
        
        print(f"\nCrawling complete! Found {len(results)} pages:")
        for i, result in enumerate(results[:5], 1):
            print(f"{i}. {result['title']} - {result['url']}")
        
        if len(results) > 5:
            print(f"...and {len(results) - 5} more")

if __name__ == "__main__":
    asyncio.run(main())
```

### Extensions

Once you have the basic implementation working, consider adding:
- Saving results to a database or file
- Adding command-line interface options
- Implementing robots.txt parsing and respecting
- Adding proxy support
- Creating a more sophisticated rate limiting system
- Adding data visualization for the crawled pages

### Notes on Dependencies

This project requires these external libraries:
- `aiohttp` for async HTTP requests
- `beautifulsoup4` for HTML parsing

You can install them with:
```
pip install aiohttp beautifulsoup4
```

This project will test your understanding of:
- Asynchronous programming with asyncio
- Handling concurrent operations
- Rate limiting and resource management
- Error handling in asynchronous code

Good luck with your implementation!