# 🚚 EasyStorageTracker v2 – Java Generics + Exception Handling Assignment

Welcome to **EasyStorageTracker v2**, where we still build a type-safe, flexible storage system — but now with the power of **robust exception handling**.

It’s Java, but more real-world and with better error messages than your average Java code 😏

---

## 🎯 Objective

You're enhancing the EasyStorageTracker system for a delivery company, adding proper error-handling. Imagine trying to store expired milk or retrieve an item from empty storage — what should happen?

You’ll integrate **exception handling** into the system, alongside **generics**, **wildcards**, and **bounded types**.

---

## ✅ Learning Outcomes

- 📦 Generic Classes & Methods  
- 🧠 Bounded Type Parameters (`<T extends Perishable>`)  
- 🔄 Wildcards (`List<? extends Object>`)  
- 🧯 Exception Handling in Java:
  - Checked Exceptions
  - Unchecked Exceptions
  - Custom Exceptions  
- 🧰 Utility Classes + Defensive Programming  
- 🌐 GitHub workflows & commit discipline  

---


## 📦 Suggested Project Structure

```
EasyStorageTrackerV2/
├── src/
│   ├── model/
│   │   ├── Book.java
│   │   ├── Device.java
│   │   ├── Snack.java
│   │   ├── Perishable.java
│   ├── storage/
│   │   ├── StorageUnit.java
│   │   ├── StorageUtils.java
│   ├── exception/
│   │   ├── EmptyStorageException.java
│   │   ├── ExpiredItemException.java
│   └── main/
│       └── TrackerDemo.java
├── README.md
```

---

### 🛠 Step-by-Step Java Project: EasyStorageTracker v2

---

## 📦 Step 1: Create Your Generic StorageUnit

This class can store any object, but only one item at a time.

---

### ✅ `StorageUnit.java`

```java
package storage;

import exception.EmptyStorageException;

public class StorageUnit<T> {
    private T item; // The stored item

    // Add item to storage
    public void addItem(T item) {
        this.item = item;
        System.out.println("Item stored successfully: " + item);
    }

    // Retrieve item
    public T getItem() throws EmptyStorageException {
        if (item == null) { // 🧠 This is how we know it's empty!
            throw new EmptyStorageException("Storage is empty! Cannot retrieve item.");
        }
        return item;
    }
}
```

---

## 💣 Step 2: Create a Custom Exception – EmptyStorageException

---

### ✅ `EmptyStorageException.java`

```java
package exception;

public class EmptyStorageException extends Exception {
    public EmptyStorageException(String message) {
        super(message);
    }
}
```

🧠 **When is it thrown?** → When someone tries to get an item, but nothing is stored.

---

## 🧊 Step 3: Add Perishable Items

Let’s model something like milk using a simple number as expiration logic.

---

### ✅ `Perishable.java`

```java
package model;

public abstract class Perishable {
    private int expirationDay; // Example: day 10

    public Perishable(int expirationDay) {
        this.expirationDay = expirationDay;
    }

    public boolean isExpired(int currentDay) {
        return currentDay > expirationDay;
    }

    public int getExpirationDay() {
        return expirationDay;
    }
}
```

🧠 We’re using `int` instead of `LocalDate`, so you can just say today is day 12, and if the item expires on day 10 → it's expired.

---

## 🥛 Step 4: Create Snack (a Perishable)

---

### ✅ `Snack.java`

```java
package model;

public class Snack extends Perishable {
    private String name;

    public Snack(String name, int expirationDay) {
        super(expirationDay);
        this.name = name;
    }

    public String toString() {
        return "Snack: " + name + " (expires on day " + getExpirationDay() + ")";
    }
}
```

---

## 💣 Step 5: Custom Exception for Expired Items

---

### ✅ `ExpiredItemException.java`

```java
package exception;

public class ExpiredItemException extends Exception {
    public ExpiredItemException(String message) {
        super(message);
    }
}
```

🧠 **When is it thrown?** → When someone tries to store or validate an expired perishable item.

---

## 🧰 Step 6: Utility Methods with Exception Handling

---

### ✅ `StorageUtils.java`

```java
package storage;

import exception.ExpiredItemException;
import model.Perishable;

import java.util.List;

public class StorageUtils {

    // Wildcard method
    public static void printItems(List<? extends Object> items) {
        for (Object item : items) {
            System.out.println("Stored item: " + item);
        }
    }

    // Generic method
    public static <T> void displayItem(T item) {
        System.out.println("Displaying item: " + item);
    }

    // Bounded type method with exception
    public static <T extends Perishable> void checkPerishable(T item, int currentDay) throws ExpiredItemException {
        if (item.isExpired(currentDay)) {
            throw new ExpiredItemException("This item is expired and cannot be stored.");
        }
    }
}
```

---

## 🎬 Step 7: Main Class to Demonstrate

---

### ✅ `TrackerDemo.java`

```java
package main;

import exception.EmptyStorageException;
import exception.ExpiredItemException;
import model.Snack;
import storage.StorageUnit;
import storage.StorageUtils;

public class TrackerDemo {

    public static void main(String[] args) {
        StorageUnit<Snack> snackStorage = new StorageUnit<>();

        // Set current day of system (for simulation)
        int today = 12;

        Snack milk = new Snack("Milk", 10); // expired
        Snack chips = new Snack("Chips", 15); // fresh

        // Track events using System.out
        System.out.println("[INFO] Trying to store milk...");

        try {
            StorageUtils.checkPerishable(milk, today); // Throws exception
            snackStorage.addItem(milk);
        } catch (ExpiredItemException e) {
            System.out.println("[ERROR] " + e.getMessage());
        }

        System.out.println("[INFO] Trying to store chips...");

        try {
            StorageUtils.checkPerishable(chips, today); // Should pass
            snackStorage.addItem(chips);
        } catch (ExpiredItemException e) {
            System.out.println("[ERROR] " + e.getMessage());
        }

        // Try to get item from storage
        try {
            Snack item = snackStorage.getItem(); // Safe
            System.out.println("[INFO] Got item: " + item);
        } catch (EmptyStorageException e) {
            System.out.println("[ERROR] " + e.getMessage());
        }

        // Simulate input validation error
        try {
            String userInput = null;
            if (userInput == null || userInput.isEmpty()) {
                throw new IllegalArgumentException("Invalid input! Item name cannot be null or empty.");
            }
        } catch (IllegalArgumentException e) {
            System.out.println("[WARNING] " + e.getMessage());
        } finally {
            System.out.println("[INFO] Thank you for using EasyStorageTracker!");
        }
    }
}
```

---

## 📋 Summary of Simulated Exceptions

| Situation                             | Exception Type            | When It Happens                                      |
|--------------------------------------|---------------------------|------------------------------------------------------|
| Getting from empty storage           | `EmptyStorageException`   | When no item is stored                              |
| Storing expired perishable item      | `ExpiredItemException`    | When `isExpired()` returns true                     |
| Invalid user input (null name)       | `IllegalArgumentException`| Simulated input validation                          |

---

## 🧪 Output Example

```
[INFO] Trying to store milk...
[ERROR] This item is expired and cannot be stored.
[INFO] Trying to store chips...
Item stored successfully: Snack: Chips (expires on day 15)
[INFO] Got item: Snack: Chips (expires on day 15)
[WARNING] Invalid input! Item name cannot be null or empty.
[INFO] Thank you for using EasyStorageTracker!
```
