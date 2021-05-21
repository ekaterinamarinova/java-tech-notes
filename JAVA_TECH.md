# JAVA TECH STUFF FOR INTERVIEWS AND COMMON KNOWLEDGE

## Content:
### 1. OOP
  - SOLID principles
  - Abstraction
  - Encapsulation
  - Inheritance
  - Polymorphism
### 2. Data Structures and Algorithms
  - Collections API hierarchy and overview
  - List Interface and its implementations
  - Set interface and its implementations
  - Queue Interface and its implementations
  - Map Interface, implementations and why it doesn't belong in the Collections
### 3. Lambda Expressions and Method References
  - Lambdas 
    - Syntax Overview
    - Examples
  - Method References
    - Syntax Overview
    - Examples
### 4. Stream API
  - Overview
  - Stream Creation
  - Stream Operations
    - Iterating
    - Filtering
    - Mapping
    - Matching
    - Reduction
    - Collecting
### 5. Date Time API
  - Using LocalDate
  - Using LocalTime
  - Using LocalDateTime
  - Using ZonedDateTime
  - Using Period and Duration
  - DateTime Formatters
### 6. Serialization API 
### 7. Networking API
### 8. Concurrent API
### 9. Spring
  - 9.1. Spring Core
  - 9.2. Spring Boot
  - 9.3. Spring MVC & Web
  - 9.4. Spring Data JPA
  - 9.5. Spring Security  
### 10. JDBC
### 11. Databases
- 11.1. Relatoinal
  - SQL Basic Queries
  - SQL Joins
  - SQL Transactions
- 11.2. NoSQL
--------------------------------------------------------------------
## 1. Object-Oriented Programming 
>OOP is a programming paradigm based on the concept of "objects", which can  contain data and code: data in the form of fields (often known as attributes or properties), and code, in the form of procedures (often known as methods).   

 Source: [Wikipedia](https://en.wikipedia.org/wiki/Object-oriented_programming)

----

### SOLID Principles  
SOLID stands for (S)ingle responsibility, (O)pen-closed, (L)iskov substitution principle, (I)nterface seggregation and (D)ependency Inversion principle.  
  - Single Responsibility represents that each class should be responsible for one single thing.
  - Open-Closed says that objects should be Open for extension but Closed for modification. This means that the design of the application should be such that if new functionallity needs to be added to the objects they shouldn't be changed, but rather extended by creating a new object that extends the base one with new functionality.
  - Liskov Substitution principle suggests that each parent object should be able to be replaced with a child of his without errors.
  - Interface Seggregation states that no client should be forced to depend on methods it does not use. That means, when building our abstraction layer, it's better to create 3 smaller interfaces than one big.
  - Dependency Inversion means two things:
    - High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g. interfaces).
    - Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.  
      
    To clarify with an example - consider having two classes ChineseFood and WoodenTable. Both of these should be implementations of abstractions like Food and Table for example. If we want to make them interact by having 'food on the table', then we should create an object of type Food inside the implementation of our table, like such: 
    ``` 
    interface Food {}
    interface Table {}

    class ChineseFood implements Food {
        //relevant code
    }

    class WoodenTable implements Table {

        //notice how the type of this variable is Food
        private Food food;

        /*
          here the DI principle is present
          because when creating an object of type WoodenTable
          we can pass any Food implementation to the constructor
          this is a way to generify our application while making it 
          work with > 1 concrete implementations 
        */
        public WoodenTable(Food food) {
            this.food = food;
        }

        //further code
    }
---

### Abstraction 
>Abstract means a concept or an Idea which is not associated with any particular instance. Using abstract class/Interface we express the intent of the class rather than the actual implementation. In a way, one class should not know the inner details of another in order to use it, just knowing the interfaces should be good enough.  

Source: [Medium](https://medium.com/@cancerian0684/what-are-four-basic-principles-of-object-oriented-programming-645af8b43727)

---

### Encapsulation   

It is the mechanism of using acess modifiers to make fields visible and accesable only through setter/getter methods.  
The advantage of that is that we can add furtherer validation in the setter methods, we can completely ommit them making the field only accessible for read operations and such.

Here's a simple example:
```
class Foo {
    private Bar bar;

