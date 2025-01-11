# vibemeet-interview

## 1. Core Java Concepts

- **`final`**: A keyword used to declare constants, prevent method overriding, and inheritance of classes.
  - **Use Case**:
    ```java
    public class Example {
        public static final int MAX_VALUE = 100;
    }
    ```

- **`finally`**: A block in exception handling that is always executed regardless of whether an exception is thrown.
  - **Use Case**:
    ```java
    try {
        // code that might throw an exception
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        // cleanup code, e.g., closing a file or database connection
    }
    ```

- **`finalize()`**: A method used to perform cleanup operations before an object is garbage collected.
  - **Use Case**:
    ```java
    @Override
    protected void finalize() throws Throwable {
        try {
            // cleanup code
        } finally {
            super.finalize();
        }
    }
    ```

## 2. Object-Oriented Programming

- **What happens if the `Dog` class does not implement the `speak` method?**
  - If the `Dog` class does not implement the `speak` method, it will result in a compilation error because `Dog` is not abstract and does not override the abstract method `speak()` in `Animal`.

- **Can you modify the `sleep` method in the `Animal` interface to prevent it from being overridden?**
  - Yes, you can modify the `sleep` method to prevent it from being overridden by using the `final` keyword. However, interfaces do not support `final` methods, but you can use `static` methods which are implicitly `final`.
  - **Use Case**:
    ```java
    interface Animal {
        void speak();
        static void sleep() {
            System.out.println("Sleeping...");
        }
    }
    ```

## 3. Collections Framework

- **Describe the internal structure of a `HashMap` in Java and explain how it resolves collisions. What changes were introduced in the `HashMap` implementation in Java 8?**
  - **Internal Structure**: A `HashMap` is internally an array of buckets. Each bucket is a linked list (or tree in Java 8 and later).
  - **Collisions**: Collisions occur when two keys have the same hash value. They are resolved using:
    - **Separate chaining**: Storing entries in a linked list/tree in the bucket.
  - **Java 8 Changes**: Java 8 introduced a significant change where the linked list is replaced with a balanced tree (Red-Black Tree) once the size of the linked list exceeds a certain threshold (8). This improves the performance from O(n) to O(log n) for high-collision scenarios.

## 4. Multithreading

- **Write a simple program to demonstrate the difference between synchronized blocks and ReentrantLock. Explain when you would prefer one over the other:**
  - **Use Case**:
    ```java
    import java.util.concurrent.locks.ReentrantLock;

    public class SyncVsLock {
        private static final ReentrantLock lock = new ReentrantLock();
        private static int counter = 0;

        public static void main(String[] args) {
            Runnable syncTask = () -> {
                synchronized (SyncVsLock.class) {
                    counter++;
                }
            };

            Runnable lockTask = () -> {
                lock.lock();
                try {
                    counter++;
                } finally {
                    lock.unlock();
                }
            };

            Thread syncThread1 = new Thread(syncTask);
            Thread syncThread2 = new Thread(syncTask);
            Thread lockThread1 = new Thread(lockTask);
            Thread lockThread2 = new Thread(lockTask);

            syncThread1.start();
            syncThread2.start();
            lockThread1.start();
            lockThread2.start();
        }
    }
    ```

- **When to prefer one over the other:**
  - Use `synchronized` blocks for simple synchronization needs.
  - Use `ReentrantLock` when you need advanced features like fairness, `tryLock`, or interruptible locks.

## 5. Memory Management

- **What is a memory leak in Java?**
  - A memory leak occurs when objects are no longer needed but are still referenced, preventing the garbage collector from reclaiming their memory.

- **How does the garbage collector help prevent it?**
  - The garbage collector automatically frees memory by destroying unreachable objects.

- **Can you give an example where a memory leak might still occur despite garbage collection?**
  - **Use Case**:
    ```java
    import java.util.ArrayList;
    import java.util.List;

    public class MemoryLeakExample {
        private List<Object> list = new ArrayList<>();

        public void addToList(Object obj) {
            list.add(obj); // Objects added to the list are never removed, causing a memory leak
        }
    }
    ```

