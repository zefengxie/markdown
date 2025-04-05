# 🚀 Java Threading & Thread Management Tutorial
## ✅ What is a Thread?
A **thread** is a single path of execution within a program. By default, every Java program starts with one thread — the `main` thread. We can create additional threads to run tasks **concurrently**.
### 🧠 Real-world analogy:
> While you're writing code, your computer can also play music — this is multi-threading.
---
## 1. Create a Thread (Two Ways)
### 🔹 Method 1: Extend the `Thread` class
```java
public class MyThread extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Child Thread: " + i);
        }
    }

    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start(); // Start the thread

        for (int i = 0; i < 5; i++) {
            System.out.println("Main Thread: " + i);
        }
    }
}
```
### 🔹 Method 2: Implement the `Runnable` interface (✅ Recommended)
```java
public class MyRunnable implements Runnable {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Runnable Thread: " + i);
        }
    }

    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start();

        for (int i = 0; i < 5; i++) {
            System.out.println("Main Thread: " + i);
        }
    }
}
```
> ✅ Use `Runnable` when you want more flexibility (e.g., using thread pools).
---
## 2. Thread Methods
| Method      | Description                          |
|-------------|--------------------------------------|
| `start()`   | Starts a new thread                  |
| `run()`     | Contains the code to be executed     |
| `sleep(ms)` | Pauses thread for `ms` milliseconds  |
| `join()`    | Waits for another thread to finish   |
| `isAlive()` | Checks if a thread is still running  |
### 🔸 Example: Sleep
```java
public class SleepExample extends Thread {
    public void run() {
        try {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Child Thread: " + i);
                Thread.sleep(1000); // pause for 1 second
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
---
## 3. Thread Safety with `synchronized`
### ❌ Problem: Race condition when accessing shared variable
```java
public class UnsafeCounter {
    static int count = 0;

    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                count++; // Unsafe
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start(); t2.start();

        try {
            t1.join(); t2.join();
        } catch (InterruptedException e) {}

        System.out.println("Final Count: " + count); // Not always 2000
    }
}
```
### ✅ Solution: Use `synchronized`
```java
public class SafeCounter {
    static int count = 0;

    public synchronized static void increment() {
        count++;
    }

    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                increment();
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start(); t2.start();

        try {
            t1.join(); t2.join();
        } catch (InterruptedException e) {}

        System.out.println("Final Count: " + count); // Always 2000
    }
}
```
---
## 4. Thread Communication with `wait()` / `notify()`
Used when **threads need to cooperate**, like in **Producer-Consumer problems**.
👉 Let me know if you'd like this explained next!
---
## 5. Thread Pools with `ExecutorService`
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService pool = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 5; i++) {
            int taskId = i;
            pool.submit(() -> {
                System.out.println("Running Task #" + taskId + " in " + Thread.currentThread().getName());
            });
        }

        pool.shutdown();
    }
}
```
---
## 🧠 Exercises
### 1. Alternate printing using threads
```
Thread A prints "A"
Thread B prints "B"
Repeat A B A B ... 10 times
```
### 2. Thread-safe counter
- Shared variable `count`
- Two threads add to `count`
- Use `synchronized` to ensure thread safety
### 3. Thread pool task manager
- Create a thread pool of size 3
- Submit 5 tasks
- Each task should print its number and thread name
---
Would you like me to prepare:
- A downloadable `.md` version?
- Real Java files you can run?
- Next topic: Thread communication or GUI with threads?
