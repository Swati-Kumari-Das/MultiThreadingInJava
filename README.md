# MultiThreadingInJava

# ⚙️ CPU, Processes, Threads, Multitasking & Multithreading

## 🧠 CPU (Central Processing Unit)
- Known as the **brain of the computer**.  
- Executes instructions from programs.  
- Performs arithmetic, logic, control, and I/O operations.  

👉 Example: **Intel Core i7**, **AMD Ryzen 7**.

---

## 🧩 Core
- A **core** is an individual processing unit within a CPU.  
- Multiple cores allow CPUs to run tasks in **parallel**.  

---

## 📜 Program
- A **program** is a set of instructions written in a programming language.  
- Tells the computer how to perform a specific task.  

👉 Example: **Microsoft Word** is a program for creating and editing documents.  

---

## ⚙️ Process
- A **process** is an **instance of a program** being executed.  
- When a program runs, the OS creates a process to manage execution.  

👉 Example: Opening Microsoft Word creates a **Word process**.  

---

## 🧵 Thread
- A **thread** is the **smallest unit of execution** within a process.  
- Threads share process resources but run independently.  

👉 Example: Google Chrome may use a separate thread for each browser tab.  

---

## 🖥️ Multitasking
- **Ability of OS to run multiple processes at the same time.**  
- On single-core CPUs: achieved via **time-sharing** (rapid switching).  
- On multi-core CPUs: true **parallel execution** occurs.  
- Managed by the **OS scheduler**.  

👉 Example: Browsing the internet + listening to music + downloading a file.  

---

## 🔀 Multithreading
- **Concurrent execution of multiple threads within a single process.**  
- Makes programs more efficient and responsive.  

👉 Example: A web browser has threads for rendering pages, running JavaScript, and handling user inputs.  

---

## 🕒 Time Slicing
- **Divides CPU time into small intervals (time slices).**  
- The OS assigns these slices to processes/threads.  
- Prevents one process from monopolizing the CPU.  

---

## 🔄 Context Switching
- **Saving the state of one process/thread** and **loading another**.  
- Enables multitasking on a single-core CPU.  
- On multi-core CPUs, it helps optimize parallelism.  

---

## 🔑 Difference: Multitasking vs Multithreading

| Aspect             | Multitasking (Processes)                     | Multithreading (Threads)                |
|--------------------|-----------------------------------------------|-----------------------------------------|
| Unit of Execution  | Process                                       | Thread (within a process)               |
| Resource Sharing   | Independent memory & resources                | Threads share memory & resources        |
| Scope              | Multiple applications                        | Multiple tasks within one application   |
| Example            | Running Word + Chrome + Spotify              | Chrome → threads for tabs, JS, inputs   |

---

## 🏢 Analogy
- **Multitasking**: Office manager (OS) assigns employees (processes) to different projects (applications). Each works independently.  
- **Multithreading**: Within a single project (application), a team (process) of employees (threads) work together on different parts, sharing resources.  

---

## ✅ Summary
- **CPU** = Brain of the computer.  
- **Core** = Individual worker inside CPU.  
- **Program** = Set of instructions.  
- **Process** = Running instance of a program.  
- **Thread** = Smallest execution unit inside a process.  
- **Multitasking** = Multiple processes running.  
- **Multithreading** = Multiple threads within one process.  
- **Time slicing & Context switching** = OS techniques to achieve concurrency.

  # Multithreading in Java

## What is Multithreading?
- In Java, **multithreading** is the concurrent execution of two or more threads to maximize the utilization of the CPU.  
- A **thread** is the smallest unit of processing (a lightweight process).  
- Java provides multithreading support through:
  - `java.lang.Thread` class  
  - `java.lang.Runnable` interface  

---

## Main Thread
- When a Java program starts, one thread begins running immediately → called the **main thread**.  
- This main thread is responsible for executing the `main` method of the program.  

---

## Multithreading in Single-core vs Multi-core
- **Single-core environment**  
  Threads are managed by the JVM + OS using **time-slicing** to give the illusion of concurrency.  

- **Multi-core environment**  
  JVM can distribute threads across multiple cores, enabling **true parallel execution**.  

