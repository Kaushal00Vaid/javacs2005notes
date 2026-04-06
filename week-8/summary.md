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
    <a href="https://javaiitmbs.github.io/week-7/summary.html">Week 7</a>
    <a href="https://javaiitmbs.github.io/index.html">Home</a>
    <a href="https://javaiitmbs.github.io/week-9/summary.html">Week 9</a>
</span> 
<hr>

## Cloning

In Java, creating a faithful copy of an object is not as straightforward as assigning one variable to another.

- Normal Assignment

When you assign one object reference to another, both variables point to the same object in memory. Changes made through one reference affect the object visible through the other reference.

**Code Example:**

```java
public class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String toString() {
        return "Employee{name='" + name + "', salary=" + salary + "}";
    }

    public static void main(String[] args) {
        Employee e1 = new Employee("Dhruv", 21500.0);
        Employee e2 = e1; // e2 refers to the same object as e1
        e2.setName("Eknath");
        System.out.println(e1); // Output: Employee{name='Eknath', salary=21500.0}
    }
}
```

In this example, both `e1` and `e2` refer to the same object. Updating the name through `e2` also changes the name as seen through e1.

### The `clone()` Method

The `clone()` method, provided by the `Object` class, creates a bitwise (shallow) copy of the object. To enable cloning, the class must implement the `Cloneable` interface, and the `clone()` method must be overridden as public.

```java
import java.util.Date;

public class Employee implements Cloneable {
    private String name;
    private double salary;
    private Date birthday;

    public Employee(String name, double salary, Date birthday) {
        this.name = name;
        this.salary = salary;
        this.birthday = birthday;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setBirthday(int day, int month, int year) {
        this.birthday.setDate(day);
        this.birthday.setMonth(month);
        this.birthday.setYear(year);
    }

    @Override
    public Employee clone() throws CloneNotSupportedException {
        return (Employee) super.clone(); // Shallow copy
    }

    public String toString() {
        return "Employee{name='" + name + "', salary=" + salary + ", birthday=" + birthday + "}";
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        Date birthday = new Date(97, 3, 16); // April 16, 1997
        Employee e1 = new Employee("Dhruv", 21500.0, birthday);
        Employee e2 = e1.clone(); // Shallow copy

        e2.setName("Eknath");
        e2.setBirthday(18, 5, 1990); // Changes shared Date object

        // Birthday and name changes affect e1 due to shallow copy
        System.out.println(e1);
        System.out.println(e2);
    }
}
```

**Output**

```
Employee{name='Dhruv', salary=21500.0, birthday=Fri May 18 00:00:00 IST 1990}
Employee{name='Eknath', salary=21500.0, birthday=Fri May 18 00:00:00 IST 1990}
```

### Shallow Copy vs. Deep Copy

- **Shallow Copy**

Copies the top-level structure of the object but does not clone the nested objects. Changes to mutable nested objects in one copy affect the other.

- **Deep Copy**

Recursively clones all nested objects, creating a fully independent copy.

**Code Example of Deep Copy**

```java
import java.util.Date;

public class Employee implements Cloneable {
    private String name;
    private double salary;
    private Date birthday;

    public Employee(String name, double salary, Date birthday) {
        this.name = name;
        this.salary = salary;
        this.birthday = birthday;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setBirthday(int day, int month, int year) {
        this.birthday.setDate(day);
        this.birthday.setMonth(month);
        this.birthday.setYear(year);
    }

    @Override
    public Employee clone() throws CloneNotSupportedException {
        Employee cloned = (Employee) super.clone(); // Shallow copy
        cloned.birthday = (Date) birthday.clone(); // Deep copy of mutable object
        return cloned;
    }

    public String toString() {
        return "Employee{name='" + name + "', salary=" + salary + ", birthday=" + birthday + "}";
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        Date birthday = new Date(97, 3, 16); // April 16, 1997
        Employee e1 = new Employee("Dhruv", 21500.0, birthday);
        Employee e2 = e1.clone(); // Deep copy

        e2.setName("Eknath");
        e2.setBirthday(18, 5, 1990);

        System.out.println(e1); // e1 remains unchanged
        System.out.println(e2);
    }
}
```

