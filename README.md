# 🚚 EasyStorageTracker v2 – Java Generics + Exception Handling Assignment

Welcome to **EasyStorageTracker v2**, where we still build a type-safe, flexible storage system — but now with the power of **robust exception handling**.

It’s Java, but more real-world and with better error messages than your average PHP script 😏

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

## 🚀 Assignment Requirements

### 🔹 Task 1: GitHub Setup (5 points)
- Create a public repo `EasyStorageTrackerV2-YourName`
- Add a brief `README.md`
- Push a minimum of **3 commits**:
  1. Project scaffold + generic classes
  2. Exception logic + item classes
  3. Utility and test/demo code

---

### 🔹 Task 2: Enhance the Storage System (30 points)

#### ✅ `StorageUnit<T>` (Generic Class)
- Can store any type of item `T`
- Add exception handling:
  - Throw `EmptyStorageException` (custom) if `getItem()` is called on empty storage
- Methods:
  - `void addItem(T item)`
  - `T getItem() throws EmptyStorageException`

#### ✅ Create `Perishable` abstract class
- Add:
  - `LocalDate expirationDate`
  - `boolean isExpired()`

#### ✅ Create item classes:
- `Book`, `Device`, `Snack` (with custom fields)
- Let `Snack` extend `Perishable`

#### ✅ Main class (`TrackerDemo`)
- Try to store and retrieve items
- Intentionally test:
  - Getting from empty storage
  - Adding an expired perishable
- Use multiple `try-catch` blocks, including a `finally` message

---

### 🔹 Task 3: Utility Class with Exceptions (25 points)

Create a `StorageUtils` class with:

#### 📌 Wildcard Method:
```java
public static void printItems(List<? extends Object> items)
```

#### 📌 Generic Method:
```java
public static <T> void displayItem(T item)
```

#### 📌 Bounded Type Method with Validation:
```java
public static <T extends Perishable> void checkPerishable(T item) throws ExpiredItemException
```

Create `ExpiredItemException` (custom exception) to be thrown when `item.isExpired()` is `true`.

---

### 🔹 Task 4: Document and Validate (10 points)

- Add basic `Javadoc` to your classes and methods
- Include user-friendly exception messages
- Add a method to validate user input (simulate null or invalid data for unchecked exception demo)

---

## 💡 Bonus Tasks (Optional – 10 points)

### 🔸 Extra 1: `StorageManager<T>`
- Store multiple items in a list
- Add method to bulk-check perishable expiration and throw collective summary

### 🔸 Extra 2: Add Logging
- Use `Logger` or `System.out.println()` to track exception events cleanly

### 🔸 Extra 3: Command-line Interface
- Use `Scanner` to allow the user to add and retrieve items interactively (simulate input validation errors)

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

## 🧠 Tips for Success

- Use your exception types wisely: unchecked for bad input, checked for rule enforcement, custom for clarity.
- Break tasks into small testable parts.
- Keep code clean and JavaDoc'd — remember, future you (or a teammate) will thank you!

---

## 📤 Submission

- Submit your **GitHub repo link** via your LMS.
- Make sure the repo is public and builds cleanly.

