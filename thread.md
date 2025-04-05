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
Since threads run concurrently, the order of output is not guaranteed. It can vary every time you run the program.
the result may be:
```
Main Thread: 0
Child Thread: 0
Child Thread: 1
Main Thread: 1
Main Thread: 2
Child Thread: 2
Main Thread: 3
Child Thread: 3
Main Thread: 4
Child Thread: 4
```
or 
```
Child Thread: 0
Child Thread: 1
Child Thread: 2
Main Thread: 0
Child Thread: 3
Main Thread: 1
Child Thread: 4
Main Thread: 2
Main Thread: 3
Main Thread: 4
```
The order will be randomly displayed 

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
The output is will be in the same situation as Thread, order can not be guaranteed, it will chaneg every time 


> ✅ Use `Runnable` when you want more flexibility (e.g., using thread pools).
This is different between Thread class an runable Interface
```
---
|Feature	   | Thread Class               |	Runnable Interface                           ｜
|Type	        |  Class (extends Thread)    |	Interface (implements Runnable)              ｜
|Inheritance	| Can’t extend other classes |	Can extend other classes (better flexibility)｜
|Best use case	| Simple one-off threads     |	Recommended for reusability & thread pooling ｜
|Used with     	| new MyThread().start()     |	new Thread(new MyRunnable()).start()         ｜
>
```

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
When multiple threads access and modify a shared variable at the same time, it can lead to inconsistent or incorrect results. This is called a race condition.
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

        System.out.println("Final Count: " + count); // Not always 2000 sometime is less than that number
    }
}
```
### System.out.println("Final Count: " + count); // Not always 2000 sometime is less than that number, It because count++ is not atomic.
count++ actually does 3 steps behind the scenes
```
// Internally, this is what happens:
1. Read the current value of count from memory
2. Add 1 to the value
3. Write the new value back to memory
```
```
Thread A reads count = 42
Thread B also reads count = 42
Both add 1 → get 43
Both write back 43
Result: count went from 42 → 43 instead of 44 
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
this Synchronized tell program taht Only one thread is allowed to enter this method at a time — others must wait
The step now will 
```
| 	| Thread A| Thread B |
| 1 | Locks the method	| Waits (blocked)
| 2	| Safely does count++ | do nothing |	
| 3	| Unlocks the method | Starts now|
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
