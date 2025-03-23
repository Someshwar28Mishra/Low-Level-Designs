# SOLID Principles

The SOLID principles are a set of five design principles intended to make software designs more understandable, flexible, and maintainable. They were introduced by Robert C. Martin (Uncle Bob).

## 1. Single Responsibility Principle (SRP)
A class should have only one reason to change, meaning it should have only one job or responsibility.

### Violation of SRP:
```java
class Report {
    public void generateReport() {
        // Generate report logic
    }

    public void saveToFile() {
        // Save report to file logic
    }
}
```

### Adhering to SRP:
```java
class Report {
    public void generateReport() {
        // Generate report logic
    }
}

class ReportSaver {
    public void saveToFile(Report report) {
        // Save report to file logic
    }
}
```

### Class Diagram:
```
+-----------------+       +-----------------+
|     Report      |       |   ReportSaver   |
+-----------------+       +-----------------+
| +generateReport()|       | +saveToFile()   |
+-----------------+       +-----------------+
```
---

## 2. Open/Closed Principle (OCP)
Software entities (classes, modules, functions) should be open for extension but closed for modification.

### Violation of OCP:
```java
class Rectangle {
    public double width;
    public double height;
}

class AreaCalculator {
    public double calculateArea(Rectangle rectangle) {
        return rectangle.width * rectangle.height;
    }
}
```

### Adhering to OCP:
```java
interface Shape {
    double calculateArea();
}

class Rectangle implements Shape {
    public double width;
    public double height;

    @Override
    public double calculateArea() {
        return width * height;
    }
}

class Circle implements Shape {
    public double radius;

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

### Class Diagram:
```
+-----------------+       +-----------------+
|     Shape       |       |   Rectangle     |
+-----------------+       +-----------------+
| +calculateArea()|<------| +calculateArea()|
+-----------------+       +-----------------+
                                ^
                                |
                        +-----------------+
                        |     Circle      |
                        +-----------------+
                        | +calculateArea()|
                        +-----------------+
```
---

## 3. Liskov Substitution Principle (LSP)
Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.

### Violation of LSP:
```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostrich cannot fly");
    }
}
```

### Adhering to LSP:
```java
interface Bird {
    void move();
}

class Sparrow implements Bird {
    @Override
    public void move() {
        System.out.println("Flying");
    }
}

class Ostrich implements Bird {
    @Override
    public void move() {
        System.out.println("Running");
    }
}
```

### Class Diagram:
```
+-----------------+       +-----------------+
|      Bird       |       |     Sparrow     |
+-----------------+       +-----------------+
| +move()         |<------| +move()         |
+-----------------+       +-----------------+
                                ^
                                |
                        +-----------------+
                        |     Ostrich     |
                        +-----------------+
                        | +move()         |
                        +-----------------+
```
---

## 4. Interface Segregation Principle (ISP)
Clients should not be forced to depend on interfaces they do not use.

### Violation of ISP:
```java
interface Worker {
    void work();
    void eat();
}

class HumanWorker implements Worker {
    @Override
    public void work() {
        System.out.println("Working");
    }

    @Override
    public void eat() {
        System.out.println("Eating");
    }
}

class RobotWorker implements Worker {
    @Override
    public void work() {
        System.out.println("Working");
    }

    @Override
    public void eat() {
        throw new UnsupportedOperationException("Robot cannot eat");
    }
}
```

### Adhering to ISP:
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class HumanWorker implements Workable, Eatable {
    @Override
    public void work() {
        System.out.println("Working");
    }

    @Override
    public void eat() {
        System.out.println("Eating");
    }
}

class RobotWorker implements Workable {
    @Override
    public void work() {
        System.out.println("Working");
    }
}
```

### Class Diagram:
```
+-----------------+       +-----------------+       +-----------------+
|   Workable      |       |   Eatable       |       |  HumanWorker    |
+-----------------+       +-----------------+       +-----------------+
| +work()         |       | +eat()          |<------| +work()         |
+-----------------+       +-----------------+       | +eat()          |
        ^                                           +-----------------+
        |
+-----------------+
|  RobotWorker    |
+-----------------+
| +work()         |
+-----------------+
```
---

## 5. Dependency Inversion Principle (DIP)
High-level modules should not depend on low-level modules. Both should depend on abstractions.

### Violation of DIP:
```java
class LightBulb {
    public void turnOn() {
        System.out.println("LightBulb: On");
    }

    public void turnOff() {
        System.out.println("LightBulb: Off");
    }
}

class Switch {
    private LightBulb bulb;

    public Switch(LightBulb bulb) {
        this.bulb = bulb;
    }

    public void operate() {
        bulb.turnOn();
    }
}
```

### Adhering to DIP:
```java
interface Switchable {
    void turnOn();
    void turnOff();
}

class LightBulb implements Switchable {
    @Override
    public void turnOn() {
        System.out.println("LightBulb: On");
    }

    @Override
    public void turnOff() {
        System.out.println("LightBulb: Off");
    }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}
