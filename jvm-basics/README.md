#  JVM Basics

##  Learning Objectives

- Understand how the JVM manages memory (heap, stack, string pool)
- Distinguish between reference equality (`==`) and value equality (`.equals()`)
- Understand how String literals and `new String(...)` differ in memory placement
- Understand what `intern()` does

---

##  Exercise: JVM Deep Dive – String Pool, Heap, and References

### Question

Consider the following Java code:

```java
public class Test {
    public static void main(String[] args) {
        String a = new String("hello");
        String b = "hello";
        String c = "hello";
        String d = new String("hello").intern();

        System.out.println(a == b);
        System.out.println(b == c);
        System.out.println(b == d);
        System.out.println(a.equals(b));
    }
}
```

### Task

Explain the output of each `System.out.println` line and describe what is happening inside the JVM in terms of:

1. **Heap memory** – where objects live
2. **String pool** – how string literals are stored and reused
3. **Object creation** – how many `String` objects are created and where
4. **References** – what each variable points to
5. **`==` vs `.equals()`** – the difference between reference and value equality
6. **What `intern()` actually does**

---

##  Answer

### Output

```
false
true
true
true
```

---

### Line-by-line Explanation

#### `String a = new String("hello");`

- The keyword `new` **forces** the JVM to allocate a **brand-new `String` object on the heap**, even if an identical value already exists in the string pool.
- The literal `"hello"` inside the constructor causes the JVM to also place (or reuse) `"hello"` in the **string pool**.
- Result: `a` points to a **new heap object** that is *separate* from the string pool entry.

#### `String b = "hello";`

- String literals are handled by the JVM's **string pool**, which resides in the **main heap** (since Java 7). Prior to Java 7 it was stored in `PermGen`; `Metaspace` replaced `PermGen` in Java 8 but only stores class metadata, not the string pool.
- Because `"hello"` already exists in the pool (placed when `a` was created), `b` simply **receives a reference to that existing pool object**.

#### `String c = "hello";`

- Same literal `"hello"` – the JVM looks up the pool, finds the existing entry, and assigns the **same reference** to `c`.
- `b` and `c` now point to the **exact same object**.

#### `String d = new String("hello").intern();`

- `new String("hello")` creates yet another **new heap object** (not in the pool).
- `.intern()` tells the JVM: *"Return the canonical (pool) reference for this value."*
- The JVM looks up `"hello"` in the string pool, finds it, and returns **that pool reference**.
- So `d` ends up pointing to **the same object** as `b` and `c`.

---



### `==` vs `.equals()`

| Expression     | What it compares         | Result  | Why                                                               |
|----------------|--------------------------|---------|-------------------------------------------------------------------|
| `a == b`       | Reference (memory address) | `false` | `a` is a heap object; `b` is the pool object – different addresses |
| `b == c`       | Reference (memory address) | `true`  | Both point to the same pool entry                                 |
| `b == d`       | Reference (memory address) | `true`  | `intern()` returned the pool reference, same as `b`              |
| `a.equals(b)`  | Value (character content)  | `true`  | Both contain `"hello"` – `String.equals()` compares content       |

---

### Key Takeaways

| Concept | Summary |
|---|---|
| **String pool** | A cache of string literals maintained by the JVM to save memory. Strings created with literals (`"..."`) are automatically interned. |
| **`new String(...)`** | Always creates a new heap object, bypassing the pool. |
| **`intern()`** | Returns the pool's canonical reference for the string's value. Subsequent `==` comparisons with pool literals will return `true`. |
| **`==`** | Compares **object identity** (memory addresses) for objects – do both variables point to the *same* object? |
| **`.equals()`** | Compares **value/content** – do both strings contain the same characters? |
| **Heap** | The general-purpose memory area where all Java objects (including `new String(...)`) are allocated. |