**Output**

```
Employee{name='Dhruv', salary=21500.0, birthday=Wed Apr 16 00:00:00 IST 1997}
Employee{name='Eknath', salary=21500.0, birthday=Fri May 18 00:00:00 IST 1990}
```

### Cloning and Inheritance

Cloning becomes more complex when inheritance is involved. If a subclass adds additional mutable fields, the inherited `clone()` method will not automatically deep-copy these fields. Each subclass must override `clone()` to ensure proper behavior.

**Code Example**

```java
import java.util.Date;

public class Manager extends Employee {
    private Date promotionDate;

    public Manager(String name, double salary, Date birthday, Date promotionDate) {
        super(name, salary, birthday);
        this.promotionDate = promotionDate;
    }

    @Override
    public Manager clone() throws CloneNotSupportedException {
        Manager cloned = (Manager) super.clone();
        cloned.promotionDate = (Date) promotionDate.clone(); // Deep copy of promotionDate
        return cloned;
    }

    public String toString() {
        return super.toString() + ", promotionDate=" + promotionDate;
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        Date birthday = new Date(97, 3, 16);
        Date promotionDate = new Date(121, 5, 18);

        Manager m1 = new Manager("Dhruv", 40000.0, birthday, promotionDate);
        Manager m2 = m1.clone();

        m2.setName("Eknath");
        m2.setBirthday(18, 5, 1990);
        m2.promotionDate.setDate(1);

        System.out.println(m1);
        System.out.println(m2);
    }
}
```

### Restrictions on Cloning

1. **Interface:** A class must implement the `Cloneable` marker interface to use the `clone()` method.

2. **Visibility:** The `clone()` method in `Object` is protected. It must be overridden as public for external use.

3. **Exception Handling:** The `clone()` method in `Object` throws
   `CloneNotSupportedException`. Subclasses must either declare or handle this exception.

## Type Inference

Java is a strongly typed programming language, which means every variable must be explicitly declared with its type before use. This enables the compiler to enforce type safety, ensuring that programs are well-typed and free from a significant category of runtime errors. However, in recent years, Java has incorporated limited support for type inference to reduce redundancy in type declarations.

### Type Declarations in Java

Type declarations explicitly specify the type of a variable when it is defined.

**Code Example:**

```java
public class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String toString() {
        return "Employee{name='" + name + "', salary=" + salary + "}";
    }

    public static void main(String[] args) {
        Employee e1 = new Employee("Dhruv", 21500.0);
        Employee e2 = e1;
        e2.setName("Eknath");

        System.out.println(e1); // Output: Employee{name='Eknath', salary=21500.0}
    }
}
```

In this example, `e1` and `e2` are explicitly declared as `Employee` objects. The compiler ensures type safety by verifying that the assignments and operations involving these variables are consistent with the `Employee` type.

### Key Features of Type Inference in Java

Type inference allows the compiler to deduce the type of a variable based on the context of its initialization. In Java, type inference is supported for local variables using the `var` keyword, introduced in Java 10.

1. **Local Variables Only**: Type inference is applicable only for local variables within methods or blocks, not for instance variables or method parameters.

2. **Mandatory Initialization**: Variables declared with `var` must be initialized at the time of declaration.

3. **Inference from Initialization**: The compiler determines the type of the variable based on the expression used to initialize it.

**Code Example:**

```java
public class TypeInferenceExample {
    public static void main(String[] args) {
        var name = "Dhruv"; // Inferred as String
        var salary = 21500.0; // Inferred as double

        System.out.println("Name: " + name);
        System.out.println("Salary: " + salary);

        var employee = new Employee("Eknath", 30000.0); // Inferred as Employee
        System.out.println(employee);
    }
}
```

