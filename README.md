# UML → Java: Relationship Examples

A compact README with copy-pasteable Java snippets that demonstrate common UML class relationships. Each section contains a short explanation and a runnable (or near-runnable) Java example.

> **Tip:** Each code block is standalone. Copy the block into a `.java` file or an IDE, adjust package/imports if needed, and run.

---

## Table of Contents
1. [Association (bi-directional)](#1-association-bi-directional)
2. [Directed Association (unidirectional)](#2-directed-association-unidirectional)
3. [Aggregation (whole-part, parts independent)](#3-aggregation-whole-part-parts-independent)
4. [Composition (strong whole-part, lifecycle tied)](#4-composition-strong-whole-part-lifecycle-tied)
5. [Generalization (Inheritance — “is-a”)](#5-generalization-inheritance--is-a)
6. [Realization (Interface implementation)](#6-realization-interface-implementation)
7. [Dependency Relationship (loose, temporary use)](#7-dependency-relationship-loose-temporary-use)
8. [Usage (Dependency) Relationship (client uses supplier)](#8-usage-dependency-relationship-client-uses-supplier)

---

### 1. Association (bi-directional)
**Association** — two classes hold references to each other (both know about the other).

```java
// Student and Course have a bi-directional association.
import java.util.*;

class Student {
    private final String name;
    private final List<Course> courses = new ArrayList<>();

    public Student(String name){ this.name = name; }

    public void enroll(Course c){
        if(!courses.contains(c)){
            courses.add(c);
            c.addStudent(this); // keep both sides consistent
        }
    }

    public List<Course> getCourses(){ return Collections.unmodifiableList(courses); }
    public String getName(){ return name; }
}

class Course {
    private final String code;
    private final List<Student> students = new ArrayList<>();

    public Course(String code){ this.code = code; }

    void addStudent(Student s){
        if(!students.contains(s)){
            students.add(s);
            // Student.enroll() is the public entry point to avoid recursion
        }
    }

    public List<Student> getStudents(){ return Collections.unmodifiableList(students); }
    public String getCode(){ return code; }
}
```

---

### 2. Directed Association (unidirectional)
**Directed association** — A references B, but B doesn’t reference A.

```java
// Order has a reference to ShippingAddress (Order -> ShippingAddress)
class ShippingAddress {
    private final String line;
    public ShippingAddress(String line){ this.line = line; }
    public String getLine(){ return line; }
}

class Order {
    private final String id;
    private ShippingAddress shipping; // directed association: Order -> ShippingAddress

    public Order(String id){ this.id = id; }
    public void setShipping(ShippingAddress addr){ this.shipping = addr; }
    public ShippingAddress getShipping(){ return shipping; }
}
```

---

### 3. Aggregation (whole-part, parts independent)
**Aggregation** — whole has parts but parts may live independently of the whole.

```java
// Team aggregates Player. Player can exist without a Team.
import java.util.*;

class Player {
    private final String name;
    public Player(String name){ this.name = name; }
    public String getName(){ return name; }
}

class Team {
    private final String name;
    private final List<Player> roster = new ArrayList<>();

    public Team(String name){ this.name = name; }

    // adds an existing Player (Player can be created outside Team)
    public void addPlayer(Player p){ roster.add(p); }

    public List<Player> getRoster(){ return Collections.unmodifiableList(roster); }
}
```

---

### 4. Composition (strong whole-part, lifecycle tied)
**Composition** — whole creates/owns parts. Parts don’t make sense independently.

```java
// House composes Room. Rooms are created and owned by House.
import java.util.*;

class House {
    private final List<Room> rooms = new ArrayList<>();

    public House(int numRooms){
        for(int i=1;i<=numRooms;i++){
            rooms.add(new Room("Room " + i)); // House constructs and owns Rooms
        }
    }

    public List<Room> getRooms(){ return Collections.unmodifiableList(rooms); }

    // Room is an inner class; constructor is private to restrict external creation
    public static class Room {
        private final String name;
        private Room(String name){ this.name = name; }
        public String getName(){ return name; }
    }
}
```

---

### 5. Generalization (Inheritance — “is-a”)
**Generalization** — subclass extends superclass (inherits implementation).

```java
abstract class Animal {
    protected final String name;
    public Animal(String name){ this.name = name; }
    public void eat(){ System.out.println(name + " is eating."); }
    public abstract void speak(); // subclass must implement
}

class Dog extends Animal { // Dog is-a Animal
    public Dog(String name){ super(name); }
    @Override
    public void speak(){ System.out.println("Woof!"); }
}
```

---

### 6. Realization (Interface implementation)
**Realization** — class implements an interface (fulfills a contract).

```java
interface Drivable {
    void accelerate();
    void brake();
}

class Car implements Drivable { // Car realizes Drivable
    @Override
    public void accelerate(){ System.out.println("Car accelerating"); }
    @Override
    public void brake(){ System.out.println("Car braking"); }
}
```

---

### 7. Dependency Relationship (loose, temporary use)
**Dependency** — one class uses another temporarily (method parameter / local variable) — weak coupling.

```java
// ReportGenerator depends on ReportFormatter (uses it to format output but doesn't own it)
class ReportFormatter {
    public String format(String title, String body){
        return "Title: " + title + "\n" + body;
    }
}

class ReportGenerator {
    public void generate(String title, String body, ReportFormatter formatter){
        // dependency: ReportGenerator depends on ReportFormatter to format, but doesn't keep it.
        String formatted = formatter.format(title, body);
        System.out.println(formatted);
    }
}
```

---

### 8. Usage (Dependency) Relationship (client uses supplier)
**Usage dependency** — client calls supplier to perform tasks (emphasizes temporary usage, not ownership).

```java
// PaymentClient uses ExternalPaymentService to process payment. It does not own the service.
class ExternalPaymentService {
    public boolean process(String account, double amount){
        System.out.println("Processing payment of " + amount + " for " + account);
        return true;
    }
}

class PaymentClient {
    public boolean checkout(String acct, double amt, ExternalPaymentService svc){
        // usage dependency: PaymentClient uses ExternalPaymentService to perform checkout.
        return svc.process(acct, amt);
    }
}
```

---

## Optional: PlantUML snippets
If you'd like, I can add PlantUML code blocks that visually depict each relationship. Tell me which relationships you want diagrams for and I will append them.

---

## License
MIT — feel free to reuse and adapt.

---

*Generated for you. If you want a single `README.md` file downloaded or modified (add PlantUML, TOC links, or examples combined into packages), tell me what to change.*

