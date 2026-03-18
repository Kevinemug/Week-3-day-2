# 💻 Command-Line and Execution

##  Learning Objectives

- Compile and run Java programs from the command line
- Pass arguments to a Java program via `args[]`
- Understand the role of `javac`, `java`, and the classpath
- Run programs packaged in a JAR file

---

##  Exercise 1: Compile and Run "Hello, World"

### Question

Given the following source file `Hello.java`:

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### Task

1. What command compiles this file?
2. What command runs the compiled program?
3. What files are produced after compilation?

---



##  Exercise 2: Command-Line Arguments

### Question

Write a Java program `Greeter.java` that:

1. Reads the first command-line argument as a name.
2. Prints `"Hello, <name>!"`.
3. If no argument is provided, prints `"Hello, Stranger!"`.

### Task

Show both the source code and the commands to run it with and without an argument.

---



## Exercise 3: Classpath

### Question

You have the following project layout:

```
project/
├── src/
│   └── com/example/App.java
└── out/
```

`App.java`:
```java
package com.example;

public class App {
    public static void main(String[] args) {
        System.out.println("Running from package!");
    }
}
```

### Task

1. What command compiles `App.java` and places the `.class` file in `out/`?
2. What command runs `App` using `out/` as the classpath?

---


##  Exercise 4: Running a JAR File

### Question

After packaging `App` into a JAR file `app.jar`:

### Task

1. What command creates an executable JAR (with a manifest specifying the main class)?
2. What command runs it?

---



##  Key Commands Reference

| Command | Purpose |
|---|---|
| `javac File.java` | Compile a Java source file |
| `java ClassName` | Run a compiled class |
| `java -cp <dir> pkg.ClassName` | Run with a custom classpath |
| `jar -cvfe out.jar MainClass -C dir/ .` | Package classes into a JAR |
| `java -jar out.jar` | Run an executable JAR |
| `java --version` | Print the installed JDK/JRE version |