In this example, the type of each variable is inferred from its initialization. For instance, `name` is inferred as `String`, `salary` as `double`, and `employee` as `Employee`.

### Benifits of Type Inference

1. **Reduced Redundancy**: Eliminates the need to repeat type information in declarations, making code more concise.

**For Example:**

```java
Manager m = new Manager("Ravi", 50000.0);
```

can be simplified to:

```java
var m = new Manager("Ravi", 50000.0);
```

2. **Improved Readability**: Simplifies code by reducing clutter, especially in cases involving generics or complex type hierarchies.

3. **Enhanced Productivity**: Allows developers to focus on logic rather than writing verbose type annotations.

### Limitations of Type Inference

1. **Limited Scope**: Applicable only to local variables. Instance variables and method parameters still require explicit type declarations.

2. **Potential Ambiguity**: Without explicit types, understanding the inferred type requires examining the initialization expression.

3. **Initialization Requirement**: Variables declared with `var` must be initialized, which can sometimes lead to verbose or repetitive initialization expressions.

4. **Precision**: The inferred type is the most specific type possible.

For instance:

```java
var e = new Manager("Ravi", 50000.0);
```

Here, `e` is inferred as `Manager`. If `e` should have been `Employee`, an explicit declaration is required:

```java
Employee e = new Manager("Ravi", 50000.0);
```

### Propagation of Inferred Types

Type inference allows the compiler to propagate type information through expressions.

**For Example**

```java
var s = "Hello, "; // Inferred as String
var t = s + "world!"; // Propagated as String
System.out.println(t);
```

The inferred type of `t` is `String` because it is the result of concatenating `s` (inferred as `String`) with another string constant.

### Static Type Checking

Java performs static type checking at compile-time, ensuring that all type inferences are consistent with the declared types in the code. For example, consider the following:

```java
Employee e;
Manager m = new Manager("Ravi", 50000.0);
e = m; // Allowed due to subtyping (Manager extends Employee)

var x = e;
x.bonus(); // Compilation error: Employee does not have a bonus() method
```

Here, the inferred type of `x` is `Employee`, so invoking `bonus()` on `x` results in a compilation error.

## Higher Order Function

A higher-order function is a function that takes another function as an argument. While this concept is common in many programming paradigms, its integration into Java—a strongly object-oriented language—is achieved through interfaces and functional programming constructs like **lambda expressions** and **method references**.

### Callbacks Example

A typical use case for higher-order functions is a callback mechanism. Consider a scenario where an object, `MyClass`, creates a `Timer` that runs in parallel. When the timer expires, it must notify `MyClass`.

In object-oriented programming, this is achieved using an `interface`:

**Code Example**

```java
public interface TimerOwner {
    void timerDone();
}

public class MyClass implements TimerOwner {
    @Override
    public void timerDone() {
        System.out.println("Timer has expired.");
    }

    public static void main(String[] args) {
        MyClass myClass = new MyClass();
        Timer timer = new Timer(myClass);
        timer.start();
    }
}

public class Timer implements Runnable {
    private final TimerOwner owner;

    public Timer(TimerOwner owner) {
        this.owner = owner;
    }

    public void start() {
        new Thread(this).start();
    }

    @Override
    public void run() {
        try {
            Thread.sleep(5000); // Simulate timer
            owner.timerDone();
        } catch (InterruptedException e) {
            System.out.println("Timer interrupted.");
        }
    }
}
```

### Customizing Behavior with `Comparator`

The `Comparator` interface is a practical example of higher-order functions in Java. It allows customization of the `Arrays.sort` method by specifying a comparison function.

**Code Example**

```java
import java.util.Arrays;
import java.util.Comparator;

public class StringLengthSorter {
    public static void main(String[] args) {
        String[] strings = {"apple", "banana", "cherry"};

        Arrays.sort(strings, new Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                return s1.length() - s2.length();
            }
        });

        System.out.println(Arrays.toString(strings));
    }
}
```

