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
    Tradeoff: observer vs violate Design Principles; balance transparency and safety.
    NullIterator.

## 10. The State Pattern: The State of Things

## 11. The Proxy Pattern: Controlling Object Access
    A remote proxy acts as a local representive to a remote object.
    RMI: remote method invocation.
    RMI Nonmenclature: in RMI, the client helper is a 'stub' and the service helper is a 'skeleton'.
    Dynamic class downloading: With it, serialized objects (like the stub) are "stamped"
        with a URL that tells the RMI system on the client where to find the class file for that object.
    "transient" tells the JVM not to serialize this field.
    A remote proxy controls access to a remote object;
    A virtual proxy controls access to a resource that is expensive to create.
    A protection proxy controls access to a resource based on access rights.
    java.lang.reflect: create a proxy class on the fly, which is a dynamic proxy.
    Firewall Proxy, Smart Reference Proxy, Caching Proxy, Complexity Hiding Proxy, Copy-on-Write Proxy

## 12. Compound Patterns: Patterns of Patterns
    Pattersn are often used together and combined within the same design solution.
    A compound pattern combines two or more patterns into a solution that solves a recurring or general problem.

## 13. Better Living with Patterns: Patterns in the Real World
    Pattern Categories: Creational-- Singleton, Abstract Factory, Factory Method, Builder, Prototype
                        , Behavioral-- Template Method, Iterator, Command, Observer, State, Strategy, Visitor, Mediator, Memento, Interpreter, Chain of Responsiblity
                        , Structural-- Decorator, Composite, Adapter, Proxy, Facade, Flyweight, Bridge
    Think in Patterns:
        1. Keep it simple (KISS)
        2. Refactoring time is Patterns time! The goal is to improve its structure, not change its behavior.
        3. Take out what you don't really need. Don't be afraid to remove a Design Pattern from your design. Patterns are powerful but complicated.
    Complexity and patterns should only be used where they are needed for practical extensibility.
    The Zen mind is able to see patterns where they fit natrually.
    Overuse of design patterns can lead to code that is downright over-engineered. Always go with the simplest solution tha tdoes the job and introduce patterns
        where the need emerges.
    Patterns are tools not rules.
    The gang of four: GoF.
    Books:
        1.Design Patterns: Elements of Reusable Object-Oriendted Software.
        2. The Timeless Way of Building.
        3. A Pattern Language.
    Websites:
        [The Porland Patterns Repository](http://c2.com/cgi/wiki?WelcomeVisitors)
        [Hillside Group](http://hillside.net)
    Patterns Zoo:
        1. Architectural Patterns: used to create the living, vibrant architecture of buildings, towns, and cities. This is where patterns got their start.
        2. Application Patterns: for creating system level architecture. many multi-tier architectures fall into this category: MVC
        3. Domain-specific Patterns: that concern problems in specific domains like concurrent systems or real-time systems: J2EE.
        4. Business Process Patterns: describe the interaction between businesses, customers and data, and can be applied to problems such as how to effictively make and commuincate decisions.
        5. Organizational Patterns: describe the strucures and practices of human organizations. Most efforts to date have focused on organizations that produce and/or support software: Develpment team, Customer support team.
        6. User Interface Design Patterns: address the problems of how to design interactive software programs.
    An Anti-pattern tells you how to go from a problem to a BAD solution, e.g. Golden Hammer.

## 14. Leftover Patterns:
    1.Bridege: to vary not only your implementations, but also your abstractions.
    2.Builder: to encapsulate the construction of a product and allow it to be constructed in steps.
    3.Chain of Responsiblity: when you want to give more than one object a chance to handle a request.
    4.Flyweight: When one instance of a class can be used to provide many "virtual instances".
    5.Interpreter: to build an interpreter for a language.
    6.Mediator: to centralize complex communications and control between related objects.
    7.Memento: When you need to be able to return an object to one of its previous states; for instace, if your user request an "undo".
    8.Prototype: When creating an instance of a given class is either expensive or complicated.
    9.Visitor: When you want to add capabilities to a composite of objects and encapsulation is not important.

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
        Iterator<interface>: hasNext(); next();
        Client call createIterator()

    - Composite: allows you to compose objects into tree structures to represent part-whole
        hierarchies. Composite lets clients treat individual objects and compositions of
        objects uniformly.
        Component<abstract class>: add(Component); remove(Component); getChild(i);
                                   getName(); getDescription(); print();// by default throw exception or else
        SpecificComponent: implementing Component

    - State: allows an object to alter its behavior when its internal state changes.
        The object will appear to change its class.
        Context: holding different states and current state points to one of them. Actions are
            delegated to state.
        State<interface>: all actions();
        SpecificState: implementing State and doing state transition.

    - Proxy: provides a surrogate or placeholder for another object to control access to it.
        Use the Proxy Pattern to create a representative object that controls access to another
            object, which may be remote, expensive to create or in need of securing.
            remote proxy:
                server: implement remote interface; generate stub and skeleton; register it and
                    run service.
                client: get service by looking up, and call method on stub object.
            virtual proxy:
