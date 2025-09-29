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


# 🔐 Locks in Java (Extended with `lockInterruptibly`)

## 📌 Why `lockInterruptibly`?
- Sometimes a thread may **wait indefinitely** to acquire a lock.
- Using `lockInterruptibly()`, the thread can be **interrupted while waiting**.
- This prevents situations where a thread gets stuck forever.

---

## ⚡ Example Without Interruptibility (Thread stuck)

```java
import java.util.concurrent.locks.ReentrantLock;

class SharedResource {
    private final ReentrantLock lock = new ReentrantLock();

    public void longTask() {
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + " got the lock, working...");
            Thread.sleep(5000); // simulates long work
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        SharedResource resource = new SharedResource();

        Thread t1 = new Thread(resource::longTask, "T1");
        Thread t2 = new Thread(resource::longTask, "T2");

        t1.start();
        Thread.sleep(100); // ensure T1 acquires the lock first
        t2.start();

        // ❌ If we try to interrupt t2, it will still wait for lock forever
        t2.interrupt();
    }
}
```
➡️ Here, even though t2.interrupt() is called, t2 stays blocked until T1 releases the lock.

✅ Example With lockInterruptibly()
```java

import java.util.concurrent.locks.ReentrantLock;

class SharedResource {
    private final ReentrantLock lock = new ReentrantLock();

    public void longTask() {
        try {
            lock.lockInterruptibly(); // waits, but can be interrupted
            try {
                System.out.println(Thread.currentThread().getName() + " got the lock, working...");
                Thread.sleep(5000);
            } finally {
                lock.unlock();
            }
        } catch (InterruptedException e) {
            System.out.println(Thread.currentThread().getName() + " was interrupted while waiting!");
        }
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        SharedResource resource = new SharedResource();

        Thread t1 = new Thread(resource::longTask, "T1");
        Thread t2 = new Thread(resource::longTask, "T2");

        t1.start();
        Thread.sleep(100); // ensure T1 acquires the lock
        t2.start();

        Thread.sleep(1000); 
        t2.interrupt(); // ✅ t2 exits gracefully instead of waiting forever
    }
}
```
📝 Output (sample)

T1 got the lock, working...
T2 was interrupted while waiting!

🔑 Key Notes on lockInterruptibly()
Use when you don’t want threads stuck forever.

Thread can react to interrupts while waiting.

Must be wrapped in try-catch for InterruptedException.

Great for systems where responsiveness matters (e.g., servers, UI threads).

# 📋 Comparison Recap of Lock Methods in Java

| Method                | Behavior |
|------------------------|----------|
| `lock()`              | Blocks until lock is acquired (ignores interrupt). |
| `tryLock()`           | Tries immediately (or with timeout). Non-blocking option. |
| `lockInterruptibly()` | Blocks until lock is acquired **but responds to interrupts**. |

# ⚖️ Fair vs Unfair Locking in Java

## 🔑 What is Fairness in Locks?
- **Fair Lock**: Threads acquire the lock **in the order they requested it** (FIFO).
- **Unfair Lock (Default)**: Threads may "jump the queue" if the lock happens to be free when they request it.
  - This can improve performance but may cause **starvation** for some threads.

---

## 🟢 Example: Fair Lock
```java
import java.util.concurrent.locks.ReentrantLock;

class FairExample {
    private final ReentrantLock lock = new ReentrantLock(true); // fair = true

    public void work() {
        try {
            lock.lock();
            System.out.println(Thread.currentThread().getName() + " acquired the lock");
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        FairExample obj = new FairExample();
        Runnable task = obj::work;

        for (int i = 1; i <= 5; i++) {
            new Thread(task, "Thread-" + i).start();
        }
    }
}
```
✅ Output (Fair Lock, FIFO Order)

Thread-1 acquired the lock
Thread-2 acquired the lock
Thread-3 acquired the lock
Thread-4 acquired the lock
Thread-5 acquired the lock
🔴 Example: Unfair Lock (Default)

```java
import java.util.concurrent.locks.ReentrantLock;

class UnfairExample {
    private final ReentrantLock lock = new ReentrantLock(); // unfair by default

    public void work() {
        try {
            lock.lock();
            System.out.println(Thread.currentThread().getName() + " acquired the lock");
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        UnfairExample obj = new UnfairExample();
        Runnable task = obj::work;

        for (int i = 1; i <= 5; i++) {
            new Thread(task, "Thread-" + i).start();
        }
    }
}
```
⚠️ Output (Unfair Lock, No Guaranteed Order)
Thread-3 acquired the lock
Thread-1 acquired the lock
Thread-4 acquired the lock
Thread-2 acquired the lock
Thread-5 acquired the lock

📌 Summary
Fair Lock → Ensures FIFO ordering, avoids starvation.

Unfair Lock (default) → Faster (better throughput), but some threads may starve.

Choose based on use case:

Fair → Banking systems, critical ordering.

Unfair → High-performance apps where strict ordering isn’t required.

# ReadWriteLock (Deep Dive)

`ReadWriteLock` (most commonly `ReentrantReadWriteLock`) is a synchronization aid that distinguishes **read access** from **write access**.  
It allows **multiple threads to read** a shared resource concurrently while **only one thread** may write at a time. This is ideal for **read-heavy** workloads.

---

## 🔑 Core Ideas

- **Read Lock** (`readLock()`):
  - Shared lock. Multiple readers can hold it **simultaneously** (if no writer holds the write lock).
  - Good for operations that only observe state (no mutation).
- **Write Lock** (`writeLock()`):
  - Exclusive lock. Only one writer can hold it, and no readers are allowed while it is held.
  - Used for operations that modify state.
- **Reentrant**: the same thread can reacquire read/write locks it already holds (subject to rules).
- **Fairness**: `ReentrantReadWriteLock` can be constructed `new ReentrantReadWriteLock(true)` to enforce FIFO ordering; default is unfair (higher throughput).

**Use if:** your application is read-heavy (many reads, few writes) — e.g., caches, configuration access, analytics reads.

**Don’t use if:** writes are frequent (then read/write lock offers little benefit vs. a simple exclusive lock); or if the data structure has very small critical sections where lock overhead dominates.

---

## ✅ Memory & Visibility
Acquiring/releasing read/write locks establishes **happens-before** relationships (like `synchronized`), so changes made under a write lock become visible to later readers.

---

## ⚠️ Important Pitfalls & Rules

- **Writer starvation**: in unfair mode, readers can continuously acquire the read lock and starve writers. Use fair mode or strategies (timeouts, tryLock) when needed.
- **Lock upgrade = dangerous**: attempting to *upgrade* from read lock → write lock (i.e., holding a read lock and then acquiring the write lock) **can deadlock**. Release the read lock first before acquiring the write lock.
- **Lock downgrade = allowed**: acquiring the write lock, then acquiring the read lock, then releasing the write lock (downgrade) is safe and supported.
- **Use finally** to always `unlock()`.

---

## Typical API (ReentrantReadWriteLock)
```java
ReentrantReadWriteLock lock = new ReentrantReadWriteLock();
Lock readLock  = lock.readLock();
Lock writeLock = lock.writeLock();

readLock.lock();      // acquire shared read access
readLock.unlock();

writeLock.lock();     // acquire exclusive write access
writeLock.unlock();

writeLock.tryLock(timeout, TimeUnit.MILLISECONDS);  // try with timeout
writeLock.lockInterruptibly();                     // interruptible wait
```
Example 1 — Simple Readers / Single Writer
This example demonstrates multiple reader threads reading concurrently, while writer threads run exclusively.

```java

import java.util.concurrent.locks.ReentrantReadWriteLock;
import java.util.concurrent.locks.Lock;

class SharedData {
    private final ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
    private final Lock r = rwLock.readLock();
    private final Lock w = rwLock.writeLock();

    private int value = 0;

    // Read method (many threads can read concurrently)
    public int read() {
        r.lock();
        try {
            // simulate read delay
            Thread.sleep(100);
            return value;
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            return -1;
        } finally {
            r.unlock();
        }
    }

    // Write method (exclusive)
    public void write(int newValue) {
        w.lock();
        try {
            // simulate write delay
            Thread.sleep(200);
            value = newValue;
            System.out.println(Thread.currentThread().getName() + " wrote " + newValue);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            w.unlock();
        }
    }
}

public class ReadWriteExample {
    public static void main(String[] args) {
        SharedData sd = new SharedData();

        // start reader threads
        for (int i = 0; i < 5; i++) {
            new Thread(() -> {
                for (int j = 0; j < 5; j++) {
                    int v = sd.read();
                    System.out.println(Thread.currentThread().getName() + " read " + v);
                }
            }, "Reader-" + i).start();
        }

        // start writer threads
        for (int i = 0; i < 2; i++) {
            final int val = i + 100;
            new Thread(() -> {
                for (int j = 0; j < 3; j++) {
                    sd.write(val + j);
                }
            }, "Writer-" + i).start();
        }
    }
}
```
What you’ll observe: multiple Reader-* threads will often print their reads simultaneously (concurrent reads). Writer-* messages appear exclusively, and while a writer is writing readers are blocked.

Example 2 — Downgrading vs Unsafe Upgrading
Downgrade (safe): acquire write lock → update → acquire read lock → release write lock → continue reading.

Unsafe Upgrade: holding a read lock and then trying to acquire write lock may deadlock (other readers exist).

```java

import java.util.concurrent.locks.ReentrantReadWriteLock;
import java.util.concurrent.locks.Lock;

class DataWithDowngrade {
    private final ReentrantReadWriteLock rw = new ReentrantReadWriteLock();
    private final Lock r = rw.readLock();
    private final Lock w = rw.writeLock();
    private int data = 0;

    // Unsafe upgrade (DON'T DO)
    public void unsafeUpgrade() throws InterruptedException {
        r.lock();
        try {
            // read something
            if (data < 0) {
                // try to upgrade - this may deadlock if other readers exist
                w.lock(); // BAD: holding readLock while trying to get writeLock
                try {
                    data = 42;
                } finally {
                    w.unlock();
                }
            }
        } finally {
            r.unlock();
        }
    }

    // Safe downgrade
    public void safeWriteThenRead() {
        w.lock();
        try {
            data++;
            // now acquire read lock before releasing write lock to safely continue with read
            r.lock();
        } finally {
            w.unlock();  // still hold read lock
        }

        try {
            // now read while holding read lock
            System.out.println("After write, read = " + data);
        } finally {
            r.unlock();
        }
    }
}
```
Rule of thumb: To upgrade from read → write, drop the read lock first, then acquire the write lock. To downgrade, acquire write then acquire read then release write.

Advanced Usage Patterns
tryLock(timeout) for writers: allows avoiding long blocking or starvation.

lockInterruptibly(): allows interrupting a waiting thread (useful in responsive systems).

Fair mode: new ReentrantReadWriteLock(true) reduces writer starvation but may reduce throughput.

Example: writer using tryLock with timeout to avoid waiting forever:

```java

import java.util.concurrent.TimeUnit;

if (w.tryLock(500, TimeUnit.MILLISECONDS)) {
    try {
        // safe write
    } finally {
        w.unlock();
    }
} else {
    // couldn't get the lock within 500ms -> skip or retry later
}
```
Performance Considerations
ReadWriteLock pays overhead compared to a simple synchronized for very short critical sections. Use it when reads vastly outnumber writes.

Writer-heavy scenarios may perform worse with ReadWriteLock (due to the complexity and potential reader-writer contention).

Prefer specialized concurrent collections (ConcurrentHashMap, etc.) for common patterns — they are highly optimized.

When to Use ReadWriteLock (Summary)
Good fit: large, mostly-read data structures (caches, configuration, cached query results).

Not a fit: extremely short critical sections with heavy writes (lock overhead may dominate), or when you can use lock-free or concurrent collections.

Final Notes
Always unlock() in finally.

Be mindful of upgrade/downgrade semantics to avoid deadlocks.

Consider fairness vs throughput trade-offs.

Combine tryLock / lockInterruptibly to improve responsiveness and avoid starvation.

# 💀 Deadlock in Java — Coffman Conditions

Deadlock happens when two or more threads are **permanently blocked**, waiting for resources held by each other.  

According to **Coffman conditions**, a deadlock occurs only if **all four conditions** hold simultaneously.

---

## 🔑 Four Coffman Conditions

1. **Mutual Exclusion**
   - Resources cannot be shared (e.g., locks, files, DB connections).
   - Only one thread can use a resource at a time.

2. **Hold and Wait**
   - Threads hold resources while waiting for additional ones.
   - They don’t release what they already own.

3. **No Preemption**
   - Resources cannot be forcibly taken away.
   - Only the thread holding a resource can release it.

4. **Circular Wait**
   - A cycle of waiting exists:
     - Thread A waits for resource held by Thread B
     - Thread B waits for resource held by Thread C
     - Thread C waits for resource held by Thread A

---

## ⚠️ Deadlock Example (Java)

```java
class SharedResource {
    void method1() {
        synchronized (String.class) {
            System.out.println(Thread.currentThread().getName() + " locked String.class");

            try { Thread.sleep(100); } catch (InterruptedException e) {}

            synchronized (Integer.class) {
                System.out.println(Thread.currentThread().getName() + " locked Integer.class");
            }
        }
    }

    void method2() {
        synchronized (Integer.class) {
            System.out.println(Thread.currentThread().getName() + " locked Integer.class");

            try { Thread.sleep(100); } catch (InterruptedException e) {}

            synchronized (String.class) {
                System.out.println(Thread.currentThread().getName() + " locked String.class");
            }
        }
    }
}

public class DeadlockDemo {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();

        Thread t1 = new Thread(resource::method1, "Thread-1");
        Thread t2 = new Thread(resource::method2, "Thread-2");

        t1.start();
        t2.start();
    }
}
```
📝 Output (sample)
arduino
Copy code
Thread-1 locked String.class
Thread-2 locked Integer.class
// ❌ Both threads are stuck waiting forever
Here:

Thread-1 → holds String.class, waiting for Integer.class

Thread-2 → holds Integer.class, waiting for String.class

Both are stuck → Deadlock!

✅ Deadlock Prevention
We can prevent deadlocks by breaking at least one Coffman condition.
The most common strategy is breaking Circular Wait using a resource ordering rule.

Example: Ordered Lock Acquisition

```java
class SharedResourceSafe {
    void method1() {
        synchronized (String.class) {
            System.out.println(Thread.currentThread().getName() + " locked String.class");

            try { Thread.sleep(100); } catch (InterruptedException e) {}

            synchronized (Integer.class) {
                System.out.println(Thread.currentThread().getName() + " locked Integer.class");
            }
        }
    }

    void method2() {
        // Enforce same order: String -> Integer
        synchronized (String.class) {
            System.out.println(Thread.currentThread().getName() + " locked String.class");

            try { Thread.sleep(100); } catch (InterruptedException e) {}

            synchronized (Integer.class) {
                System.out.println(Thread.currentThread().getName() + " locked Integer.class");
            }
        }
    }
}

public class DeadlockPrevention {
    public static void main(String[] args) {
        SharedResourceSafe resource = new SharedResourceSafe();

        Thread t1 = new Thread(resource::method1, "Thread-1");
        Thread t2 = new Thread(resource::method2, "Thread-2");

        t1.start();
        t2.start();
    }
}
```
📝 Output (sample)
vbnet
Copy code
Thread-1 locked String.class
Thread-1 locked Integer.class
Thread-2 locked String.class
Thread-2 locked Integer.class
✅ No deadlock because both threads acquire locks in the same global order (String → Integer).

# 🔄 Thread Communication in Java (`wait()`, `notify()`, `notifyAll()`)

## ⚠️ Problem Without Communication
- Without proper communication, threads may end up in **busy-waiting** states.
- Busy-waiting = a thread keeps checking a condition repeatedly instead of waiting efficiently.
- This wastes CPU resources and can lead to **deadlocks**.

❌ Example of Busy Waiting (inefficient):
```java
while (!condition) {
    // continuously checking wastes CPU time
}
```
✅ Java's Solution: Thread Communication
Java provides methods from the Object class for thread communication:

wait()

Makes the current thread release the lock and wait until notify() or notifyAll() is called.

notify()

Wakes up one waiting thread (chosen by the JVM).

notifyAll()

Wakes up all waiting threads.

⚠️ Important: These methods must always be used inside a synchronized block or method.

🏭 Example: Producer-Consumer Problem
We’ll implement the classic Producer-Consumer Problem using wait() and notify().

🔹 Shared Resource Class
```java

class SharedResource {
    private int data;
    private boolean hasData = false;

    // Producer puts data
    public synchronized void produce(int value) {
        while (hasData) { // wait until consumer consumes
            try {
                wait(); // releases lock and waits
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        data = value;
        hasData = true;
        System.out.println("Produced: " + value);
        notify(); // wake up consumer
    }

    // Consumer takes data
    public synchronized int consume() {
        while (!hasData) { // wait until producer produces
            try {
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        hasData = false;
        notify(); // wake up producer
        return data;
    }
}
```
🔹 Producer Thread
```java

class Producer implements Runnable {
    private final SharedResource resource;

    public Producer(SharedResource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 1; i <= 10; i++) {
            resource.produce(i);
        }
    }
}
```
🔹 Consumer Thread
```java

class Consumer implements Runnable {
    private final SharedResource resource;

    public Consumer(SharedResource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 1; i <= 10; i++) {
            int value = resource.consume();
            System.out.println("Consumed: " + value);
        }
    }
}
```
🔹 Driver Program
```java

public class ThreadCommunicationDemo {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();

        Thread producer = new Thread(new Producer(resource), "Producer");
        Thread consumer = new Thread(new Consumer(resource), "Consumer");

        producer.start();
        consumer.start();
    }
}
```
📝 Sample Output

Produced: 1
Consumed: 1
Produced: 2
Consumed: 2
...
Produced: 10
Consumed: 10
🔑 Key Points
wait() → Pauses the thread until notified, releases lock while waiting.

notify() → Wakes up one waiting thread.

notifyAll() → Wakes up all waiting threads.

Always use wait() inside a loop (while) to avoid spurious wakeups.

synchronized is required when using wait(), notify(), and notifyAll().

📌 Why Use notifyAll()?
If multiple consumers or producers exist, notifyAll() ensures no thread remains waiting forever.

notify() is fine for single producer-consumer setups.

🚀 With wait(), notify(), and notifyAll(), threads can communicate efficiently, avoiding busy-waiting and deadlocks.

# 🧵 Thread Safety in Java

## 📌 What is Thread Safety?
**Thread Safety** means that a piece of code, class, or object behaves correctly when **accessed by multiple threads at the same time**, without causing:
- Inconsistent data
- Race conditions
- Unexpected behavior

If code is **thread-safe**, it ensures that:
1. Data remains consistent.
2. No corruption or errors occur when multiple threads access shared resources.
3. Synchronization or proper concurrency mechanisms are used.

---

# 🔹 Lambda Expressions in Java

## 📌 What is a Lambda Expression?
A **Lambda Expression** is a short block of code that represents an **anonymous function** (a function without a name).  
It can be treated like an object and passed around as a parameter to methods.  

👉 Introduced in **Java 8**, it enables **functional programming** and makes code more concise.

---

## 🔑 Key Characteristics
- ✅ **Anonymous function** → Has no name.  
- ✅ **Concise syntax** → Shorter than anonymous inner classes.  
- ✅ **Functional interface requirement** → Works only with **Functional Interfaces** (interfaces with exactly **one abstract method**).  
- ✅ **Can be passed as arguments** → Used in multithreading, collections, and event handling.

---

## 📝 Syntax
```java
(parameters) -> { expression or statements }
⚡ Examples
1. Simple Lambda

Runnable runnable = () -> System.out.println("Hello");
Thread t1 = new Thread(runnable);
t1.start();
Or even shorter:


Thread t1 = new Thread(() -> System.out.println("Hello"));
t1.start();
2. With Parameters

Student lawStudent = name -> name + " is law student";

System.out.println(lawStudent.getBio("Ram"));
3. Multi-Line Lambda

Thread t1 = new Thread(() -> {
    for (int i = 0; i < 10; i++) {
        System.out.println("Hello world");
    }
});
t1.start();

4. Functional Interface Example

@FunctionalInterface
public interface Student {
    String getBio(String name);
}

public class Test2 {
    public static void main(String[] args) {
        // Anonymous inner class
        Student engineeringStudent = new Student() {
            @Override
            public String getBio(String name) {
                return name + " is Engineering student";
            }
        };

        // Lambda expression
        Student lawStudent = name -> name + " is law student";

        System.out.println(engineeringStudent.getBio("Ram"));
        System.out.println(lawStudent.getBio("Shyam"));
    }
}
```
✅ Output

Ram is Engineering student
Shyam is law student

🎯 Why Use Lambda Expressions?
Less boilerplate code → No need for long anonymous inner classes.

Improves readability → Code is shorter and cleaner.

Encourages functional programming in Java.

Works well with Streams API, Collections API, and Concurrency API.

📋 Summary
Lambda expressions allow you to write anonymous functions.

They can only be used with functional interfaces.

Syntax: (parameters) -> { body }.

Makes Java code more concise, readable, and expressive.
# 🔄 Thread Pool in Java

## 📌 What is a Thread Pool?
A **Thread Pool** is a collection of pre-created reusable threads that are managed by a framework (like `ExecutorService`).  
Instead of creating a new thread every time a task is needed, the application reuses existing threads from the pool.

---

## 🤔 Why Do We Need a Thread Pool?

### 1. **Resource Management**
- Creating a new thread is **expensive** (takes memory, CPU time).
- Too many threads can cause **OutOfMemoryError** or **context switching overhead**.
- Thread Pool limits the **maximum number of active threads**, ensuring efficient use of system resources.

### 2. **Improved Performance**
- Reusing threads reduces the cost of thread creation/destruction.
- Ideal for handling a large number of **short-lived tasks**.

### 3. **Scalability**
- Provides better scalability for applications with **concurrent tasks**.
- Prevents uncontrolled growth of threads.

### 4. **Centralized Management**
- Easier to control thread lifecycle.
- Allows scheduling, prioritization, and monitoring of tasks.

---

## ⚡ Example: Without Thread Pool
```java
public class WithoutThreadPool {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            Thread thread = new Thread(() -> {
                System.out.println("Task executed by " + Thread.currentThread().getName());
            });
            thread.start();
        }
    }
}
```
❌ Creates a new thread for every task → expensive.

❌ No limit on threads → may exhaust resources.

✅ Example: With Thread Pool (ExecutorService)
```java

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class WithThreadPool {
    public static void main(String[] args) {
        // Create a fixed thread pool with 3 threads
        ExecutorService executor = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 10; i++) {
            final int taskId = i;
            executor.execute(() -> {
                System.out.println("Task " + taskId + " executed by " + Thread.currentThread().getName());
            });
        }

        executor.shutdown(); // Gracefully shut down the pool
    }
}
```
✅ Output (sample)

Task 1 executed by pool-1-thread-1
Task 2 executed by pool-1-thread-2
Task 3 executed by pool-1-thread-3
Task 4 executed by pool-1-thread-1
...
🛠️ Types of Thread Pools in Java
Java provides different Executor factory methods (Executors class):

FixedThreadPool → A fixed number of threads (Executors.newFixedThreadPool(n)).

CachedThreadPool → Creates new threads as needed, reuses old ones (Executors.newCachedThreadPool()).

SingleThreadExecutor → Only one thread executes tasks sequentially (Executors.newSingleThreadExecutor()).

ScheduledThreadPool → For periodic tasks (Executors.newScheduledThreadPool(n)).

🔑 Key Takeaways
Thread Pool = Reusable threads for efficient resource management.

Prevents system from being overloaded by too many threads.

Provides better performance, scalability, and control.

Used widely in web servers, databases, and multithreaded applications.

# ⚡ Executors Framework in Java

## 📌 Introduction
The **Executors Framework** was introduced in **Java 5** as part of the `java.util.concurrent` package.  
It simplifies the development of concurrent applications by abstracting away the complexity of **creating, managing, and controlling threads**.

---

## 🚦 Why Executors Framework?
1. **Manual Thread Management** → Avoids creating threads manually for every task.  
2. **Resource Management** → Prevents system overload by limiting the number of concurrent threads.  
3. **Scalability** → Handles thousands of tasks efficiently.  
4. **Thread Reuse** → Reuses existing threads instead of creating/destroying repeatedly.  
5. **Error Handling** → Provides `Future` and exception handling support.  

---

## ✅ Example: Fixed Thread Pool
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        ExecutorService executor = Executors.newFixedThreadPool(9);

        for (int i = 1; i < 10; i++) {
            int finalI = i;
            executor.submit(() -> {
                long result = factorial(finalI);
                System.out.println(result);
            });
        }

        executor.shutdown();
        System.out.println("Total time: " + (System.currentTimeMillis() - startTime));
    }

    private static long factorial(int n) {
        long fact = 1;
        for (int i = 1; i <= n; i++) fact *= i;
        return fact;
    }
}
✅ Example: Fixed Pool (3 Threads)
java
Copy code
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        for (int i = 1; i < 10; i++) {
            int finalI = i;
            executor.submit(() -> {
                long result = factorial(finalI);
                System.out.println(result);
            });
        }

        executor.shutdown();
    }

    private static long factorial(int n) {
        long fact = 1;
        for (int i = 1; i <= n; i++) fact *= i;
        return fact;
    }
}
✅ Example: Single Thread Executor + Future
java
Copy code
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService = Executors.newSingleThreadExecutor();

        Future<?> future = executorService.submit(() -> System.out.println("Hello"));
        future.get(); // waits for task completion

        executorService.shutdown();
    }
}
🔑 ExecutorService Methods
Method	Description
.submit(Runnable)	Runs a task (no return value).
.submit(Callable)	Runs a task and returns a value.
.submit(Runnable, result)	Runs task and returns given result.
.shutdown()	Initiates graceful shutdown (no new tasks).
.shutdownNow()	Stops all running tasks immediately.
.awaitTermination()	Waits for tasks to finish before shutdown.
.isShutdown()	Returns true if shutdown has started.
.isTerminated()	Returns true if all tasks finished.

✅ Example: Callable with Return Value
java
Copy code
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService = Executors.newFixedThreadPool(2);

        Future<Integer> submit = executorService.submit(() -> 1 + 2);
        Integer i = submit.get(); // waits for result

        System.out.println("sum is " + i);

        executorService.shutdown();
        System.out.println(executorService.isTerminated());
    }
}
// Output:
// sum is 3
// true
✅ Example: invokeAll()
java
Copy code
import java.util.concurrent.*;
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService = Executors.newFixedThreadPool(2);

        Callable<Integer> callable1 = () -> 1;
        Callable<Integer> callable2 = () -> 2;
        Callable<Integer> callable3 = () -> 3;

        List<Callable<Integer>> list = Arrays.asList(callable1, callable2, callable3);

        List<Future<Integer>> futures = executorService.invokeAll(list);

        for (Future<Integer> f : futures) {
            System.out.println(f.get());
        }

        executorService.shutdown();
    }
}
🆚 Runnable vs Callable
Feature	Runnable	Callable
Interface	java.lang.Runnable	java.util.concurrent.Callable<V>
Method	void run()	V call() throws Exception
Return Type	void (no result)	Generic type V (can return result)
Exceptions	Cannot throw checked exceptions	Can throw checked exceptions
Usage	Tasks with no return value	Tasks needing a result or exception handling

🌟 Key Benefits of ExecutorService
Thread Pool Management: Reuses threads efficiently.

Resource Control: Limits concurrent threads.

Task Scheduling: Manages execution order.

Result Handling: Uses Future to retrieve results.

Lifecycle Management: Clean startup/shutdown of thread pools.