## 6. Streams and Functional Programming

- **Code Snippet**:
  ```java
  import java.util.Arrays;
  import java.util.List;
  import java.util.stream.Collectors;

  public class StreamExample {
      public static void main(String[] args) {
          List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

          List<Integer> evenNumbers = numbers.stream()
              .filter(n -> n % 2 == 0)
              .sorted((a, b) -> b - a)
              .collect(Collectors.toList());

          System.out.println(evenNumbers); // Output: [10, 8, 6, 4, 2]
      }
  }

- **Explain how Streams improve performance and readability over traditional loops**:
- Streams allow for declarative programming, making code more readable and concise.
- They can be optimized by the JVM for better performance, especially with parallel streams.

## 7. Exception Handling

- **What is the difference between checked and unchecked exceptions in Java?**
  - **Checked Exceptions**: Must be declared in the method signature or handled using a try-catch block (e.g., `IOException`).
  - **Unchecked Exceptions**: Runtime exceptions that do not need to be declared or handled (e.g., `NullPointerException`).

- **Can you write a custom unchecked exception class and demonstrate its usage?**
  - **Use Case**:
    ```java
    public class CustomUncheckedException extends RuntimeException {
        public CustomUncheckedException(String message) {
            super(message);
        }
    }

    public class Main {
        public static void main(String[] args) {
            throw new CustomUncheckedException("This is a custom unchecked exception");
        }
    }
    ```

## 8. Java 8+ Features

- **Code Snippet**:
  ```java
  import java.util.concurrent.*;

  public class CompletableFutureExample {
      public static void main(String[] args) {
          CompletableFuture<Integer> task1 = CompletableFuture.supplyAsync(() -> 10);
          CompletableFuture<Integer> task2 = CompletableFuture.supplyAsync(() -> 20);

          task1.thenCombine(task2, Integer::sum)
               .thenAccept(result -> System.out.println("Result: " + result));
      }
  }

## 9. Performance Optimization

- **A Java application is experiencing high CPU usage due to frequent garbage collection. What steps would you take to identify and resolve the issue? Mention specific tools or JVM options you would use:**

  - **Steps to identify and resolve high CPU usage due to frequent garbage collection:**

    - **Identify the Issue:**
      - Use tools like VisualVM, JConsole, or Java Mission Control to monitor GC activity.
      - Analyze GC logs by enabling `-XX:+PrintGCDetails -XX:+PrintGCDateStamps`.
    
    - **Resolve the Issue:**
      - Increase heap size (`-Xmx`).
      - Tune GC parameters (e.g., `-XX:NewRatio`).
      - Consider using a different GC algorithm (e.g., `G1GC`).

## 10. Advanced Topics: Difference between volatile and AtomicInteger

- **volatile**:
  - Ensures visibility of changes to variables across threads.
  - Does not guarantee atomicity.
  - **Example**:
    ```java
    public class VolatileExample {
        private volatile int counter;

        public void increment() {
            counter++; // Not atomic, may result in race conditions
        }
    }
    ```

- **AtomicInteger**:
  - Provides atomic operations for integers.
  - Ensures both visibility and atomicity.
  - **Example**:
    ```java
    import java.util.concurrent.atomic.AtomicInteger;

    public class AtomicExample {
        private AtomicInteger counter = new AtomicInteger(0);

        public void increment() {
            counter.incrementAndGet(); // Atomic and thread-safe
        }
    }
    ```

- **Example where volatile is insufficient**:
  ```java
  public class VolatileCounter {
      private volatile int counter = 0;

      public void increment() {
          counter++; // Not atomic, may result in race conditions
      }
  }

  public class AtomicCounter {
      private AtomicInteger counter = new AtomicInteger(0);

      public void increment() {
          counter.incrementAndGet(); // Atomic and thread-safe
      }
  }
