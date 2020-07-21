# Design Pattern Basics



# ***Creational Patterns***

## singleton




Singletons are used to ensure only one instance of a class. Singleton class also provides a global access point for that single object. Example in java: logger class.

We can create singleton by making the constructor private and creating a static method which returns the singleton object(private static).

  

We can also use the synchronized keyword in the getInstance() static method to ensure only one object is created even in a multi-threaded environment. This is called lazy initialization.

  

synchronization can be expensive, early initialization of a private static singleton object creates the object once the class is loaded(as static keyword is used). Now we can simply use the static getInstance() method to return the object reference.

  

## Factory

Let's say we have an abstract type Product and many concrete products. The client should not explicitly use new operator to create a concrete product every time. This usually involves if else statements to create a concrete product based on client input. Instead of this, we can create a ProductFactory which takes some kind of input(let's say id of product or type in string etc) and returns the concrete product. In this way, the client need not be even aware of the concrete implementation, and just use the Product reference everywhere.

  

One drawback with the above is that, if we add another concrete type, we need to add it in createProduct method of our ProductFactory. This violates the Open/Closed Principle.

  

We can enhance our ProductFactory by adding a new method called registerProduct which adds the productId as key with value as concrete product object(Map<Integer, Product>). Each concrete class can call this registerProduct. In this way, createProduct will simply return the concrete product type. Now we can add as many concrete types we want and our ProductFactory will create them without even modifying the createProduct method!

  

Factory and AbstractFactory are usually singletons.

  

## AbstractFactory

  

When we have multiple Product families to create, we use abstract factory. Abstract factory is a factory of factories. Our concrete factories implement the abstract factory. Instead of having only one concrete implementation of createProduct, we have multiple createProduct implementations in all the concreteFactories.

  

Lets understand with the following UML diagram. Here we have 2 kinds of products, Button and editBox. They have their concrete implementations, for example WindowsButton for windows os.

  

Now on runtime, we create concreteWindowsFactory if user selects windows. This concreteFactory will have implementations to return windows button and windows EditBox objects.

  

![](https://lh6.googleusercontent.com/moCsZfzibm3pVnHEq8RsnVH3xQoafQ119PMYQJKBP19YyyPfrl3StSXbdPE-UUXXGdCdLgeL9ucbbynjZe4ETPjMFuLdJmvVY2NuYHoH_3WqY3-4TVur7f6GXlCHNKwdMldIQ1gP)

  
  

## Builder


If we have to create a complex object and set many field values in it, calling a parameterized constructor looks messy. According to clean code concepts, we should not pass more than 3 arguments in a function. One could say we can call setters to set these fields individually. This is fine but then our object is mutable.

  

To solve this we can use builder pattern. We have a static inner class called builder. It has all the setters for our complex object and each returns “this”, i.e return type is Builder it self. After calling each setter(order doesn’t matter), we finally call the build method on the Builder which invokes the constructor of our complex object and returns us the final object.

  

    ComplexObject obj =
    
    ComplexObject.Builder.getInstance().setA(a).setB(b).setD(d).setC(c).setF(f).build();

  
thread safety is also provided if we use builder pattern with an inner static class, instead of just returing this on every setter call. Ex: new complexobj().setA(a).setB(b); settter returing "this"
https://www.geeksforgeeks.org/builder-pattern-in-java/

# ***Behavioral Patterns***

  

## Strategy

  

We may have different algorithms(or “Strategy”) to call which is decided at runtime. We define an interface called Strategy and the concrete strategies implement the algorithm. On runtime, the algorithm is called using Strategy reference.

  

For example, we have an interface called RobotBehavior which has an abstract method called move. Now we have 3 classes implementing RobotBehavior- NormalRobotBehavior, AggressiveRobotBehavior and DefensiveRobotBehavior. They implement the move() method in their own way.

  

## Command

Coupling the invoker of a request to a particular request should be avoided. That is, hard-wired requests should be avoided. Command pattern helps us to design in this manner.

  

![](https://lh6.googleusercontent.com/d4csQsIOqg7Ahd8eKEXJ3Uv-q-vgUCD25zdGfb4vXE62VD_b2m4kYXL3CCbVK1ZMr19jne9dGNVXfE1dDQCzn4M-f9OOvX8edhPDUHnkF55V0VaG0vbdv7lbObQMrClMJJA6sqM5)

  

In the above diagram, we can see that the invoker is not implementing the request directly. Instead it is calling execute method on a concrete command object which will call the appropriate action on the appropriate receiver. This makes invoker independent of how the command is executed.

  

The invoker may also have a queue and it calls all the commands one by from the queue.

  

Example of command: Undo and Redo operations.

  

We may have a receiver with methods undo and redo, ex Document class, and also an interface with execute method and 2 concrete command classes, RedoCommand and UndoCommand. These have a reference of the receiver, i.e Document object reference. The execute method implemented will call the appropriate method on the Document object(i.e doc.undo() or doc.redo()). An invoker class will have a queue to store Command(UndoCommand or RedoCommand) objects. The invoker will simply call the execute method on these Command objects. The client will set what kind of command object to pass to the invoker and also what kind of receiver these commands have(i,e Document).

  
  

## Chain of responsibility

We have a Handler interface and concrete handlers which are chained together. The request is fed to the first concrete handler and delegated to it’s successor if it can’t handle it. In this way, the client doesn’t know which concrete handler will handle the request.

  

It avoids attaching the sender of a request to its receiver, giving this way other objects the possibility of handling the request too.

  

Example: GUI event handling

  

## Observer

Define a one to many dependency. When the state of the subject changes, all its observers(or dependents) are notified and updated automatically.

  

Example: Stock market server(subject) and various clients(web app, smartphone app) etc.

  
  

# ***Structural Patterns***



## Adapter

  

Work together with incompatible classes. Adapter acts as an interface between them. It can also be thought of some kind of “Wrapper”.

  

The client only interacts with the Target interface. The adapter implements the target interface and delegates the request to the adaptee.

  

All the concrete classes implementing Target are of type Target. We can call target methods on them directly. But we want to use target method with some other class(adaptee) which doesn’t implement target and has its own methods.

  

With the use of adapter class(which implements Target), we can implement Target methods and call adaptee’s specific methods in them. In this way, we have created a wrapper around the adaptee and now it's like it is also implementing Target interface.

  

## Decorator


Using decorator pattern, we can add new functionality at the run time instead of statically extending functionality at compile time using inheritance. All the decorators simply act as a “wrapper”.

  

Lets say we have a simple Window interface. We have PlainWindow class which implements Window interface. Now we want to add new features to the window like make it scrollable and add a frame to it. We can create a WindowDecorator class which will implement Window. Now we can create multiple classes extending WindowDecorator which will add functionality to objects of type Window, without touching any concrete Window classes.

  

    Window windowWithFrame = new FramedWindow(new PlainWindow());
    
    windowWithFrame.draw()

    // the draw method will first add a frame and then call draw on the Window reference we passed to it in the constructor. (i.e PlainWindow)

  

## Flyweight


We can use flyweight design pattern to share the intrinsic state for many objects, i.e many objects can share the common state while other part of state can vary.

  

For example: Consider a game in which soldiers battle. The graphical representation of many soldier objects can be same and thus can be stored in a concrete flyweight like SoldierGraphicalImpl. The behavior of the soldier which has to extrinsic like soldier health and soldier ammo can be stored in the soldier objects explicitly.