---

## Creating Threads in Java

There are **two ways** to create a new thread in Java:  

1. **Extending the Thread class**  
2. **Implementing the Runnable interface**

---

### 1. Extending the `Thread` class

Steps:
1. Create a new class that **extends `Thread`**.  
2. Override the **`run()`** method to define the task.  
3. Create an object of the new class and call **`start()`** to initiate the thread.  

```java
// Example: Extending Thread class
class WorldThread extends Thread {
    @Override
    public void run() {
        System.out.println("Hello from WorldThread!");
    }
}

public class ThreadExample1 {
    public static void main(String[] args) {
        WorldThread t1 = new WorldThread(); // create thread object
        t1.start(); // start new thread
        System.out.println("Hello from Main Thread!");
    }
}

```
2. Implementing the Runnable interface
Steps:

Create a new class that implements Runnable.

Override the run() method to define the task.

Pass the runnable object to a Thread object and call start().


```java
// Example: Implementing Runnable interface
class WorldRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Hello from WorldRunnable!");
    }
}

public class ThreadExample2 {
    public static void main(String[] args) {
        WorldRunnable worldTask = new WorldRunnable();   // create runnable object
        Thread t2 = new Thread(worldTask);              // pass runnable to Thread
        t2.start();                                     // start new thread
        System.out.println("Hello from Main Thread!");
    }
}
```
## Key Difference

| Extending Thread Class | Implementing Runnable Interface |
|-------------------------|---------------------------------|
| Subclass `Thread` directly. | Create a class that implements `Runnable`. |
| Override `run()` method. | Override `run()` method. |
| Object itself is a thread. | `Runnable` is passed into a `Thread` object. |
| Less flexible (cannot extend another class). | More flexible (can implement multiple interfaces). |

---

## Summary

- **Thread** = lightweight unit of execution.  
- **Main Thread** runs the program initially.  
- Two ways to create threads → **Extend `Thread`** OR **Implement `Runnable`**.  
- **Runnable is preferred** in real-world applications because it allows better flexibility (Java supports only single inheritance).  


# Java Thread Lifecycle

In Java, threads go through multiple states during their execution.  
The **Thread Lifecycle** defines these states and transitions.

---

## Thread States

1. **NEW**  
   - A thread is in this state when it is created but not yet started.  
   - Example: `Thread t = new Thread();`

2. **RUNNABLE**  
   - After the `start()` method is called, the thread becomes runnable.  
   - The thread is ready to run and is waiting for CPU time.  

3. **RUNNING**  
   - The thread is actively executing.  
   - Only one thread per CPU core can be in the Running state at a time.

4. **WAITING / BLOCKED / TIMED_WAITING**  
   - **WAITING** → Thread is waiting indefinitely for another thread to signal.  
   - **TIMED_WAITING** → Thread is waiting for a specific time (e.g., `Thread.sleep(2000)`).  
   - **BLOCKED** → Thread is waiting to acquire a monitor lock.  

5. **TERMINATED**  
   - The thread has finished execution.  

---

## Thread Lifecycle Diagram

NEW ---> RUNNABLE ---> RUNNING ---> TERMINATED
^ | |
| v v
| WAITING TIMED_WAITING
|__________ BLOCKED



---

## Example 1: NEW → RUNNABLE → RUNNING

```java
public class Test {
    public static void main(String[] args) {
        World ti = new World(); // NEW
        ti.start(); // RUNNABLE
        System.out.println(Thread.currentThread().getName());

        for (;;) {
            System.out.println("Hello");
        }
    }
}

class World extends Thread {
    @Override
    public void run() {
        for (;;) {
            System.out.println("World");
        }
    }
}
Example 2: Checking Thread States
java
Copy code
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("RUNNING");
        try {
            Thread.sleep(2000); // moves to TIMED_WAITING
        } catch (InterruptedException e) {
            System.out.println(e);
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) throws InterruptedException {
        MyThread t1 = new MyThread();

        System.out.println(t1.getState()); // NEW

        t1.start();
        System.out.println(t1.getState()); // RUNNABLE

        Thread.sleep(100);
        System.out.println(t1.getState()); // TIMED_WAITING (sleeping)

        t1.join();
        System.out.println(t1.getState()); // TERMINATED
    }
}
```
# Summary of Thread States in Java

## Thread States

| **State**        | **Description**                                      |
|-------------------|------------------------------------------------------|
| **NEW**           | Thread is created but not started.                   |
| **RUNNABLE**      | Thread is ready, waiting for CPU scheduling.         |
| **RUNNING**       | Thread is executing on the CPU.                      |
| **WAITING**       | Thread is waiting indefinitely for another thread.   |
| **TIMED_WAITING** | Thread is waiting for a specific time (`sleep()`).   |
| **BLOCKED**       | Thread is waiting for a resource (monitor lock).     |
| **TERMINATED**    | Thread has finished execution.                       |

---

## Key Points

- **Main Thread**:  
  When a Java program starts, one thread (main thread) runs immediately.

- **Thread Creation**:  
  Threads can be created in two ways:  
  1. Extending the **Thread** class  
  2. Implementing the **Runnable** interface  

- **State Transition**:  
  Threads move between states automatically, managed by the **JVM** and the **Operating System (OS)**.


  # Thread vs Runnable in Java

## 1. Using **Thread Class**
- Create a class that **extends Thread**.
- Override the `run()` method.
- Create an object of that class and call `start()`.

### Example:
```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running...");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // starts new thread
    }
}
```
2. Using Runnable Interface
Create a class that implements Runnable.

Override the run() method.

Pass the object to a Thread and call start().

Example:
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable is running...");
    }
}

public class Main {
    public static void main(String[] args) {
        Runnable r = new MyRunnable();
        Thread t1 = new Thread(r);
        t1.start(); // starts new thread
    }
}
```
# Thread vs Runnable in Java

## Key Differences

| **Extending Thread Class**               | **Implementing Runnable Interface**        |
|------------------------------------------|---------------------------------------------|
| Subclass `Thread` directly.              | Create a class that implements `Runnable`. |
| Override `run()` method.                 | Override `run()` method.                   |
| Object itself is a thread.               | `Runnable` is passed into a `Thread`.      |
| Less flexible (cannot extend another class). | More flexible (can implement multiple interfaces). |

---

## Summary
- **Thread** = Lightweight unit of execution in Java.  
- **Main Thread** = The first thread that runs in any Java program.  
- Two ways to create threads → **Extend Thread** OR **Implement Runnable**.  
- **Runnable is preferred** in real-world applications because:
  - Java supports only **single inheritance**.  
  - Runnable allows a class to **extend another class** while still enabling multithreading.
 
  # Java Thread Methods and Daemon Threads

## 📌 Overview
In Java, threads provide a way to achieve **multithreading** — the concurrent execution of two or more parts of a program.  
The `Thread` class provides several useful methods to control and manage thread execution.

---

## 📝 Thread Methods (Quick Reference)

| Method                  | Description |
|--------------------------|-------------|
| `start()`               | Starts a new thread and calls the `run()` method internally. |
| `run()`                 | Defines the code executed by the thread. |
| `sleep(ms)`             | Pauses execution of the thread for given milliseconds. |
| `join()`                | Waits for a thread to finish execution. |
| `getName()` / `setName()` | Gets or sets the thread name. |
| `getPriority()` / `setPriority()` | Gets or sets thread priority (`1–10`). |
| `isAlive()`             | Checks if a thread is still running. |
| `setDaemon(true)`       | Marks a thread as **Daemon (background thread)**. |
| `isDaemon()`            | Checks if a thread is daemon or not. |

---

## 1️⃣ `start()`

Starts a new thread.  
Without `start()`, the `run()` method will just run like a normal method in the main thread.

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }
}

public class Test {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // creates a new thread
    }
}
```
2️⃣ run()
Contains the code that runs in the thread.
Calling run() directly won’t start a new thread.
```java 
class MyThread extends Thread {
    public void run() {
        System.out.println("Running in thread: " + Thread.currentThread().getName());
    }
}

public class Test {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.run();   // runs in main thread
        t1.start(); // runs in a separate thread
    }
}

```
3️⃣ sleep(ms)
Pauses the thread for given milliseconds.

```java
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 3; i++) {
            System.out.println("Thread running: " + i);
            try {
                Thread.sleep(1000); // pause for 1 second
            } catch (InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}

public class Test {
    public static void main(String[] args) {
        new MyThread().start();
    }
}

```
4️⃣ join()
Makes one thread wait until another finishes.


```java
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Child Thread: " + i);
        }
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        MyThread t1 = new MyThread();
        t1.start();
        t1.join(); // main waits for t1
        System.out.println("Main thread finished after child");
    }
}

```

5️⃣ getName() and setName()
Used to get or set the thread name.

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running: " + Thread.currentThread().getName());
    }
}

public class Test {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.setName("Worker-1");
        t1.start();

        System.out.println("Main: " + Thread.currentThread().getName());
    }
}

```
6️⃣ getPriority() and setPriority()
Priority ranges from 1 (MIN_PRIORITY) to 10 (MAX_PRIORITY).
Default = 5 (NORM_PRIORITY).

```java
class MyThread extends Thread {
    public void run() {
        System.out.println(getName() + " Priority: " + getPriority());
    }
}

public class Test {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.setPriority(Thread.MAX_PRIORITY);
        t1.start();
    }
}

```
7️⃣ isAlive()
Checks if a thread is still running.

```java
class MyThread extends Thread {
    public void run() {
        try { Thread.sleep(500); } catch (Exception e) {}
        System.out.println("Thread finished");
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        MyThread t1 = new MyThread();
        t1.start();
        System.out.println("Alive? " + t1.isAlive());
        t1.join();
        System.out.println("Alive after join? " + t1.isAlive());
    }
}
``` 
🌀 Daemon Threads
What is a Daemon Thread?
Background thread that provides services to user threads.

JVM exits automatically when only daemon threads are running.

Example: Garbage Collector.

Example

```java
class MyDaemon extends Thread {
    public void run() {
        while (true) {
            System.out.println("Daemon running...");
            try { Thread.sleep(1000); } catch (Exception e) {}
        }
    }
}

public class Test {
    public static void main(String[] args) {
        MyDaemon d = new MyDaemon();
        d.setDaemon(true); // must set before start()
        d.start();
        
        System.out.println("Main thread finished...");
        // JVM will exit even if daemon is still running
    }
}

```
🔑 Key Points
User threads → Main tasks of application.

Daemon threads → Background tasks.

JVM terminates once all user threads finish, regardless of daemon threads.

# 🔒 Synchronization in Java

## 📌 What is Synchronization?
- In multithreading, multiple threads may try to access **shared resources** (like variables, objects, or files) at the same time.  
- **Synchronization** is the process of controlling access to these shared resources to avoid data inconsistency.  
- Java provides the `synchronized` keyword to achieve synchronization.

---

## ⚡ The Problem: Race Condition
- A **race condition** happens when two or more threads try to access and modify a shared resource at the same time.  
- This can lead to **unexpected results**.

### Example Without Synchronization (Race Condition)

```java
class Counter {
    int count = 0;

    public void increment() {
        count++; // critical section
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        Counter c = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) c.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) c.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Final Count: " + c.count); 
        // ❌ Output may be less than 2000 due to race condition
    }
}

```
🏷️ Critical Section
The part of the code that accesses shared resources is called the critical section.

In the above example, count++ is a critical section.

✅ Solution: Mutual Exclusion
To solve race conditions, we need mutual exclusion:
Only one thread can access the critical section at a time.

In Java, this is achieved using the synchronized keyword.

🔑 Using synchronized Method

```java
class Counter {
    int count = 0;

    public synchronized void increment() {
        count++; // only one thread at a time can enter here
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        Counter c = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) c.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) c.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Final Count: " + c.count); 
        // ✅ Always 2000 now
    }
}
``` 
🔒 Using synchronized Block
Instead of synchronizing the whole method, we can synchronize only the critical section (better performance).

```java 
class Counter {
    int count = 0;

