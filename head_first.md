Pay attention to how you pay attention.

## 1. Introduction to design pattern
    Instead of code resue, with patterns you get experience reuse.
    Design pattern helps to structure our application and communicate more with each other
using less words.
    OO basics: Abstraction, Encapsulation, Polymorphism, and Inheritance.
    Good OO desings are resuable, extensible and maintainable.

## 2. The Observer Pattern: Keeping your Objects in the konw
    Strive for loosely coupled designs between objects that interact. When two objects are loosely coupled, they can interact, but have
very little knowledge of each other.
    Java has built-in support for Observer pattern:
        - Observer<interface>: void update(Observable o, Object arg): if arg == null, pull; else push
        - Observable<class>: addObserver(Observer o); clearChanged(); countObservers();
                             deleteObserver(Object o); deleteObservers(); hasChanged();
                             notifyObservers(); notifyObservers(Object arg); setChanged()
                setChanged() is protected and means it cannot be called in non-package
                    non-subclass, which implies composition is not viable.
    Observer pattern is heavily used in GUI frameworks and other places, including JavaBeans
        and RMI.

## 3. The Decorator Pattern: Decorating Objects
    Code should be closed to change, yet open to extension.
    Decorator has the same supertype as the object it decorates.
    Java.io:      InputStream<component>-------> 1. FileInputStream, 2. StringBufferInputStream, 3. ByteArrayInputStream
                  FilterInputStream<decorator>: 1. PushbackInputStream, 2. BufferInputStream,
                        3. DataInputStream, 4. LineNUmberInputStream
    Composition and delegation can often be sued to add new behaviros at runtime.

## 4. The Factory Pattern: Baking with OO Goodness.
    When you see "new", think of concrete.
    Simple factory idiom: one object handles the instatiation of concrete classes.
    Factory method<abstract>: implemented by subclasses. This decouples the client
        code in the superclass from te object creation code in the subclass.
    Abstract factory.
    Factories are a powerful technique for coding to abstractions, not concrete classes.

## 5. The Singleton Pattern: One of a Kind Objects
    Synchronization a method can decrease performance by a factor of 100.
        - Lazy instantiation
        - ?Move to an eagerly created instance rather than a lazily created one.
        - Double checked locking:
                private volatile static Singleton unqiueInstance;
                private Singleton() {}
                public static Singleton getInstance() {
                    if (uniqueInstance == null) {
                        synchronized (Singleton.class) {
                            if (uniqueInstance == null) {
                                uniqueInstance = new Singleton();
                            }
                        }
                    }
                }

## 6. The Command Pattern: Encapsulating Invocation
    A nul object is useful when you don't have a meaningful object to return, and yet
        you want to remove the responsibility for handling null form the client.
    Macrocommands.
    Undo.
    Queueing requests:
        Commands give us a way to package a piece of computation(a reciever and a set of action)
        and pass it around as a first-class object. Now, the computation itself may be invoked
        long after some client application creates the comand object. In fact, it may even be
        involked by a different thread, e.g. thread pool.
    Logging and transactional requests: store() and load().

## 7. The Adapter and Facade Patterns: Being adpative
    Object adpaters and class adapters(in multiple inheritance language) use two different
        means of adapting the adaptee (composition versus inheritance).

## 8. The Template Method Pattern: Encapsulating Algorithms
    A hook is a method that is declared in the abstract class, but only given an empty or
        default implementation, giving subclasses the ability to hook into the algorithm or
        ignore the hook.

## 9. The Iterator and Composite Patterns: Well-Managed Collections
    Cohesion: a module or class has high cohesion when it is designed around a set of related
        functions, and we say it has low cohsesion when it is designed around a set of
        unrelated functions.

# Design Principle:
   - Encapsulate what varies. Identify the aspects of your application that vary
and separate them from what stays the same. That is the basis for almost every design pattern.

   - Program to an interface, not an implementation. It means to exploit polymorphism
by programming to a supertype so that the actual runtime object isn't locked into the
code. Behaviors will live in a seperate class. Delegate the behavior into instace variables,
not in the implementation
of superclass or subclass.

   - Favor composition over inheritance. Can change behavior at runtime.
