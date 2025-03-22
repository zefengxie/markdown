# Full Guide to `Set` in Java

## What is a `Set`?

- A **Set** is a **collection that cannot contain duplicate elements**.
- It models the mathematical concept of a **set** (just like in math: `{1, 2, 3}`).
- **No indexing** — unlike `List`, you can’t do `set.get(0)`.

## Why use a `Set`?

- To store **unique items**
- To **quickly check if an element exists**
- To **remove duplicates** from a list

## Key Interfaces and Classes

Java provides different `Set` types for different use cases:

| Implementation | Order | Duplicate Allowed? | Null Allowed? | Auto-sorting? |
|----------------|-------|--------------------|---------------|---------------|
| `HashSet` | ❌ No order | ❌ No | ✅ One `null` | ❌ No |
| `LinkedHashSet` | ✅ Insertion order | ❌ No | ✅ One `null` | ❌ No |
| `TreeSet` | ✅ Sorted (natural order or comparator) | ❌ No | ❌ No | ✅ Yes |

**no duplicate means if there is duplicate item added, set will only store the first one**
example:
```
Set<String> set = new HashSet<>();
set.add("Java");
set.add("Java"); // ❌ second time will be ignored

System.out.println(set); // [Java] instead of [java, java]

```

## 1. `HashSet` — Fastest, Unordered

### Features:

- Based on **hashing**
- No guarantee of order
- Very fast for `add()`, `remove()`, `contains()`
  **Hashing is a technique that turns data (like a string or a number) into a unique fixed-size value called a hash code**
  This hash code is used to determine where to store the data in memory, like an index in an internal array.
  Example: Hashing in HashSet
```
HashSet<String> set = new HashSet<>();
set.add("Apple");
```
Behind the scenes:

**Java calls "Apple".hashCode() → gets a number like 63062875**
  
  Java maps that hash code to an array index
  It stores "Apple" at that index in the internal structure
  So when you later do:
```
set.contains("Apple");
```
Java doesn't check every item. It just:

  Gets "Apple"’s hash code again
  Goes directly to the bucket (array slot) where it should be
  Checks if "Apple" is there


### Example:

```java
import java.util.HashSet;

public class HashSetExample {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        set.add("Java");
        set.add("Python");
        set.add("Java"); // Duplicate

        System.out.println(set); // [Java, Python] or [Python, Java]
    }
}
```

## 2. `LinkedHashSet` — Keeps Insertion Order

### Features:

- Keeps elements in the **order you added them**
- Slower than `HashSet`, but predictable iteration

### Example:

```java
import java.util.LinkedHashSet;

public class LinkedHashSetExample {
    public static void main(String[] args) {
        LinkedHashSet<String> set = new LinkedHashSet<>();
        set.add("Apple");
        set.add("Banana");
        set.add("Orange");

        System.out.println(set); // [Apple, Banana, Orange]
    }
}
```

## 3. `TreeSet` — Automatically Sorted

### Features:

- Elements are stored in **sorted order**
- No `null` allowed (throws `NullPointerException`)
- Under the hood, it uses a **Red-Black Tree**

### Example:

```java
import java.util.TreeSet;

public class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(30);
        set.add(10);
        set.add(20);

        System.out.println(set); // [10, 20, 30]
    }
}
```

### Custom Sorting (with Comparator):

```java
import java.util.*;

public class CustomSortSet {
    public static void main(String[] args) {
        TreeSet<String> set = new TreeSet<>(Comparator.reverseOrder());
        set.add("C");
        set.add("A");
        set.add("B");

        System.out.println(set); // [C, B, A]
    }
}
```

## Common Set Operations

```java
Set<String> set = new HashSet<>();
set.add("A");
set.remove("A");
set.contains("A"); // true / false
set.size();
set.isEmpty();
set.clear(); // removes all elements
```

## When to Use Which?

| Scenario | Best Set Type |
|----------|----------------|
| Fast lookups, don't care about order | `HashSet` |
| Maintain insertion order | `LinkedHashSet` |
| Need sorted data | `TreeSet` |

## Real-World Use Cases

### Remove duplicates from a list:

```java
List<String> list = Arrays.asList("A", "B", "A", "C");
Set<String> set = new HashSet<>(list); // removes duplicates
```

### Check if a string has all unique characters:

```java
public static boolean isUnique(String str) {
    Set<Character> set = new HashSet<>();
    for (char c : str.toCharArray()) {
        if (!set.add(c)) return false;
    }
    return true;
}
```

## Summary

| Feature | HashSet | LinkedHashSet | TreeSet |
|--------|---------|---------------|---------|
| Allows duplicates? | ❌ | ❌ | ❌ |
| Maintains order? | ❌ | ✅ Insertion order | ✅ Sorted |
| Allows null? | ✅ One | ✅ One | ❌ No |
| Performance | 🔥 Fast | 🐢 Medium | 🐢 Slow (due to sorting) |
