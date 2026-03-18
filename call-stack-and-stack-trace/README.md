#  Call Stack and Stack Trace

## Learning Objectives

- Understand what the call stack is and how the JVM uses it
- Read and interpret a Java stack trace
- Identify the origin of an exception from a stack trace
- Understand stack frames and method invocation order

---

##  Exercise 1: Trace the Call Stack

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


##  Exercise 3: StackOverflowError

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



---

##  Key Concepts Summary

| Concept | Description |
|---|---|
| **Call stack** | A LIFO (last-in, first-out) data structure that tracks active method calls for the current thread |
| **Stack frame** | A record pushed onto the call stack for each method call; contains local variables, parameters, and the return address |
| **Stack trace** | A snapshot of the call stack at the moment an exception was thrown; printed from most-recent to least-recent frame |
| **StackOverflowError** | Thrown when the call stack exceeds the JVM's allocated stack size, typically due to infinite recursion |
| **NullPointerException** | Thrown when code attempts to use a `null` reference as if it were a valid object |
