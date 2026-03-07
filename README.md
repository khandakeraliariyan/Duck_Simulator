# Duck Simulator

A comprehensive Java project demonstrating six fundamental design patterns in object-oriented programming. The Duck Simulator showcases how different design patterns work together to create a flexible, extensible, and maintainable codebase for managing duck behaviors and simulations.

## Project Overview

The Duck Simulator is an educational project that implements a realistic duck simulation system. It demonstrates how different design patterns solve common software design problems by allowing ducks to quack, be tracked, and interact with each other in flocks, while also supporting unconventional birds like geese.

## Design Patterns Implemented

This project implements six key design patterns from the Gang of Four (GoF) design patterns:

### 1. **Strategy Pattern** 🦆

**Location:** `src/strategy/`

The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. In this project, different duck types have different quacking behaviors.

**Components:**

- `Quackable.java` - Interface defining the contract for objects that can quack
- `MallardDuck.java` - Concrete mallard duck implementation with "Quack" behavior
- `RedheadDuck.java` - Concrete redhead duck implementation
- `RubberDuck.java` - Concrete rubber duck implementation (squeaks instead of quacking)

**How it works:** Each duck type implements the `Quackable` interface with its own quacking strategy, allowing different behaviors to be switched at runtime.

---

### 2. **Observer Pattern** 👀

**Location:** `src/observer/`

The Observer pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified automatically.

**Components:**

- `Observable.java` - Interface for objects that can be observed
- `ObservableImpl.java` - Implementation of the Observable interface
- `Observer.java` - Interface for objects that observe others
- `Quackologist.java` - Concrete observer that reacts to duck quacks

**How it works:** When a duck quacks, all registered observers (Quackologists) are notified and can react to the event. This decouples the duck from the specific observers.

---

### 3. **Composite Pattern** 🐦

**Location:** `src/composite/`

The Composite pattern composes objects into tree structures to represent part-whole hierarchies. It lets clients treat individual objects and compositions of objects uniformly.

**Components:**

- `Flock.java` - Container class that holds multiple ducks and manages them as a single unit

**How it works:** A `Flock` contains multiple `Quackable` objects (individual ducks or even other flocks). When you call `quack()` on a flock, it delegates to all ducks in the flock. This allows treating a collection of ducks the same way as a single duck.

---

### 4. **Adapter Pattern 🪡 (Bridge)**

**Location:** `src/adapter/`

The Adapter pattern converts the interface of a class into another interface clients expect. It lets classes work together that couldn't otherwise because of incompatible interfaces.

**Components:**

- `Goose.java` - External class (incompatible with Quackable interface)
- `GooseAdapter.java` - Adapter that makes Goose implement the Quackable interface

**How it works:** The `GooseAdapter` wraps a `Goose` object and implements the `Quackable` interface. When `quack()` is called, it delegates to the goose's `honk()` method, allowing geese to be treated like ducks in the simulator.

---

### 5. **Decorator Pattern 🎀**

**Location:** `src/decorator/`

The Decorator pattern attaches additional responsibilities to an object dynamically. It provides a flexible alternative to subclassing for extending functionality.

**Components:**

- `QuackCounter.java` - Decorator that wraps a `Quackable` and counts total quacks

**How it works:** `QuackCounter` wraps any `Quackable` object and tracks how many times `quack()` is called across all ducks. It transparently intercepts calls and increments a counter while delegating the actual quacking to the wrapped duck.

---

### 6. **Factory Pattern 🏭**

**Location:** `src/factory/`

The Factory pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes.

**Components:**

- `AbstractDuckFactory.java` - Abstract factory defining factory methods
- `DuckFactory.java` - Basic factory implementation
- `CountingDuckFactory.java` - Enhanced factory that wraps ducks with `QuackCounter` decorator

**How it works:** Instead of directly instantiating duck objects, clients request them from the factory. The `CountingDuckFactory` extends this by automatically decorating ducks with the `QuackCounter`, ensuring all ducks are tracked from creation.

---

## Project Structure

```
Duck_Simulator/
├── src/
│   ├── Main.java                          # Entry point demonstrating all patterns
│   ├── strategy/
│   │   ├── Quackable.java                 # Strategy interface
│   │   ├── MallardDuck.java               # Concrete strategy: Mallard
│   │   ├── RedheadDuck.java               # Concrete strategy: Redhead
│   │   └── RubberDuck.java                # Concrete strategy: Rubber
│   ├── observer/
│   │   ├── Observable.java                # Observable interface
│   │   ├── ObservableImpl.java             # Observable implementation
│   │   ├── Observer.java                  # Observer interface
│   │   └── Quackologist.java              # Concrete observer
│   ├── composite/
│   │   └── Flock.java                     # Composite container
│   ├── adapter/
│   │   ├── Goose.java                     # Incompatible class
│   │   └── GooseAdapter.java              # Adapter
│   ├── decorator/
│   │   └── QuackCounter.java              # Decorator
│   └── factory/
│       ├── AbstractDuckFactory.java        # Abstract factory
│       ├── DuckFactory.java               # Concrete factory
│       └── CountingDuckFactory.java       # Enhanced factory
├── Project.iml                            # IntelliJ IDEA project file
└── README.md                              # This file
```

## How It Works

### Main Execution Flow