    public void increment() {
        synchronized (this) {
            count++;
        }
    }
}
```
🔑 Key Concepts Recap
Race Condition → Multiple threads modify shared data → inconsistent results.

Critical Section → Code that accesses shared resources (needs protection).

Mutual Exclusion → Only one thread at a time executes critical section.

synchronized → Ensures mutual exclusion.

✅ Summary
Use synchronized to prevent race conditions.

Synchronization can be applied at:

Method level → public synchronized void method()

Block level → synchronized(object) { ... }

Guarantees data consistency, but adds overhead (slower).


# 🔐 Locks in Java

## 📌 Why Do We Need Locks?
- `synchronized` works, but it has **limitations**:
  1. **Less control** → Once a thread enters a synchronized block, others must wait (you can’t interrupt or timeout).
  2. **Deadlocks** are harder to handle.
  3. No way to check if a lock is available without blocking.
  4. Always tied to the intrinsic lock of an object.

➡️ To overcome these issues, Java introduced the **`Lock` interface** in `java.util.concurrent.locks`.

---

## 🏷️ Types of Locks

### 1. Intrinsic Locks (Monitor Locks)
- Built into every Java object.
- Used implicitly when you use `synchronized`.
- Simple but less flexible.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```
2. Explicit Locks
Provided in java.util.concurrent.locks.

You explicitly control them with lock() and unlock().

Example: ReentrantLock.

⚡ Problem with Synchronization Example

```java
class SharedResource {
    private int count = 0;

    public void increment() {
        // ❌ Not synchronized → race condition
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        SharedResource s = new SharedResource();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) s.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) s.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Final Count: " + s.getCount()); 
        // ❌ May be < 2000 due to race condition
    }
}
```
✅ Solution Using ReentrantLock
```java
import java.util.concurrent.locks.ReentrantLock;

class SharedResource {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();

    public void increment() {
        lock.lock();   // acquire lock
        try {
            count++;
        } finally {
            lock.unlock(); // release lock (always in finally!)
        }
    }

    public int getCount() {
        return count;
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        SharedResource s = new SharedResource();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) s.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) s.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Final Count: " + s.getCount()); 
        // ✅ Always 2000 now
    }
}
```
🔄 How the Flow Goes Between Threads
Thread A calls lock.lock() → acquires the lock.

Thread B calls lock.lock() → must wait until A releases the lock.

Thread A executes critical section, then calls unlock().

Thread B gets the lock and continues.

🔑 Features of ReentrantLock
1. Reentrant
A thread holding the lock can acquire it again without deadlocking itself.

```java
lock.lock();
lock.lock(); // same thread can lock again
try {
    // critical section
} finally {
    lock.unlock();
    lock.unlock(); // must unlock twice
}
```
2. tryLock()
Attempts to acquire the lock without blocking.

```java

if (lock.tryLock()) {
    try {
        System.out.println("Lock acquired");
    } finally {
        lock.unlock();
    }
} else {
    System.out.println("Lock not available, skipping work");
}
```
3. tryLock with Timeout
Prevents deadlock by waiting only a limited time.

```java

if (lock.tryLock(2, TimeUnit.SECONDS)) {
    try {
        System.out.println("Work done safely");
    } finally {
        lock.unlock();
    }
} else {
    System.out.println("Could not get lock, avoiding deadlock");
}
```
4. Interruptible Locking
You can interrupt a thread waiting for a lock.

```java

try {
    lock.lockInterruptibly();
    try {
        System.out.println("Got the lock!");
    } finally {
        lock.unlock();
    }
} catch (InterruptedException e) {
    System.out.println("Thread interrupted while waiting for lock");
}
```
📋 Summary
Feature	synchronized (Intrinsic Lock)	ReentrantLock (Explicit Lock)
Simplicity	Easy to use	More code (lock/unlock)
Reentrancy	✅ Yes	✅ Yes
Try acquiring lock	❌ No	✅ tryLock()
Timeout on lock	❌ No	✅ Yes
Interrupt while waiting	❌ No	✅ Yes
Deadlock prevention tools	Limited	✅ Advanced

✅ Final Takeaway
Use synchronized for simple cases.

Use ReentrantLock when you need:

Timeout,

Non-blocking tryLock,

Interruptible waits,

More flexible deadlock handling.
