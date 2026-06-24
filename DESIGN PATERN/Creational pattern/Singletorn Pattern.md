
Different Ways to Achieve Thread Safety

There are several ways to make the Singleton pattern thread-safe. Here are a few common approaches:
1. Synchronized Method

This is the simplest way to ensure thread safety. By synchronizing the method that creates the instance, we can prevent multiple threads from creating separate instances at the same time. However, this approach can lead to performance issues due to the overhead of synchronization.

Consider the following code snippet for better understanding:
Java


```java
public class Singleton {
    // Object declaration
    private static Singleton instance;

    // Private constructor
    private Singleton() {}

    // Synchronized keyword used
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
What synchronized keyword does?

The synchronized keyword ensures that only one thread at a time can execute the getInstance() method. This prevents multiple threads from entering the method simultaneously and creating multiple instances.
Pros

Simple and easy to implement.
Thread-safe without needing complex logic.
Cons

Performance overhead: Every call to getInstance() is synchronized, even after the instance is created.
May slow down the application in high-concurrency scenarios.

2. Double-Checked Locking

This is a more efficient way to achieve thread safety. The idea is to check if the instance is null before acquiring the lock. If it is, then we synchronize the block and check again. This reduces the overhead of synchronization after the instance has been created.

Consider the following code snippet for better understanding:
Java


```java
public class Singleton {
    // Volatile object declaration
    private static volatile Singleton instance;

    // Private constructor
    private Singleton() {}

    // Thread-safe method using double-checked locking
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
Understanding

The outer if check avoids synchronization once the instance is created.
The inner if inside synchronized ensures that only one thread creates the instance.
volatile keyword ensures changes made by one thread are visible to others. Without volatile, one thread might create the Singleton instance, but other threads may not see the updated value due to caching. volatile ensures that the instance is always read from the main memory, so all threads see the most up-to-date version.
Pros

Efficient: Synchronization only happens once, when the instance is created.
Safe and fast in concurrent environments.
Cons

Slightly more complex than the synchronized method.
Requires Java 1.5 or above due to reliance on volatile.

3. Bill Pugh Singleton (Best Practice for Lazy Loading)

This is a highly efficient way to implement the Singleton pattern. It uses a static inner helper class to hold the Singleton instance. The instance is created only when the inner class is loaded, which happens only when getInstance() is called for the first time.

Consider the following code snippet for better understanding:
Java


```java
public class Singleton {
    // Private constructor
    private Singleton() {}

    // Static inner class to hold the Singleton instance
    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    // Public method to return the Singleton instance
    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```
Explanation

The Singleton instance is not created until getInstance() is called.
The static inner class (Holder) is not loaded until referenced, thanks to Java's class loading mechanism.
It ensures thread safety, lazy loading, and high performance without synchronization overhead.
Pros

Best of both worlds: Lazy + Thread-safe.
No need for synchronized or volatile.
Clean and efficient.
Cons

It is slightly less intuitive for beginners due to the use of a nested static class.