### Functional Interfaces

Functional interfaces are interfaces with a single abstract method.

Examples include `Comparator` and `TimerOwner`. Functional interfaces enable passing behavior using anonymous classes or lambda expressions.

### Lambda Expressions

Lambda expressions are anonymous functions that can be used wherever a functional interface is required. They are concise and eliminate the need for verbose anonymous class implementations.

**Code Example**

```java
import java.util.Arrays;

public class LambdaExample {
    public static void main(String[] args) {
        String[] strings = {"apple", "banana", "cherry"};

        Arrays.sort(strings, (s1, s2) -> s1.length() - s2.length());

        System.out.println(Arrays.toString(strings));
    }
}
```

In this example, `(s1, s2) -> s1.length() - s2.length()` is a lambda expression that replaces the anonymous class implementation.

#### Complex Lambda Expressions

Lambda expressions can include multiple statements within a block, making them suitable for more complex logic.

**Code Example**

```java
Arrays.sort(strings, (s1, s2) -> {
    if (s1.length() < s2.length())
        return -1;
    else if (s1.length() > s2.length())
        return 1;
    else
        return 0;
});
```

#### Method References

If a lambda expression consists of a single method call, it can be replaced by a method reference. Method references simplify code further by directly referencing existing methods.

**Code Example**

```java
import java.util.Arrays;
import java.util.List;

public class MethodReferenceExample {
    public static void main(String[] args) {
        List<String> strings = Arrays.asList("apple", "banana", "cherry");

        strings.forEach(System.out::println); // Method reference
    }
}
```

**Method Reference Syntax**

1. **Static Method**: `ClassName::methodName`

2. **Instance Method of Specific Object**: `object::methodName`

3. **Instance Method of Arbitrary Object of a Class**: `ClassName::methodName`

4. **Constructor**: `ClassName::new`

_Example with Constructor Reference_

```java
import java.util.function.Function;

public class ConstructorReferenceExample {
    public static void main(String[] args) {
        Function<String, Integer> stringToInteger = Integer::new;
        Integer number = stringToInteger.apply("123");

        System.out.println(number);
    }
}
```

## Streams

Collections in Java provide a powerful way to store and manipulate groups of elements. Traditionally, an **iterator** is used to sequentially process these elements. Java’s **streams** offer an alternative declarative and functional approach for working with collections.

Streams enable operations such as filtering, mapping, and reducing, often in a more concise and readable manner.

**Example:** _Counting Long Words_

- Using an Iterator:

```java
import java.util.List;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        // Example list of words
        List<String> words = Arrays.asList(
            "banana", "hippopotamus", "apple", "elephant", "encyclopedia"
        );

        // Initialize count to 0
        long count = 0;

        // Iterate over each word in the list
        for (String word : words) {
            // Check if the word length is greater than 10
            if (word.length() > 10) {
                count++;
            }
        }

        // Output the count of words with length greater than 10
        System.out.println("Number of words with length greater than 10: " + count);
    }
}
```

- Using Streams:

```java
import java.util.List;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        List<String> words = Arrays.asList(
            "banana", "hippopotamus", "apple", "elephant", "encyclopedia"
        );

        // Using streams to count words with length greater than 10
        long count = words.stream()
                          .filter(w -> w.length() > 10)
                          .count();

        System.out.println("Number of words with length greater than 10: " + count);
    }
}
```

### Why Use Streams?

1. **Declarative Style:** Focus on what to compute rather than how to compute.

2. **Parallel Processing:** Operations like `filter()` and `count()` can be parallelized.

3. **Lazy Evaluation:** Streams process elements only when needed, optimizing performance.

4. **Support for Infinite Streams:** Streams can generate values dynamically, even for infinite sequences.

**Example:** _Parellel Streams_

```java
long count = words.parallelStream()
                  .filter(w -> w.length() > 10)
                  .count();
```

