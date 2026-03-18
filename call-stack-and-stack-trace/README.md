# 📋 Call Stack and Stack Trace

## 🎯 Learning Objectives

- Understand what the call stack is and how the JVM uses it
- Read and interpret a Java stack trace
- Identify the origin of an exception from a stack trace
- Understand stack frames and method invocation order

---

## 📌 Exercise 1: Trace the Call Stack

### Question

Consider the following program:

```java
public class CallStackDemo {

    public static void methodC() {
        System.out.println("Inside C");
        throw new RuntimeException("Error in C!");
    }

    public static void methodB() {
        System.out.println("Inside B");
        methodC();
    }

    public static void methodA() {
        System.out.println("Inside A");
        methodB();
    }

    public static void main(String[] args) {
        System.out.println("Starting main");
        methodA();
    }
}
```

### Task

1. What will the output be before the exception is thrown?
2. Draw the call stack at the moment the exception is thrown.
3. Write the stack trace that the JVM would print (in the standard format).

---

### ✅ Answer

#### 1. Output before the exception

```
Starting main
Inside A
Inside B
Inside C
```

#### 2. Call stack at the moment of the exception

```
┌─────────────────────────────┐  ← top (most recent)
│  methodC()                  │
├─────────────────────────────┤
│  methodB()                  │
├─────────────────────────────┤
│  methodA()                  │
├─────────────────────────────┤
│  main()                     │
└─────────────────────────────┘  ← bottom (entry point)
```

Each entry is a **stack frame** that stores local variables, the current instruction pointer, and the return address for that method call.

#### 3. Stack trace

```
Exception in thread "main" java.lang.RuntimeException: Error in C!
	at CallStackDemo.methodC(CallStackDemo.java:5)
	at CallStackDemo.methodB(CallStackDemo.java:10)
	at CallStackDemo.methodA(CallStackDemo.java:15)
	at CallStackDemo.main(CallStackDemo.java:20)
```

- The **top line** names the exception type and message.
- Each `at` line is one **stack frame**, listed from the most recent (where the exception occurred) to the oldest.
- Reading from bottom to top shows the **execution path** that led to the error.

---

## 📌 Exercise 2: Interpret a Stack Trace

### Question

A teammate shares the following stack trace from a production application:

```
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "String.length()" because "str" is null
	at com.example.StringUtils.getLength(StringUtils.java:12)
	at com.example.UserService.validateName(UserService.java:34)
	at com.example.UserController.createUser(UserController.java:57)
	at com.example.Main.main(Main.java:10)
```

### Task

Answer the following questions:

1. What exception was thrown?
2. In which class and method did the exception originate?
3. What is the likely root cause?
4. Which file and line number should you investigate first?
5. Describe the chain of calls that led to the exception.

---

### ✅ Answer

1. **Exception**: `java.lang.NullPointerException` with message `"Cannot invoke "String.length()" because "str" is null"`.
2. **Origin**: `StringUtils.getLength()` in `StringUtils.java` at line 12.
3. **Root cause**: A variable named `str` was `null` when `String.length()` was called on it. Someone passed a `null` string into `getLength`.
4. **First file to investigate**: `StringUtils.java`, line 12 – that's where the NPE was thrown. Then check `UserService.java` line 34 to see where the `null` value came from.
5. **Call chain** (oldest → newest):
   - `Main.main` called `UserController.createUser`
   - `createUser` called `UserService.validateName`
   - `validateName` called `StringUtils.getLength` with a `null` argument
   - `getLength` threw the NPE

---

## 📌 Exercise 3: StackOverflowError

### Question

```java
public class Overflow {
    public static void recurse(int n) {
        System.out.println("n = " + n);
        recurse(n + 1);
    }

    public static void main(String[] args) {
        recurse(0);
    }
}
```

### Task

1. What will eventually happen when this program runs?
2. Why does this happen in terms of the call stack?
3. How would you fix it?

---

### ✅ Answer

1. The JVM will throw a `java.lang.StackOverflowError` after printing many values of `n`.
2. Every call to `recurse` adds a **new stack frame** to the call stack. Because `recurse` never reaches a base case, frames accumulate indefinitely until the JVM exhausts the fixed-size stack memory allocated for that thread.
3. **Fix**: Add a base case that stops the recursion.

```java
public static void recurse(int n) {
    if (n >= 100) return;   // base case – stop at 100
    System.out.println("n = " + n);
    recurse(n + 1);
}
```

---

## 📌 Key Concepts Summary

| Concept | Description |
|---|---|
| **Call stack** | A LIFO (last-in, first-out) data structure that tracks active method calls for the current thread |
| **Stack frame** | A record pushed onto the call stack for each method call; contains local variables, parameters, and the return address |
| **Stack trace** | A snapshot of the call stack at the moment an exception was thrown; printed from most-recent to least-recent frame |
| **StackOverflowError** | Thrown when the call stack exceeds the JVM's allocated stack size, typically due to infinite recursion |
| **NullPointerException** | Thrown when code attempts to use a `null` reference as if it were a valid object |
