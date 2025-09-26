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