When you run `Main.java`, the following sequence occurs:

1. **Factory Creation**: A `CountingDuckFactory` is instantiated to manage duck creation
2. **Duck Creation**: Three different duck types are created (Mallard, Redhead, Rubber)
3. **Adaptation**: A Goose is adapted to function as a Quackable duck
4. **Composition**: All ducks and the goose are added to a Flock
5. **Observation**: A Quackologist observer is registered to monitor the flock
6. **Simulation**: The entire flock quacks together
7. **Decoration**: The total quack count is displayed (tracked by QuackCounter)

### Code Example from Main.java

```java
// Create factory that decorates with QuackCounter
AbstractDuckFactory duckFactory = new CountingDuckFactory();

// Create various ducks using the factory (Strategy pattern)
Quackable mallardDuck = duckFactory.createMallardDuck();
Quackable redheadDuck = duckFactory.createRedheadDuck();
Quackable rubberDuck = duckFactory.createRubberDuck();

// Adapt a goose to work as a duck (Adapter pattern)
Quackable gooseDuck = new GooseAdapter(new Goose());

// Group ducks into a flock (Composite pattern)
Flock flock = new Flock();
flock.add(mallardDuck);
flock.add(redheadDuck);
flock.add(rubberDuck);
flock.add(gooseDuck);

// Register observers (Observer pattern)
Quackologist quackologist = new Quackologist();
flock.registerObserver(quackologist);

// Simulate quacking
flock.quack();

// Display total quacks counted by decorator
System.out.println("Total quacks: " + QuackCounter.getQuacks());
```

## Key Design Principles Demonstrated

1. **Encapsulation**: Each pattern encapsulates behavior changes within specific classes
2. **Loose Coupling**: Objects interact through interfaces, not concrete implementations
3. **Single Responsibility**: Each class has one reason to change
4. **Open/Closed Principle**: Classes are open for extension, closed for modification
5. **Dependency Inversion**: Depend on abstractions, not concrete classes
6. **Composition over Inheritance**: Uses composition to build complex behaviors

## Running the Project

### Prerequisites

- Java Development Kit (JDK) 8 or higher
- An IDE like IntelliJ IDEA, Eclipse, or VS Code with Java support

### Compilation and Execution

```bash
# Navigate to the project directory
cd Duck_Simulator

# Compile all Java files
javac -d . src/*/*.java src/Main.java

# Run the main program
java Main
```

### Expected Output

```
--- Duck Simulator ---
Quack
Quack
Squeak
Honk
---
Total quacks: 3
```

(Note: The Quackologist observer may also print notifications for each quack depending on implementation)

## Learning Outcomes

By studying this project, you will understand:

- ✅ How to apply the Strategy pattern for interchangeable algorithms
- ✅ How the Observer pattern enables loose coupling and real-time notifications
- ✅ How the Composite pattern simplifies hierarchical object structures
- ✅ How the Adapter pattern bridges incompatible interfaces
- ✅ How the Decorator pattern adds features without modifying originals
- ✅ How the Factory pattern centralizes object creation
- ✅ How to combine multiple patterns in a cohesive system
- ✅ Best practices for object-oriented design

## Design Pattern Relationships

```
┌─────────────────────────────────────────────────────────┐
│                    Main (Orchestrator)                   │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ CountingDuckFactory  │ (Factory Pattern)
                │  + Decorator Support │
                └──────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
   Quackable          Quackable          Quackable
   (Strategy)        (Strategy)         (Strategy)
   • MallardDuck     • RedheadDuck      • RubberDuck

        ▲                  ▲                  ▲
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                    (wrapped with)
                           │
                    ┌──────▼──────┐
                    │QuackCounter │ (Decorator)
                    │  Decorator  │
                    └─────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
   ┌─────────────────────────────────────────┐
   │          Flock                          │
   │  (Composite Pattern)                    │
   │  • Manages multiple Quackable objects   │
   └─────────────────────────────────────────┘
        │                  │                  │
        ├─ DuckQuackable   ├─ GooseAdapter   ├─ Observer
        │                  │  (Adapter)      │ Pattern
        │                  │                 │
        └─────────────────────────────────────┘
```

## Extensions and Improvements

Possible enhancements to the project:

1. **Add More Duck Types**: Implement new duck species with different behaviors
2. **Expand Observer Functionality**: Create more observer types (Logger, Counter, etc.)
3. **Persistent Flocks**: Save and load flock configurations
4. **GUI Visualization**: Create a visual representation of the duck simulator
5. **Threading**: Implement async quacking with thread simulation
6. **Sound Simulation**: Integrate actual sound files for different quack types
7. **Statistics Collection**: Expand QuackCounter to track quacks per duck type

## Repository Information

- **Project Type**: Educational Design Pattern Demonstration
- **Language**: Java
- **Design Patterns**: 6 (Strategy, Observer, Composite, Adapter, Decorator, Factory)
- **Purpose**: Learning and understanding GoF design patterns

## Author Notes

This project is an excellent learning resource based on the principles outlined in "Head First Design Patterns" and provides practical examples of how design patterns solve real-world software design challenges. Each pattern is implemented in isolation but also demonstrates how they can work together harmoniously in a single application.

## License

This is an educational project. Feel free to use it for learning purposes.
