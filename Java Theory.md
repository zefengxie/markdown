**Java theory**

## Collections Framework
The Java Collections Framework is a set of classes and interfaces that help you store, retrieve, and manipulate groups of objects efficiently.

## common collection interface 
| Interface | Description |
|----------|-------------|
|map|Ordered collection, allows duplicates|
|set|Unordered, no duplicates|
|list|Key-value pairs|
|queue|FIFO (First-In-First-Out) structure|

## ArrayList VS LinkedList 
## ArrayList
Characteristics:
* Backed by a resizable array
* **Fast random access (get(index))**  random access means you can directly access any element in a collection using its index, in constant time 
```
list.get(4);  // Instantly get the 5th element
```
* **Slower insertion/deletion in the middle (requires shifting elements)**
here is example of why it is Slow:
```
import java.util.ArrayList;

public class InsertExample {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        
        // Initial list
        list.add("A");
        list.add("B");
        list.add("D");
        list.add("E");

        System.out.println("Before insertion: " + list);

        // Insert "C" at index 2 (between B and D)
        list.add(2, "C");

        System.out.println("After insertion: " + list);
    }
}

```
What’s happening internally is: 
```
Before insertion:
[A, B, D, E]

Insert "C" at index 2:
- Move D → index 3
- Move E → index 4
- Insert C at index 2

After insertion:
[A, B, C, D, E]
```
As list size increases, more elements need to be moved, which slows down performance

---

## LinkedList
**Characteristics:**
* Implemented as a doubly linked list

*this means it uses a chain of nodes, and each node stores two links:
  * A reference to the previous node
  * A reference to the next node
  * And of course, it stores the data (value) itself

  
Fast insertions/deletions at the beginning/middle
Slower random access (must traverse node by node)





