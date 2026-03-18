# 💻 Command-Line and Execution

## 🎯 Learning Objectives

- Compile and run Java programs from the command line
- Pass arguments to a Java program via `args[]`
- Understand the role of `javac`, `java`, and the classpath
- Run programs packaged in a JAR file

---

## 📌 Exercise 1: Compile and Run "Hello, World"

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

### ✅ Answer

```bash
# 1. Compile
javac Hello.java

# 2. Run
java Hello

# 3. Files produced
# Hello.class  (JVM bytecode)
```

- `javac Hello.java` invokes the **Java compiler**, which translates source code (`.java`) into **bytecode** (`.class`).
- `java Hello` starts the JVM and loads `Hello.class`, then calls the `main` method.
- You pass the **class name** (not the file name) to `java`.

---

## 📌 Exercise 2: Command-Line Arguments

### Question

Write a Java program `Greeter.java` that:

1. Reads the first command-line argument as a name.
2. Prints `"Hello, <name>!"`.
3. If no argument is provided, prints `"Hello, Stranger!"`.

### Task

Show both the source code and the commands to run it with and without an argument.

---

### ✅ Answer

```java
// Greeter.java
public class Greeter {
    public static void main(String[] args) {
        String name = args.length > 0 ? args[0] : "Stranger";
        System.out.println("Hello, " + name + "!");
    }
}
```

```bash
# Compile
javac Greeter.java

# Run with argument
java Greeter Alice
# Output: Hello, Alice!

# Run without argument
java Greeter
# Output: Hello, Stranger!
```

- `args` is a `String[]` containing all tokens passed after the class name.
- `args[0]` is the first argument; `args.length` tells you how many were provided.

---

## 📌 Exercise 3: Classpath

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

### ✅ Answer

```bash
# 1. Compile – -d specifies the output directory; javac preserves the package structure
javac -d out/ src/com/example/App.java

# 2. Run – -cp (or -classpath) points to the directory containing compiled classes
java -cp out/ com.example.App
# Output: Running from package!
```

- The `-d` flag tells `javac` where to place the compiled `.class` files.
- The `-cp` flag tells `java` where to search for `.class` files.
- You must use the **fully qualified class name** (`com.example.App`) when using packages.

---

## 📌 Exercise 4: Running a JAR File

### Question

After packaging `App` into a JAR file `app.jar`:

### Task

1. What command creates an executable JAR (with a manifest specifying the main class)?
2. What command runs it?

---

### ✅ Answer

```bash
# 1. Create the JAR
#    -c: create  -v: verbose  -f: output file  -e: entry point (main class)
jar -cvfe app.jar com.example.App -C out/ .

# 2. Run the JAR
java -jar app.jar
# Output: Running from package!
```

- The **manifest** inside the JAR records which class contains `main`.
- `-e com.example.App` sets the `Main-Class` attribute automatically.
- Anyone with the JAR file can run it on any machine with a JRE installed.

---

## 📌 Key Commands Reference

| Command | Purpose |
|---|---|
| `javac File.java` | Compile a Java source file |
| `java ClassName` | Run a compiled class |
| `java -cp <dir> pkg.ClassName` | Run with a custom classpath |
| `jar -cvfe out.jar MainClass -C dir/ .` | Package classes into a JAR |
| `java -jar out.jar` | Run an executable JAR |
| `java --version` | Print the installed JDK/JRE version |