### Working with Streams

Streams allow you to:

1. Create a Stream

2. Apply Intermediate Operations (transformations)

3. Apply a Terminal Operation (result computation)

Streams are non-destructive; they do not modify the underlying collection.

_Example Workflow:_

```java
long count = words.stream()
                  .filter(w -> w.length() > 10) // Intermediate operation
                  .count(); // Terminal operation
```

### Creating Streams

Streams can be Created from:

**1. Collections**

```java
List<String> wordList = Arrays.asList(
    "banana", "hippopotamus", "apple", "elephant", "encyclopedia"
);

Stream<String> wordStream = wordList.stream();
```

**2. Arrays**

```java
String[] wordArr = {
    "banana", "hippopotamus", "apple", "elephant", "encyclopedia"
};

Stream<String> wordStream = Stream.of(wordArr);
```

**3. Generated Values**

- Using `Stream.generate()`

```java
Stream<String> echos = Stream.generate(() -> "Echo");
Stream<Double> randomDs = Stream.generate(Math::random);
```

- Using `Stream.iterate()`

```java
Stream<Integer> integers = Stream.iterate(0, n -> n + 1);
```

- With a termination condition:

```java
Stream<Integer> limitedIntegers = Stream.iterate(0, n -> n < 100, n -> n + 1);
```

### Transforming Streams

- **Filtering**

Filters elements based on a predicate:

```java
Stream<String> longWords = wordList.stream()
    .filter(w -> w.length() > 10);
```

- **Mapping**

Applies a function to each element:

```java
Stream<String> startLongWords = wordList.stream()
    .filter(w -> w.length() > 10)
    .map(s -> s.substring(0, 1));
```

- **Flattening**

Combines nested lists into a single stream:

```java
Stream<Character> letters = wordList.stream()
    .flatMap(s -> s.chars().mapToObj(c -> (char) c));
```

### Managing Stream Size

- **Limiting**

Restricts the stream to a fixed number of elements:

```java
Stream<Double> randomDs = Stream.generate(Math::random)
    .limit(100);
```

- **Skipping**

Skips the first `n` elements:

```java
Stream<Double> randomds = Stream.generate(Math::random).skip(10);
```

- **Conditional Stopping**
  - **Take While**: Stops when a condition becomes false:

  ```java
  Stream<Double> filtered = Stream.generate(Math::random)
      .takeWhile(n -> n >= 0.5);
  ```

  - **Drop While**: Starts when a condition becomes false:

  ```java
  Stream<Double> filtered = Stream.generate(Math::random)
      .dropWhile(n -> n <= 0.05);
  ```

### Reducing Streams

- **Counting**

Counts the number of elements:

```java
long count = Stream.generate(Math::random)
    .limit(100)
    .filter(n -> n > 0.1)
    .count();
```

- **Finding Maximum/Minimum**

Finds the largest/smallest element based on a comparator:

```java
import java.util.Optional;
import java.util.stream.Stream;

public class MaxRandomExample {
    public static void main(String[] args) {
        Optional<Double> maxRand = Stream.generate(Math::random)
            .limit(100)
            .max(Double::compareTo);

        // Print the maximum random number if present
        maxRand.ifPresentOrElse(
            max -> System.out.println("Maximum random number: " + max),
            () -> System.out.println("No maximum value found")
        );
    }
}
```

- **Finding the First Element**

Retrieves the first element:

```java
import java.util.Optional;
import java.util.stream.Stream;

public class FirstRandomExample {
    public static void main(String[] args) {
        Optional<Double> firstRand = Stream.generate(Math::random)
            .limit(100)
            .filter(n -> n > 0.999)
            .findFirst();

        // Print the first matching random number if present
        firstRand.ifPresentOrElse(
            num -> System.out.println("First random number > 0.999: " + num),
            () -> System.out.println("No number greater than 0.999 found")
        );
    }
}
```
