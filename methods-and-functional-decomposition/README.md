#  Methods and Functional Decomposition

## Learning Objectives

- Understand how to break a large problem into smaller, focused methods
- Know how to pass arguments and return values
- Understand method signatures, overloading, and scope
- Apply the principle of single responsibility to methods

---

##  Exercise 1: Decompose a Shopping Cart

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




##  Exercise 2: Method Overloading

### Question

Write a class `Greeter` with **overloaded** `greet` methods:

1. `greet(String name)` → prints `"Hello, <name>!"`
2. `greet(String name, String language)` → prints a greeting in the given language (`"en"`, `"es"`, `"fr"`)
3. `greet(String name, int times)` → prints the greeting `times` times

### Task

Implement all three overloads and write a `main` method that demonstrates each one.

---



##  Exercise 3: Recursive Method

### Question

Write a recursive method `int factorial(int n)` that returns the factorial of `n`.

- `factorial(0)` → `1`
- `factorial(5)` → `120`

Identify the **base case** and **recursive case**.




