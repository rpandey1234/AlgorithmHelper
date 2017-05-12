---
layout: post
title:  Concurrency
author: Christian Griset
date:   2017-02-12
categories: threads concurrency
---

Concurrency is a fundamental tool for maximizing performance and CPU power. Concurrency is a mechanism which deserializes
programming instructions and allows multiple threads to be run simultaneously on the same processing core. We'll clarify what
all this means below, but for illustration imagine a grocery store. Shoppers can be thought of having a set of instructions, e.g.
"get milk, eggs, kombucha". Grocery stores would be extremely inefficient if they allowed only one shopper in at a time, and
instead have a concurrent process for many shoppers. This example may sound trivial, but consider that much code is written
serially.

Contents
===========

- [Threads and Processors](#threads-and-processors)
- [Parallelism and Concurrency](#parallelism-and-concurrency)
- [Difficulties with Concurrency](#difficulties-with-concurrency)
- [Deadlock and Livelock](#deadlock-and-livelock)
- [Solving for Deadlock and Livelock](#solving-for-deadlock-and-livelock)

### Threads and Processors

Before we talk about concurrency, let's establish what we mean by threads and processors.

A **thread** is a set of computer instructions. Imagine the following code:

```
func Job() {
  x = 1
  y = 2
  z = x + y
  print z
}
```

When this function is called, it's translated as a thread into computer code, e.g.

```
Thread:
  Do: Assign x = 1
  Do: Assign y = 2
  Do: Assign z = x + y
  Do: Print z
```

The function defines a set of instructions, and a thread is the execution of the set of instructions on a processor.

A **processor** is a machine, e.g. a CPU, which executes machine instructions. Multi-processor cores, which are very common
now a day, enable a device to run multiple processors, and therefore multiple threads, simultaneously.

### Parallelism and Concurrency

**Parallelism** is the procedure where threads are distributed across multiple processes. For illustration, imagine
we'd like to call Job above multiple times. We distribute as follows:

```
Processor1 - Processor2 - Processor3
Do Job       Do Job        Do Job
```

**Concurrency** allows multiple threads to be intertwined on the *same* processor. Machine instructions don't happen 
instantly, and often commands take time to compute. Concurrency deserializes threads to allow multiple sets of
instructions to occur.

Imagine the following functions:

```
func LongJob1() {
   a = 1
   sleep(1 second)
   b = 2
   sleep(1 second)
   c = a + b
   sleep(1 second)
   print c
}

func LongJob2() {
   x = 3
   sleep(1 second)
   y = 4
   sleep(1 second)
   z = x + y
   sleep(1 second)
   print z
}
```

Often you'll see code structured as follows:

```
func main() {
  Do LongJob1()
  Do LongJob2()
  
  // Do other stuff
}
```

This whole process takes 6 seconds because LongJob2 is blocked by LongJob1, i.e. the commands are serialized. The processor
executes the following instructions

```
Do Assign a = 1
Do Wait 1 Second
Do Assign b = 2
...
Do Assign z = x + y
Do Wait 1 Second
Do Print z
```

Concurrency will combine the instructions of both threads to more efficient run. The code may look as follows:

```
func main() {
  Concurrently Do LongJob1()
  Concurrently Do LongJob2()
  Wait() // Wait for both jobs to complete before moving on
  
  // Do other stuff
}
```

The thread now may look as follows:

```
Do Assign a=1, x=3
Do Sleep 1 Second, Sleep 1 Second
Do Assign b=2, y=4
...
Do Print c, Print z
```

This allows our main function to run in 3 seconds instead of 6. Imagine if we had 100 similar jobs like this to run, our
main would then take 300 seconds to run serially but still 3 seconds concurrently.

(Note: The instructions don't necessarily happen instantly as suggested above, but can rather be in very quick succession.
But in effect, the program can feel as if instructions are executed simultaneously).

### Difficulties with Concurrency

Concurrency is powerful, however this power doesn't come for free. Concurrency in a complex codebase isn't always easy
to reason about and concurrency related bugs can be difficult to track. 

Perhaps the most common pitfalls around concurrency involve accessing shared memory. In particular, multiple jobs might need to
access and modify a variable shared between them, (e.g. graphs, arrays and counter variables). Primary known problems
around shared memory are classified as follows:

- Race Conditions
- Deadlock
- Livelock

#### Race Conditions and Locks

Race conditions occur when multiple threads modify a shared resource without appropriate coordination. Imagine the following
code.

```
func Increment(x) {
  x = x + 1
}

func main() {
  x = 0
  Concurrently Do Increment(x)
  Concurrently Do Increment(x)
  Wait()
  
  Print x
}
```

What do you think the result will be? Both methods are trying to access the same patch of memory (the variable x) and increment it.
If they access it simultaneously, both processes register an initial value of x=0 and increment it to x=1. This type of 
problem is called a **race condition**.

The solution is to use a **lock**. Locks are objects which limit the access to a piece of memory. Without getting
too deeply into the variety of locks, one of the common types of locks is called a **mutex**. Mutexes establish 
the rule that only the thread which locks a piece of memory can unlock it.

We can remove the race condition within our code by modifying it as follows:

```
func Increment(x, mutex) {
  mutex.Lock(x)
  x = x + 1
  mutex.Unlock(x)
}

func main() {
  x = 0
  mutex = New Mutex
  Concurrently Do Increment(x, mutex)
  Concurrently Do Increment(x, mutex)
  Wait()
  
  Print x
}
```

Now we're assured the program will print 2.

### Deadlock and Livelock

Locks help prevent race conditions, but if not used carefully your process might be blocked indefinitely.

**Deadlock** occurs when a process can't complete because it locks a resource necessary for some other process to
complete, and vice versa.

To exhibit deadlock imagine the following code where we increment two variables, x and y:

```
func IncrementXThenY(x, y, mutex) {
  mutex.Lock(x)
  x = x + 1
  mutex.Lock(y)
  y = y + 1
  mutex.Unlock(x)
  mutex.Unlock(y)
}

func IncrementYThenX(y, x, mutex) {
  mutex.Lock(y)
  y = y + 1
  mutex.Lock(x)
  x = x + 1
  mutex.Unlock(y)
  mutex.Unlock(x)
}

func main() {
  x = 0
  y = 0
  mutex = New Mutex
  Concurrently Do IncrementXThenY(x, y, mutex)
  Concurrently Do IncrementYThenX(y, x, mutex)
  Wait()
  
  Print x
  Print y
}
```

This program will never complete, can you see why?

The first function locks x, does some work, then attempts to access and lock y. However the second function first locks
y, does some work, then attempts to access and lock x. Both functions are simultaneously locking variables the other function 
needs to complete, causing both functions to wait forever for the other to release their resources.

**Livelock** occurs when functions keep on locking and unlocking resources while making no forward progress in their
work. Ironically, livelock often occurs when trying to solve for deadlock. Here's some code exhibiting livelock:

```
func IncrementXThenY(x, y, mutex) {
  mutex.Lock(x)
  if mutex.IsLocked(y) { // If y is currently locked, give up the lock on x and try again
    mutex.Unlock(x)
    return IncrementXThenY(x, y, mutex)
  }
  x = x + 1
  mutex.Lock(y)
  y = y + 1
  mutex.Unlock(x)
  mutex.Unlock(y)
}

func IncrementYThenX(y, x, mutex) {
  mutex.Lock(y)
  if mutex.IsLocked(x) { // If x is currently locked, give up the lock on y and try again
    mutex.Unlock(y)
    return IncrementYThenX(y, x, mutex)
  }
  y = y + 1
  mutex.Lock(x)
  x = x + 1
  mutex.Unlock(y)
  mutex.Unlock(x)
}

func main() {
  x = 0
  y = 0
  mutex = New Mutex
  Concurrently Do IncrementXThenY(x, y, mutex)
  Concurrently Do IncrementYThenX(y, x, mutex)
  Wait()
  
  Print x
  Print y
}
```

Do you see the livelock? The first function locks x and attempts to lock y. If it can't lock y, it releases x and tries again,
where it immediately locks x again. Vice versa for the second function. Thus the functions are continuously locking and 
unlocking resources without making any progress.

### Solving for Deadlock and Livelock

Deadlock and livelock can be solved robustly with a **lock hierarchy**. A lock hierarchy establishes a fixed ordering which
resources should be locked. In each example above, if we agreed that x should always be attempted to be locked before y,
no process would ever block the other. This solution scales to any number of resources and concurrent processes. The downside
of this solution is in complex code bases, establishing and maintaining a hierarchy can be difficult, especially as it grows.

Another solution is to always release locks after some randomized time. You lock a resource x and if the lock is held for
too long, then the lock is released and you try again (potentially after some small wait time). This isn't always a desirable 
solution because it can slow down the process unnecessarily. Further the user needs to maintain what a good lock/waiting times.



