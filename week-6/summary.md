<img src="https://javaiitmbs.github.io/assets/logo.png" width=30% />
<style> 
img {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style> 
<hr>
<span style="display:flex; justify-content: space-between;">
    <a href="https://javaiitmbs.github.io/week-5/summary.html">Week 5</a>
    <a href="https://javaiitmbs.github.io/index.html">Home</a>
    <a href="https://javaiitmbs.github.io/week-7/summary.html">Week 7</a>
</span> 
<hr>

## The Benefits of Indirection - Separating Interface from Implementation

Abstract Data Types (ADTs) separate the public interface from private implementation, promoting reusability. For example, a generic **Queue** typically includes methods like:

- `add(element)`: Adds an element to the queue.
- `remove()`: Removes an element.
- `size()`: Returns the queue size

Common implementations include:

- **Circular Array**: Fixed-size array where elements wrap around when reaching the end.
- **Linked List**: Dynamic structure where each node links to the next.

### Trade-offs Between Implementations

- **Circular Array:**
  - Efficient due to fixed storage.
  - Limited by initial size.

- **Linked List:**
  - Flexible and grows dynamically.
  - Higher memory overhead.

### Multiple Implementations

Developers may offer separate implementations to suit various needs:

```java
CircularArrayQueue<Date> dateQueue = new CircularArrayQueue<>();
LinkedListQueue<String> stringQueue = new LinkedListQueue<>();
```

However, changing `dateQueue` to a flexible type requires updating all references in the code, which can be error-prone.

### Using Interfaces for Flexibility

Using an interface abstracts implementation details and improves flexibility:

```java
public interface Queue<E> {
    void add(E element);
    E remove();
    int size();
}
public class CircularArrayQueue<E> implements Queue<E> {
    public void add(E element) { /* Implementation */ }
    public E remove() { /* Implementation */ }
    public int size() { /* Implementation */ }
}
public class LinkedListQueue<E> implements Queue<E> {
    public void add(E element) { /* Implementation */ }
    public E remove() { /* Implementation */ }
    public int size() { /* Implementation */ }
}
```

Declare variables using the interface, deferring implementation decisions to instantiation:

```java
Queue<Date> dateQueue = new CircularArrayQueue<>();
Queue<String> stringQueue = new LinkedListQueue<>();
```

Here, Switching implementations require only updating the instantiation as compared to previous implementation.

### Real-Life Analogy

An organization providing office cars to staff can:

- **Concrete**: Assign specific cars to individuals (rigid and inflexible).
- **Indirection**: Maintain a car pool or contract taxis, offering flexibility regardless of specific vehicle issues.

## Collections in Programming Languages

Many programming languages provide built-in data structures for managing grouped data, such as arrays, lists, dictionaries, and sets. In Python, these structures are well-integrated into the language. Similarly, Java offers a variety of data structures, which have evolved significantly over time.

### Early Data Structures in Java

When Java was first introduced, it came with several standalone data structures that were widely useful. These included:

- Vector
- Stack
- Hashtable
- BitSet

However, these early data structures lacked a unified framework. This absence of a common interface led to several challenges:

1. **Code Maintenance**: Switching from one data structure to another often required substantial changes throughout the codebase.

2. **Inconsistency**: Each data structure had its own methods, syntax, and constructors, with no standardization.

3. **Usability Issues**: Developers had to remember the unique functionalities of each collection, making it harder to achieve consistency and reusability in their code.

The fragmented nature of these collections underscored the need for a unified framework to standardize collection operations, improve usability, and promote code reusability.

---

### The `Collection` interface

To address these issues, Java introduced the Collection Framework, a unified architecture for working with data structures. This framework provides:

- **Abstraction**: Common behaviors of grouped data are abstracted through interfaces.

- **Standardization**: A consistent and predictable API for working with collections.

- **Extensibility**: The ability to add custom implementations with minimal effort.

#### Key Interfaces in the Collection Framework

The framework revolves around several core interfaces:

1. **Collection**: The root interface for all non-key-value data structures.

2. **List**: Represents an ordered collection.
   - Examples: `ArrayList`, `LinkedList`

3. **Set**: Represents a collection with unique elements.
   - Examples: `HashSet`, `LinkedHashSet`

4. **Queue**: Represents a collection designed for holding elements prior to processing.
   - Examples: `PriorityQueue`

Each interface defines specific behaviors and is implemented by concrete classes.

#### Methods in Collection interface

The Collection interface provides a variety of methods to perform common operations:

- **Adding Elements:**
  - `public boolean add(E e)` - Inserts an element into the collection.

- **Iteration:**
  - `Iterator<E> iterator()` - Returns an iterator to traverse the collection.

- **Size and Emptiness:**
  - `int size()` - Returns the number of elements in the collection.
  - `boolean isEmpty()` - Checks if the collection is empty.

- **Containment Checks:**
  - `boolean contains(Object obj)` - Checks if the collection contains a specified element.
  - `boolean containsAll(Collection<?> c)` - Checks if the collection contains all elements of another collection.

- **Equality and Additions:**
  - `boolean equals(Object other)` - Compares the collection with another object for equality.
  - `boolean addAll(Collection<? extends E> from)` - Adds all elements from another collection.

- **Removing Elements:**
  - `boolean remove(Object obj)` - Removes a specified element.
  - `boolean removeAll(Collection<?> c)` - Removes all elements contained in another collection.

While these methods offer extensive functionality, implementing all of them in a concrete collection class can be labor-intensive.

The ideal solution might be to provide default implementations in the interface but this feature (of providing default implementations in interface) was added to JAVA later.

The other way is to provide concrete implementations through an abstract class.

#### Abstract Implementations

To simplify implementation, Java provides the `AbstractCollection` class, which implements the `Collection` interface. This abstract class offers default implementations for many methods, allowing developers to focus on specific behaviors for their custom collections.

In this abstract class some implementations remain abstract while others can be extended.

---

### The `Iterator` Interface

The `Iterator` interface, located above `Collection` in the hierarchy, facilitates systematic traversal of collections. Its primary methods include:

- `public boolean hasNext()` - Checks if there are more elements to iterate over.

- `public E next()` - Returns the next element in the iteration.

- `public void remove()` - Removes the last element accessed by the iterator.

The iterator's `remove()` method is distinct from the Collection interface's `remove()` method. While the former removes the current element accessed via `next()`, the latter removes a specified object directly from the collection.

**Code Example:**

```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
public class BasicIteratorExample {
    public static void main(String[] args) {

        // Create a collection of strings
        Collection<String> words = new ArrayList<>();

        // Add elements to the collection
        words.add("Hello");
        words.add("World");
        words.add("Welcome");
        words.add("To");
        words.add("Java");

        // Create an Iterator to traverse the collection
        Iterator<String> iterator = words.iterator();

        System.out.println("Iterating over the collection:");
        // Use the Iterator to traverse and print each element
        while (iterator.hasNext()) {
            String word = iterator.next();
            System.out.println(word); // Print each element
        }
    }
}
```

**Output:**

```
Iterating over the collection:
Hello
World
Welcome
To
Java
```

- The `hasNext()` method checks if there are more elements to iterate over.
- The `next()` method retrieves the next element in the collection.

#### Enhanced `for` Loop (For-Each Loop)

Java introduced the enhanced `for` loop, which simplifies iteration by implicitly using an iterator. This feature reduces boilerplate code and improves readability.

**Code Example:**

```java
import java.util.ArrayList;
import java.util.Collection;
public class EnhancedForLoopExample {
    public static void main(String[] args) {

        // Create a collection of strings
        Collection<String> words = new ArrayList<>();

        // Add elements to the collection
        words.add("Hello");
        words.add("World");
        words.add("Welcome");
        words.add("To");
        words.add("Java");

        System.out.println("Using enhanced for loop:");
        // Use enhanced for loop to iterate over the collection
        for (String word : words) {
            System.out.println(word); // Print each element
        }
    }
}
```

**Output:**

```
Using enhanced for loop:
Hello
World
Welcome
To
Java
```

- The enhanced for loop simplifies iteration and implicitly uses an `Iterator`.

Since having iterator functionality also provides flexibility to write generic functions to operate on collections.

#### Removing elements from Iterator

The `remove()` method of the Iterator interface has specific behavior:

- It removes the last element accessed by `next()`.

- Consecutive calls to `remove()` require an intermediate call to `next()`. Failing to do so results in an `IllegalStateException`, as there is no "current element" to remove after the first `remove()` call.

**Code Example:**

```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
public class IteratorRemoveExample {
    public static void main(String[] args) {

        // Create a collection of strings
        Collection<String> words = new ArrayList<>();
        // Add elements to the collection
        words.add("Hello");
        words.add("World");
        words.add("Welcome");
        words.add("To");
        words.add("Java");

        // Create an Iterator to traverse the collection
        Iterator<String> iterator = words.iterator();
        System.out.println("Original collection:");
        for (String word : words) {
            System.out.println(word);
        }

        System.out.println("\nRemoving elements starting with 'W':"); // Use the Iterator to remove elements

        while (iterator.hasNext()) {
            String word = iterator.next();
            if (word.startsWith("W") || word.startsWith("w")) {
                iterator.remove();
                System.out.println("Removed: " + word);
            }
        }

        System.out.println("\nUpdated collection:");
        for (String word : words) {
            System.out.println(word);
        }
    }
}
```

**Output:**

```
Original collection:
Hello
World
Welcome
To
Java

Removing elements starting with 'W':
Removed: World
Removed: Welcome

Updated collection:
Hello
To
Java
```

- The `remove()` method removes the last element returned by `next()`.

- Calling `remove()` directly without invoking `next()` leads to an
  IllegalStateException.

#### Implications of Using `Iterator.remove()`

The `remove()` method of the `Iterator` interface has specific implications when attempting to remove consecutive elements from a collection. To achieve this, you must interleave calls to the `next()` method between successive `remove()` calls.

For example, if you need to remove two consecutive elements from a collection, you cannot directly call `remove()` twice in succession. The first `remove()` call removes the last element returned by `next()`. If you attempt a second `remove()` call without invoking `next()`, it results in an error because there is no "current element" for the iterator to act upon. The absence of an intermediate `next()` leaves the iterator in an invalid state.

**Code Example:**

```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
public class ConsecutiveRemoveExample {
    public static void main(String[] args) {

        // Create a collection of strings
        Collection<String> words = new ArrayList<>();

        // Add elements to the collection
        words.add("Hello");
        words.add("World");
        words.add("Welcome");

        // Create an Iterator to traverse the collection
        Iterator<String> iterator = words.iterator();
        System.out.println("Attempting consecutive remove calls:");

        try {
            while (iterator.hasNext()) {
            String word = iterator.next();

            if (word.startsWith("W") || word.startsWith("w")) {
                iterator.remove(); // First remove call
                iterator.remove(); // Second remove call (ERROR)
                System.out.println("Removed: " + word);
                }
            }

        } catch (IllegalStateException e) {
            System.err.println("Error: " + e.getMessage());
        }
    }
}
```

**Output:**

```
Attempting consecutive remove calls:
Error: remove() can only be called once per call to next()
```

- After calling `remove()`, the iterator's state requires an intermediate `next()` call to establish a "current element" before another `remove()`.

#### Distinction Between `remove()` in Iterator and Collection

It is important to note that the `remove()` method in the `Iterator` interface is distinct from the `remove()` method in the `Collection` interface, differing in both their signatures and behaviors:

1. `Iterator.remove()`

- **Signature**: `void remove()`
- **Behavior**: Removes the last element returned by the `next()` method. It operates on the current position of the iterator within the collection.

2. `Collection.remove()`

- **Signature**: `boolean remove(Object obj)`
- **Behavior**: Removes a specified object from the collection. If the object exists within the collection, it is removed, and the method returns true; otherwise, it returns false.

The `Iterator.remove()` method focuses on the element currently being traversed by the iterator, while the `Collection.remove()` method is used to directly target a specific object in the collection, independent of iteration.

## Concrete Collections

Collections are further organized based on additional properties. These captured by interfaces

- **Interface List** - For ordered collections
- **Interface Set** - For collection without duplicates.
- **Interface Queue** - For ordered collections with constraints on addition and deletion.

### The `List` Interface - Ordered Collections in Java

#### Accessing Elements in Ordered Collections

An ordered collection can be accessed in two primary ways:

1. **Through an Iterator**: Traverses the collection sequentially.

2. **By Position (Random Access)**: Directly retrieves or manipulates an element at a specified index.

#### Additional Functions for Random Access

The `ListIterator` interface, which extends `Iterator`, provides additional functionalities specific to ordered collections:

- `void add(E element)`: Inserts an element before the current index.

- `void previous()`: Moves to the previous element in the sequence.

- `boolean hasPrevious()`: Checks if there are elements before the current position.

#### The List Interface and Random Access

The `List` interface in Java supports both sequential and random access. However, the efficiency of random access varies depending on the implementation:

- **Array-based Implementations**: Compute the location of an element at index `i` using arithmetic operations. This makes random access efficient.

- **Linked List Implementations**: Require traversal of `i` links to reach the ith element, which makes random access less efficient.

The `List` interface includes the following methods for random access:

- `void add(int index, E element)` - Inserts the specified element at the specified position in this list.

- `E get(int index)` - Returns the element at the specified position in this list.

- `ListIterator<E> listIterator()` - Returns a list iterator over the elements in this list (in proper sequence), starting at the specified position in the list.

- `void remove(int index)` - Removes the element at the specified position in this list.

- `E set(int index, E element)` - Replaces the element at the specified position in this list with the specified element.

#### The Tagging Interface `RandomAccess`

The `RandomAccess` interface is a marker interface used to indicate that a List supports efficient random access. Algorithms can adapt their strategy based on whether a list implements `RandomAccess`.

For instance, swapping two elements in a list can be optimized if the list supports efficient random access:

```java
if (list instanceof RandomAccess) {
    // Efficient random access strategy
} else {
    // Sequential access strategy
}
```

#### Abstract Implementations of the List Interface

Java provides abstract classes to simplify the implementation of the `List` interface:

1. `AbstractList`:
   - Extends `AbstractCollection`.
   - Provides default implementations for `List` methods.

2. `AbstractSequentialList`:
   - Extends `AbstractList`.
   - Specifically for lists that do not support random access.

#### Concrete Implementations of Lists

- `LinkedList<E>`
  - Extends `AbstractSequentialList`.
  - Internal Structure: A doubly linked list.
  - Characteristics:
    - Efficient for inserting and removing elements at arbitrary positions.
    - Inefficient for random access due to traversal requirements.

- `ArrayList<E>`
  - Extends `AbstractList`.
  - Internal Structure: A dynamically resizable array.
  - Characteristics:
    - Supports efficient random access.
    - Handles dynamic resizing by allocating larger arrays as needed.

---

#### Practical Considerations

##### Using Random Access Methods

While the `List` interface guarantees the availability of random access methods like `get(int index)`, their efficiency depends on the underlying implementation:

- **ArrayList**: Efficient.
- **LinkedList**: Each `get()` call may require traversal from the beginning, leading to poor performance in loops.

**Code Example:**

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
public class ListAccessExample {
    public static void main(String[] args) {

        // ArrayList: Supports efficient random access
        List<String> arrayList = new ArrayList<>();

        arrayList.add("Java");
        arrayList.add("Python");
        arrayList.add("C++");

        // LinkedList: Inefficient random access
        List<String> linkedList = new LinkedList<>(arrayList);
        System.out.println("Accessing elements by index (ArrayList):");

        for (int i = 0; i < arrayList.size(); i++) {
            System.out.println("Index " + i + ": " + arrayList.get(i));
        }

        System.out.println("\nAccessing elements by index (LinkedList):");

        for (int i = 0; i < linkedList.size(); i++) {
            System.out.println("Index " + i + ": " + linkedList.get(i)); // Traverses for each call
        }
    }
}
```

**Output:**

```
Accessing elements by index (ArrayList):
Index 0: Java
Index 1: Python
Index 2: C++

Accessing elements by index (LinkedList):
Index 0: Java
Index 1: Python
Index 2: C++
```

**ArrayList** provides efficient random access using arithmetic operations.

**LinkedList** requires traversal for each `get()` call, which is less efficient.

#### Differentiating `add()` Methods

The `add()` method behaves differently depending on the context:

1. In `Collection`:

- Appends an element to the end of the collection.
- Returns a boolean indicating success.

2. In `ListIterator`:

- Inserts an element before the current position.
- Returns void as insertion always succeeds.

**Code Example:**

```java
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;
public class AddMethodExample {
    public static void main(String[] args) {

    // Using the List's add method
    List<String> list = new ArrayList<>();
    list.add("End"); // Appends to the end

    System.out.println("List after appending: " + list);


    // Using the ListIterator's add method
    ListIterator<String> iterator = list.listIterator();
    iterator.add("Insert"); // Inserts before the current position
    System.out.println("List after inserting: " + list);
    }
}
```

**Output:**

```
List after appending: [End]
List after inserting: [Insert, End]
```

- `list.add(E element)` appends elements to the end.

- `iterator.add(E element)` inserts elements before the current iterator position.

#### Summary - The `List` Interface

The `List` interface provides versatile mechanisms for managing ordered collections. Understanding the characteristics and performance implications of specific implementations like `LinkedList` and `ArrayList` is crucial for effective use. Additionally, the `ListIterator` interface extends functionality for navigating and modifying lists, while the `RandomAccess` marker interface informs the efficiency of direct access operations.

### The `Set` Interface

A `Set` in Java is a collection that ensures **no duplicate elements**. While the `Set` interface shares the same method signatures as the Collection interface, its behavior is more constrained:

- `add(E element)`: Adds an element to the set. If the element already exists, the operation has no effect and returns `false`.

- `equals(Object o)`: Determines equality by comparing contents without considering the order.

#### Purpose of the `Set` Interface

The existence of the `Set` interface alongside `Collection`, despite their shared methods, is to enforce additional constraints. By explicitly requiring a `Set`, programmers can ensure:

- No duplicate elements.
- Efficient membership checks.

#### Efficient Membership Tests in Sets

Unlike ordered collections that require iteration to locate an element, sets optimize membership testing by:

1. **Hashing**: Mapping values to positions using a **hash function**.

2. **Balanced Search Trees**: Structuring values in a hierarchical manner for efficient lookup.

#### AbstractSet and Concrete Implementations

The `AbstractSet` class provides default implementations for many `Set` methods. Concrete implementations of `Set` extend `AbstractSet` and offer optimized storage and retrieval mechanisms. As similar to the `AbstractCollection` class for `Collection` interface.

#### Concrete Implementations of Sets

1. `HashSet`

The `HashSet` class uses a hash table for storage.

- **Underlying Structure**: An array where values are mapped to positions using a hash function h(v).

- **Handling Collisions**: If two values map to the same position, the collision is resolved using strategies like probing or chaining.

- **Membership Testing**: Fast. The hash function determines the position, and membership is checked directly.

- **Order**: Unordered. The iterator visits elements in an arbitrary order but guarantees that each element is visited exactly once.

**Code Example:** Adding Elements and Ensuring Uniqueness

```java
import java.util.HashSet;
import java.util.Set;
public class SetExample {
    public static void main(String[] args) {

        // Create a HashSet
        Set<String> set = new HashSet<>();

        // Add elements to the set
        set.add("Java");
        set.add("Python");
        set.add("C++");
        set.add("Java"); // Duplicate element

        // Print the set
        System.out.println("Set contents: " + set);
    }
}
```

**Output:**

```
Set contents: [Java, Python, C++]
```

- **No Duplicates**: Adding "Java" twice results in only one instance in the set.

- **Order Not Guaranteed**: The order of elements in a HashSet is arbitrary.

**Code Example:** Membership Testing with `HashSet`

```java
import java.util.HashSet;
import java.util.Set;
public class HashSetMembership {
    public static void main(String[] args) {

        Set<Integer> set = new HashSet<>();
        set.add(5);
        set.add(10);
        set.add(15);

        // Membership test
        System.out.println("Set contains 10: " + set.contains(10)); // True
        System.out.println("Set contains 20: " + set.contains(20)); // False
    }
}
```

**Output:**

```
Set contains 10: true
Set contains 20: false
```

- **Efficient Membership Testing**: Hashing allows constant-time complexity for `contains`.

2. `TreeSet`

The `TreeSet` class uses a **balanced search tree** to store elements.

- **Underlying Structure**: A binary search tree that maintains elements in sorted order.

- **Membership Testing**: Efficient, with a time complexity `O(log n)` of for `n` elements.

- **Order:** Sorted. The iterator visits elements in ascending order.

- **Use Case**: When an ordered set is required.

**Code Example:**

```java
import java.util.TreeSet;
public class TreeSetExample {
    public static void main(String[] args) {

        TreeSet<Integer> treeSet = new TreeSet<>();

        // Add elements
        treeSet.add(20);
        treeSet.add(10);
        treeSet.add(30);

        // Print elements in sorted order
        System.out.println("TreeSet contents (sorted): " + treeSet);

        // Membership test
        System.out.println("TreeSet contains 20: " + treeSet.contains(20));
    }
}
```

**Output:**

```
TreeSet contents (sorted): [10, 20, 30]
TreeSet contains 20: true
```

- **Sorted Order**: Elements are stored in ascending order using a balanced binary search tree.

- **Efficient Membership Testing**: The `TreeSet` provides `O(log n)` complexity for lookup operations.

#### Summary - The `Set` Interface

The `Set` interface in Java provides a powerful abstraction for collections without duplicates. While `HashSet` is optimized for fast membership testing and insertion, `TreeSet` is suited for scenarios requiring sorted order. Understanding the trade-offs between these implementations is essential for writing efficient and effective code.

| **Implementation** |  **Underlying Structure**   |    **Order**     | **Membership Test Complexity** |                 **Use Case**                 |
| :----------------- | :-------------------------: | :--------------: | :----------------------------: | :------------------------------------------: | -------------------------------------- |
| `HashSet`          |         Hash Table          |    Unordered     |              True              |                    `O(1)`                    | Fast membership testing and insertion. |
| `TreeSet`          | Balanced Binary Search Tree | Sorted (natural) |           `O(logn)`            |        When sorted order is required.        |
| `LinkedHashSet`    |   Hash Table + LinkedList   | Insertion Order  |             `O(1)`             | When insertion order needs to be maintained. |

### The `Queue` Interface

A `Queue` in Java represents an **ordered collection** designed for holding elements prior to processing. Elements in a queue follow a **First-In-First-Out (FIFO)** order, meaning elements are inserted at the rear and removed from the front.

#### Core Methods in Queue Interface

The `Queue` interface includes basic operations for adding and removing elements:

- `boolean add(E element)`: Inserts the specified element into the queue. If the queue is full, this method throws an exception.

- `E remove()`: Removes and returns the head of the queue. Throws an exception if the queue is empty.

#### Gentler Variants

To handle errors more gracefully, the `Queue` interface provides alternatives:

- `boolean offer(E element)`: Attempts to insert the specified element. Returns `false` if the queue is full.

- `E poll()`: Retrieves and removes the head of the queue, returning `null` if the queue is empty.

#### Inspecting the Head

The following methods allow inspection of the element at the head without removal:

- `E element()`: Returns the head of the queue but throws an exception if the queue is empty.
- `E peek()`: Returns the head of the queue or `null` if the queue is empty.

**Code Example:**

```java
import java.util.LinkedList;
import java.util.Queue;
public class QueueExample {
    public static void main(String[] args) {

        // Create a Queue using LinkedList
        Queue<String> queue = new LinkedList<>();

        // Core Methods
        System.out.println("Adding elements using add():");
        queue.add("A");
        queue.add("B");
        queue.add("C");

        System.out.println("Queue after additions: " + queue);
        System.out.println("\nRemoving elements using remove():");
        System.out.println("Removed element: " + queue.remove());
        System.out.println("Queue after removal: " + queue);

        // Gentler Variants
        System.out.println("\nAdding elements using offer():");
        boolean offerStatus = queue.offer("D");
        System.out.println("Offer status: " + offerStatus);
        System.out.println("Queue after offer: " + queue);

        System.out.println("\nPolling elements using poll():");
        String polledElement = queue.poll();
        System.out.println("Polled element: " + polledElement);
        System.out.println("Queue after poll: " + queue);

        // Inspecting the Head
        System.out.println("\nInspecting the head using element():");

        try {
            System.out.println("Head of the queue: " + queue.element());
        } catch (Exception e) {
            System.out.println("Exception: " + e.getMessage());
        }

        System.out.println("\nInspecting the head using peek():");
        String head = queue.peek();
        System.out.println("Head of the queue (peek): " + (head != null ? head : "Queue is empty"));
    }
}
```

**Output:**

```
Adding elements using add():
Queue after additions: [A, B, C]

Removing elements using remove():
Removed element: A
Queue after removal: [B, C]

Adding elements using offer():
Offer status: true
Queue after offer: [B, C, D]

Polling elements using poll():
Polled element: B
Queue after poll: [C, D]

Inspecting the head using element():
Head of the queue: C

Inspecting the head using peek():
Head of the queue (peek): C
```

- `add(E element)`: Adds an element to the rear of the queue; throws an exception if the queue is full.

- `remove()`: Removes and returns the head of the queue; throws an exception if the queue is empty.

- `offer(E element)`: Adds an element to the queue and returns `false` if it fails.

- `poll()`: Removes and returns the head; returns `null` if the queue is empty.

- `element()`: Returns the head of the queue; throws an exception if the queue is empty.

- `peek()`: Returns the head of the queue or `null` if empty.

### The `Deque` Interface

The `Deque` (Double-Ended Queue) interface extends `Queue` to allow element insertion and removal from both ends.

#### Core Methods in `Deque` Interface

1. **Insertion:**

- `boolean addFirst(E element)`: Inserts the element at the front.

- `boolean addLast(E element)`: Inserts the element at the rear.

- `boolean offerFirst(E element)`: Attempts to insert the element at the front.

- `boolean offerLast(E element)`: Attempts to insert the element at the rear.

2. **Removal:**

- `E pollFirst()`: Removes and returns the front element or null if the deque is empty.

- `E pollLast()`: Removes and returns the rear element or null if the deque is empty.

3. **Inspection:**

- `E getFirst()`: Retrieves the front element. Throws an exception if the deque is empty.

- `E getLast()`: Retrieves the rear element. Throws an exception if the deque is empty.

- `E peekFirst()`: Retrieves the front element or `null` if the deque is empty.

- `E peekLast()`: Retrieves the rear element or `null` if the deque is empty.

### The `PriorityQueue` Interface

The `PriorityQueue` interface extends `Queue` and orders elements based on their **natural ordering** or a c**ustom comparator**. Elements with the highest priority are removed first.

#### Key Charecterstics:

- **Insertion**: Operates similarly to a standard queue but ensures elements are placed based on priority.

- **Removal**: The `remove()` method retrieves and removes the element with the highest priority.

#### Concrete Implementations

1. `LinkedList`

The `LinkedList` class implements both `Queue` and `Deque`. It provides the following:

- **Underlying Structure**: Doubly linked list.

- **Key Characteristics**:
  - Efficient insertion and removal at both ends.
  - Can function as a queue or deque.

2. `ArrayDeque`

The `ArrayDeque` class provides a circular array implementation of `Deque`.

- **Key Characteristics**:
  - Supports efficient resizing.
  - Preferred over LinkedList for stack and queue operations due to lower overhead.

#### Summary - The `Queue` Interface

The `Queue` and `Deque` interfaces in Java provide versatile tools for managing ordered collections. While `Queue` ensures **FIFO** behavior, `Deque` allows for flexible **insertions and removals from both ends**. Implementations like `LinkedList` and `ArrayDeque` cater to different performance needs, enabling developers to choose the most suitable structure for their applications.

## Maps

The `Map` interface in Java is designed to handle key-value pairs, enabling efficient data storage and retrieval based on unique keys. Unlike the `Collection` interface, which deals with grouped data (e.g., arrays, lists, and sets), `Map` focuses on key-value structures, analogous to dictionaries in Python.

A Map has two type parameters:

- **K**: The type for keys.
- **V**: The type for values.

**Key operations include:**

- `get(K key)`: Retrieves the value associated with a key.

- `put(K key, V value)`: Updates or adds a key-value pair.

- `containsKey(Object key)` and `containsValue(Object value)`: Check for the presence of specific keys or values.

### Key Charecterstics of Maps

- **Keys form a set**: Each key is unique, and adding a new value with an existing key overwrites the old value.

- `put(K key, V value)` returns the previous value associated with the key or `null` if no such value existed.

**Code Example:**

```java
import java.util.HashMap;
import java.util.Map;
public class CoreMapOperations {
    public static void main(String[] args) {

        Map<String, Integer> scores = new HashMap<>();

        // Adding key-value pairs using put()
        scores.put("Alice", 85);
        scores.put("Bob", 90);

        // Retrieving values using get()
        System.out.println("Score of Alice: " + scores.get("Alice"));

        // Checking for keys and values
        System.out.println("Contains key 'Bob'? " + scores.containsKey("Bob"));
        System.out.println("Contains value 90? " + scores.containsValue(90));
    }
}
```

**Output:**

```
Score of Alice: 85
Contains key 'Bob'? true
Contains value 90? true
```

### Updating Maps

To address the initialization problem—whether to update an existing entry or create a new one—Java provides:

1. `getOrDefault(K key, V defaultValue)`: Returns the value for a key if it exists; otherwise, returns a default value.

```java
import java.util.*;

public class MapGetOrDefault {
    public static void main(String[] args) {

        Map<String, Integer> scores = new HashMap<>();

        int defaultScore = scores.getOrDefault("Charlie", 0);
        System.out.println("Default score for Charlie: " + defaultScore);

        scores.put("Charlie", defaultScore + 10);
        System.out.println("Updated score for Charlie: " + scores.get("Charlie"));
    }
}
```

**Output:**

```
Default score for Charlie: 0
Updated score for Charlie: 10
```

2. `putIfAbsent(K key, V value)`: Adds a key-value pair only if the key is missing.

```java
import java.util.*;

public class MapPutIfAbsent {
    public static void main(String[] args) {

        Map<String, Integer> scores = new HashMap<>();

        // Adding a key-value pair only if the key is absent

        scores.putIfAbsent("Dave", 50);
        scores.putIfAbsent("Dave", 100); // Has no effect
        System.out.println("Score of Dave: " + scores.get("Dave"));
    }
}
```

**Output:**

```
Score of Dave: 50
```

3. `merge(K key, V value, BiFunction<V, V, V> remappingFunction)`: Combines the current value with a new value using a specified function.

```java
import java.util.*;

public class MapMerge {
    public static void main(String[] args) {

        Map<String, Integer> scores = new HashMap<>();
        scores.put("Eve", 20);

        // Merging values using a remapping function
        scores.merge("Eve", 10, Integer::sum);
        System.out.println("Updated score for Eve: " + scores.get("Eve"));
    }
}
```

**Output:**

```
Updated score for Eve: 30
```

### Extracting Keys and Values

Maps provide methods to extract subsets of their data:

- `keySet()`: Returns a `Set` of all keys.

- `values()`: Returns a `Collection` of all values.

- `entrySet()`: Returns a `Set` of key-value pairs as `Map.Entry<K, V>` objects.

### Iterating Over a Map

- **Using `keySet()`** – Iterates over the keys and retrieves values using them.

- **Using `values()`** – Iterates directly over the values.

- **Using `entrySet()`** – Iterates over key-value pairs together.

- **Using an Iterator** – Useful when modifying the map while iterating.

```java
import java.util.*;

public class MapExtractingData {
    public static void main(String[] args) {

    Map<String, Integer> scores = Map.of("Alice", 85, "Bob", 90);

    // Extracting keys
    System.out.println("Keys: " + scores.keySet());

    // Extracting values
    System.out.println("Values: " + scores.values());

    // Extracting entries
    System.out.println("Entries: " + scores.entrySet());
    }
}
```

**Output:**

```
Keys: [Alice, Bob]
Values: [85, 90]
Entries: [Alice=85, Bob=90]
```

### Concrete Implementations of Maps

Java provides multiple implementations of the `Map` interface, each with distinct features:

1. `HashMap `

- Uses a hash table for storage.
- Provides fast access but does not guarantee order.

**Code Example:**

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {

    public static void main(String[] args) {

        Map<String, Integer> hashMap = new HashMap<>();

        // Adding key-value pairs
        hashMap.put("Alice", 85);
        hashMap.put("Bob", 90);
        hashMap.put("Charlie", 75);

        // Printing the HashMap
        System.out.println("HashMap:");

        for (Map.Entry<String, Integer> entry : hashMap.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }
}
```

**Output:**

```
HashMap:
Alice : 85
Charlie : 75
Bob : 90
```

2. `TreeMap`

- Uses a _balanced search tree_.

- Keys are sorted in natural or custom order.

**Code Example:**

```java
import java.util.Map;
import java.util.TreeMap;

public class TreeMapExample {

    public static void main(String[] args) {

        Map<String, Integer> treeMap = new TreeMap<>();

        // Adding key-value pairs
        treeMap.put("Alice", 85);
        treeMap.put("Charlie", 75);
        treeMap.put("Bob", 90);

        // Printing the TreeMap (Keys are sorted in ascending order)
        System.out.println("TreeMap (Keys sorted):");

        for (Map.Entry<String, Integer> entry : treeMap.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }
}
```

**Output:**

```
TreeMap (Keys sorted):
Alice : 85
Bob : 90
Charlie : 75
```

3. `LinkedHashMap`

- Maintains insertion order or access order.
- Ideal for implementing least recently used (LRU) caching.

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LinkedHashMapExample {

    public static void main(String[] args) {

        Map<String, Integer> linkedHashMap = new LinkedHashMap<>();

        // Adding key-value pairs
        linkedHashMap.put("Alice", 85);
        linkedHashMap.put("Bob", 90);

        // Printing the LinkedHashMap (Preserves insertion order)
        System.out.println("LinkedHashMap (Insertion Order):");

        for (Map.Entry<String, Integer> entry : linkedHashMap.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }
}
```

```
LinkedHashMap (Insertion Order):
Alice : 85
Bob : 90
Charlie : 75
```