    public void setBar(Bar bar) {
        this.bar = bar;
    }

    public Bar getBar() {
        return this.bar;
    }
}
```

---

### Inheritance

It represents the ability to create relationships between objects.  
Such as is-a or has-a relationship.  
In Java inheritence is possible through the extends or implements keywords.

An example of an is-a relationship:
```
//by implementing an interface
interface Foo {}

class Bar implements Foo {}

//or by extending a regular or an abstract class
abstract class Pin {}

class Pon extends Pin {}

//or by an interface extending another
interface Tag extends Foo {}
```
An example of a has-a relationship:
```
class Bike {
    //bike has wheel
    private Wheel wheel;
}
```
---
### Polymorphism

It means one name many forms. It's achieved through method overloading and overrriding. The first is static and the second is labeled dynamic.  
It's related to inheritence. Here's an example:
```
interface Foo {
    void foo();
    void foo(int i);
}

class Bar implements Foo {

    @Override
    void foo() {
        //code
    }

    @Override
    void foo(int i) {
        ++i;
        //other code
    }
}
```
In the above example we have both static and dynamic polymorphism.  
We override the interface's methods in order to create some functionality, those methods on the other hand are overloaded - they have the same name but take different parameters.

---
## 2. Data Structures and Algorithms  
In Java we already have a few data structures and algorithms implemented and we're going to have a look at them.

### Collections API Hierarchy and Overview  
The Collections API contains a few basic interfaces to set some constraints, and many implementations to these interfaces.  
This is the hierarchy of the API:

![Collections Hierarchy](class-and-interface-hierarchy.png)  

### List Interface and Its Implemintations  
Some of the most frequently used List implementations are ArrayList and LinkedList the less-frequently used ones are Stack and Vector classes.
- ArrayList simply stores its elements in an array. Here is the time complexity for each main operation:
  - add(T t) - O(1), constant time
  - get(T t) - O(1), constant time
  - remove(T t) - O(n), linear time, since we have to iterate to find it
- LinkedList is an implementation of doubly-linked list, hence it keeps a reference to the next AND previous node. Time complexity is as follow:
  - add(T t) - O(1), constant time
  - get(T t) - O(n), linear time 
  - remove(T t) - O(n), linear, since it iterates to find the first occurance of t

All interfaces are generic, so the best way to create an instance is:
```
List<String> stringList = new ArrayList<>();
```
---
### Set Interface and Its Implementations
The Set is a data structure that holds only unique values and also it doesn't guarantee ordering of the elements like List does.  
The more frequently used implementations are HashSet and TreeSet.
- The HashSet stores its elements as keys inside a HashMap, since HashMap guarantees unique keys. Therefore time complexity and internal working is the same as the Map's:
  - add(T t) - O(1) since the hashing function used to calculate the index where the element will be stored is constant time
  - get(T t) - O(log n) is worst case, since the map has been tweaked to keep the data in a tree rather than linkedlist on certain conditions.
  - remove(T t) - O(log n) for searching reasons
- The TreeSet, same as HashSet, uses a Map implementation to back up its data. This time tho it's a NavigableMap, which behaves as a sorted linked list with many methods for navigation.
  - add(T t) - O(1) if adding to the head or tail
  - get(T t) - O(ln n) since it's a linear data structure
  - remove(T t) - O(ln n) except if deleting first or last element  

  It can be instantiated as follows:
  ```
  Set<String> uniqueStrings = new HashSet<>();
  ```
---
### Queue Interface and Its Implementations
Queues are again a linear data structure. They are FIFO(First In First Out) structures. 