Has-A can be better than a IS-A.

    - Strive for loosely coupled designs between objects that interact.

    - Open-Closed Principle: Classes should be open for extension, but closed for modification.

    - Dependency Inversion: Depend upon abstractions. Do not depend upon concrete classes.
        Guidelines: - No variable should hold a reference to a concrete class. If you use new,
                        you'll be holding a reference to a concrete class. Use a factory to get
                        around that.
                    - No class should derive from a concrete class. If you derive from a concrete
                        class. Derive from an abstraction, like an interface or an abstract class.
                    - No method should override an implemented metohd of any of its base classes.
                        If you override an implemented method, then your base class wasn't
                        really  an abstraction to start with. Those methods implemented in the
                        base class are meant to be shared by all your subclasses.

    - Principle of Least Knowledge: talk only to your immediate friends, i.e., the Law of Demeter.
        Guidelines: only invoke methods that belong to:
                        - The object itself;
                        - Objects passed in as a parameter to the method;
                        - Any object the method creates or instantiates;
                        - Any components of the object, i.e., an instance variable.

    - The Hollywood Principle: Don't call us, we'll call you.

    - Design Principle: A class should have only one reason to change.

# Design Pattern:
    - Strategy(algorithms): defines a family of algorithms, encasulates each one, and makes
        them interchangable. Strategy lets the algorithms vary independently from clients
        that use it.
        Duck: quackBehavior, flyBehavior, performQuack(), performFly(),
            setQuackBehavior(), setFlyBehavior()
        MallardDuck extends Duck, gets Behavior instance in constructors.
        QuackBehavior interface: quack()
        FlyBehaviro interface: fly()
        Quack, Squeak, MuteQuack implement QuackBehavior;
        FlyWithWing and FlyWithoutWing implement FlyBehavior.

    - Observer: defines a one-to-many dependency between objects so that when one
        object changes state, all of its dependencts are notified and updated automatically.
        Subject<interface>: registerObserver(), removeObserver(), notifyObserver();
        ConcreteSubject: registerObserver(), removeObserver(), notifyObserver(), getState(), setState()
        Observer<interface>: update()
        ConcreteObserver: update()

    - Decorator: attaches additional responsiblities to an object dynamically. Decorators
            provide a flexible alternative to subclassing for extending functionality.
        Component<abstract class or interface>: implemented by ConcreteComponent;
        AbstractDocorator extends Components: each Docorator extends AbstractDecorator
            and holds an instance variable to its decorated component.

    - Factory Method: defines an interface for creating an object, but lets subclasses
        decide which class to instantiate. Factory Methods lets a class defer instantiation
        to subclass.
            Creator<abstract>: orderPizza calls abstract createPizza, and other methods
                might call createPizza too.
            ConcreteCreator: createPizza()
            Product<abstract>: common interface that the classes consuming the products can
                                refer to the interface.
            ConcreteProduct: implement
    - Abstract Factory: provides an interface for creating families of related or dependent
        objects without specifying their concrete classes.
            IntegredientFactory<Interface>: createDough(), createSauce()
            NyIntegredientFactory: implementing the interface above
                To use abstract factory, instantiate one and pass it into some code that is
                writen against the abstract type.

    - Singleton: ensures a class has only one instance, and provides a global point of
        access to it.

    - Command: encapsulates a request as an object, thereby letting your parameterize
        other objects with different requests, queue or log requests, and support undoable
        operations.
        Command<interface>: execute(), undo(), store(), load()
        CommandObject: implementing command and hold a reference to vendor class and delegate
            all operations to vendor class.
        When you need to decouple an object making requests from the objects that know how
            to perform the requests, use the Command Pattern.

    - Adapter: converts the interface of a class into another interface the clients expect.
                Adpater lets classes work together that couldn't otherwise because of incompatible
                interfaces.

    - Facade: provides a unified interface to a set of interfaces in a subsystem. Facade defines
                a higher-level interface that makes the subsystem easier to use.

    - Template Method: defines the skeleton of an algorithm in a method, deferrring some steps to
                        subclasses. Themplate method lets subclasses redefine certain steps
                        of an algorithm witout changing the algorithm's structure.

    - Iterator: provides a way to access the elements of an aggregate object sequentially
        without exposing its underlying representation.