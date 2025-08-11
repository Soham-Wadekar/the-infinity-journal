# ⚙️ Concurrency in Python

> _"Concurrency is not parallelism, but it enables parallelism."_ — Rob Pike

Concurrency is the ability of a program to make progress on multiple tasks at once. In Python, it allows you to manage multiple operations that may otherwise block the flow of execution — such as file I/O, user input, or network requests.

## 📚 Contents

- [Threading](#threading)

While Python's Global Interpreter Lock (GIL) limits true multithreaded CPU-bound execution, concurrency remains a powerful tool for writing efficient and responsive programs — especially in I/O-heavy or real-time applications.

---

## Threading

A thread is like a mini program inside the main program. Multiple threads can be used concurrently to do different things at the same time.

### Defining a Thread

To create a thread, you use the `Thread` class from the `threading` module.

```python
thread = Thread(target=function_name, args=())

thread.start()
thread.join()           # Wait for the thread to finish
```

### Lock

A conflict between threads can occur if they try to modify the same variable, which can mess each other up (overwriting may occur) - called **race condition**. A `Lock` prevents this, by letting only one thread pass through at a time.

A `Lock` is defined as follows:

```python
lock = Lock()       # Imported from threading

def function():
    ...
    with lock:
        ...

```

Sometimes, by luck, you can get a correct answer. In this case, you might think that the code runs fine, but it can be dangerous. So, always use locks in such cases.

One interesting analogy I came across while understanding locks:
> Driving without a seatbelt feels fine — until you crash

### ThreadPoolExecutor

It is a higher-level tool, that can run multiple functions in parallel, submit many jobs at once, manages threads automatically.

Another smart analogy:
> Suppose you have 10 tasks at your company. Instead of hiring 10 employees and training them yourself, you go to a temp agency (`ThreadPoolExecutor`) and tell them, "Here are 10 tasks, I need 3 workers, you handle it."

```python
import time
from concurrent.futures import ThreadPoolExecutor

def square(n):
    time.sleep(2)
    print(f"Done with {n}")
    return n * n

start = time.time()

with ThreadPoolExecutor(max_workers=5) as executor:
    results = executor.map(square, range(10))

end = time.time()

print("Total time:", end - start, "seconds")

```

A few important functions used with `ThreadPoolExecutor` are:

- `as_completed()` - Returns tasks as they finish. Tasks don’t wait for slower ones before processing faster ones
- `.shutdown(wait=False)` - Exits early without waiting for threads to finish

### Subclassing

You can create a custom thread using the `Thread` class

```python
class CustomThread(Thread):
    ...
    def run(self):
        ...

```

### Daemon Threads

Daemon threads are background threads that don’t prevent the program from exiting. If only daemon threads remain, Python will terminate—even if they’re still running.

Used for logging, monitoring, etc.

```python
from threading import Thread

t1 = Thread(target=func(), daemon=True)
```

### Threading Tools

- **Barrier:** A synchronization point where a fixed number of threads arrive, then proceed together. _Three friends are hiking. They go ahead once all of them are at the meeting point_
- **Condition:** Threads wait for a certain condition to be fulfilled. _You eat food ONLY AFTER the waiter tells you that the food is ready_
- **Event:** Threads wait until an event is set, then all threads go.  _Cars wait for the signal to turn green_
- **Semaphore:** A counter that controls how many threads can access a resource. _There are three toilet stalls, so only three people can access it at the same time. Others have to wait_
- **Timer:** A thread that runs after a certain delay. _A kitchen timer_
