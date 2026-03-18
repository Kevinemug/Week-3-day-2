# 🔧 Methods and Functional Decomposition

## 🎯 Learning Objectives

- Understand how to break a large problem into smaller, focused methods
- Know how to pass arguments and return values
- Understand method signatures, overloading, and scope
- Apply the principle of single responsibility to methods

---

## 📌 Exercise 1: Decompose a Shopping Cart

### Question

The following code calculates a shopping cart total in a single method. Refactor it using **functional decomposition** – break it into multiple focused methods.

```java
public class ShoppingCart {
    public static void main(String[] args) {
        double[] prices = {12.99, 5.49, 8.00, 3.75};
        int[] quantities = {2, 1, 3, 5};
        double discountPercent = 10.0;

        double subtotal = 0;
        for (int i = 0; i < prices.length; i++) {
            subtotal += prices[i] * quantities[i];
        }
        double discountAmount = subtotal * (discountPercent / 100);
        double total = subtotal - discountAmount;
        double tax = total * 0.08;
        double finalTotal = total + tax;

        System.out.printf("Final total: $%.2f%n", finalTotal);
    }
}
```

### Task

1. Extract at least **four** separate methods:
   - `calculateSubtotal(double[] prices, int[] quantities)` → returns `double`
   - `applyDiscount(double subtotal, double discountPercent)` → returns `double`
   - `calculateTax(double amount, double taxRate)` → returns `double`
   - `printReceipt(double subtotal, double discount, double tax, double finalTotal)` → returns `void`
2. Rewrite `main` to call these methods in sequence.
3. Explain why decomposing code this way improves **readability**, **testability**, and **reusability**.

---

### ✅ Answer

```java
public class ShoppingCart {

    public static double calculateSubtotal(double[] prices, int[] quantities) {
        double subtotal = 0;
        for (int i = 0; i < prices.length; i++) {
            subtotal += prices[i] * quantities[i];
        }
        return subtotal;
    }

    public static double applyDiscount(double subtotal, double discountPercent) {
        return subtotal - (subtotal * (discountPercent / 100));
    }

    public static double calculateTax(double amount, double taxRate) {
        return amount * (taxRate / 100);
    }

    public static void printReceipt(double subtotal, double discount, double tax, double finalTotal) {
        System.out.printf("Subtotal:     $%.2f%n", subtotal);
        System.out.printf("Discount:    -$%.2f%n", discount);
        System.out.printf("Tax:          $%.2f%n", tax);
        System.out.printf("Final Total:  $%.2f%n", finalTotal);
    }

    public static void main(String[] args) {
        double[] prices = {12.99, 5.49, 8.00, 3.75};
        int[] quantities = {2, 1, 3, 5};
        double discountPercent = 10.0;
        double taxRate = 8.0;

        double subtotal = calculateSubtotal(prices, quantities);
        double afterDiscount = applyDiscount(subtotal, discountPercent);
        double discountAmount = subtotal - afterDiscount;
        double tax = calculateTax(afterDiscount, taxRate);
        double finalTotal = afterDiscount + tax;

        printReceipt(subtotal, discountAmount, tax, finalTotal);
    }
}
```

**Why decompose?**
- **Readability**: Each method has a clear, single purpose – easier to understand at a glance.
- **Testability**: Each method can be tested independently (e.g., test `calculateTax` with known inputs).
- **Reusability**: `calculateTax` can be used anywhere tax needs to be computed, without copy-pasting logic.

---

## 📌 Exercise 2: Method Overloading

### Question

Write a class `Greeter` with **overloaded** `greet` methods:

1. `greet(String name)` → prints `"Hello, <name>!"`
2. `greet(String name, String language)` → prints a greeting in the given language (`"en"`, `"es"`, `"fr"`)
3. `greet(String name, int times)` → prints the greeting `times` times

### Task

Implement all three overloads and write a `main` method that demonstrates each one.

---

### ✅ Answer

```java
public class Greeter {

    public static void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public static void greet(String name, String language) {
        switch (language) {
            case "es": System.out.println("¡Hola, " + name + "!"); break;
            case "fr": System.out.println("Bonjour, " + name + "!"); break;
            default:   System.out.println("Hello, " + name + "!");
        }
    }

    public static void greet(String name, int times) {
        for (int i = 0; i < times; i++) {
            System.out.println("Hello, " + name + "!");
        }
    }

    public static void main(String[] args) {
        greet("Alice");
        greet("Carlos", "es");
        greet("Marie", "fr");
        greet("Bob", 3);
    }
}
```

---

## 📌 Exercise 3: Recursive Method

### Question

Write a recursive method `int factorial(int n)` that returns the factorial of `n`.

- `factorial(0)` → `1`
- `factorial(5)` → `120`

Identify the **base case** and **recursive case**.

---

### ✅ Answer

```java
public class MathUtils {

    public static int factorial(int n) {
        if (n == 0) return 1;          // base case
        return n * factorial(n - 1);   // recursive case
    }

    public static void main(String[] args) {
        System.out.println(factorial(0)); // 1
        System.out.println(factorial(5)); // 120
    }
}
```

**Base case**: `n == 0` – stops the recursion.  
**Recursive case**: `n * factorial(n - 1)` – reduces the problem towards the base case.
