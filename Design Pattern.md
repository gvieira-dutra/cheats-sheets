## SOLID

### Single Responsibility Principle

- Methods and Classes should have one single responsibility
- Methods should have 1 intention!!! (preferably small)
- High cohesion among methods and classes

### Open Closed Principle

- Keep the closed for changes but open for extensions

### Liskov Substitution Principle

-

### Dependency Inversion Principle

- Concrete classes should inherit from abstract classes, not concrete ones.

## DRY Don't Repeat Yourself

# Design Patterns

## Factory Pattern

Isolate the creation of complex objects, keeping same configurations but abstracting it to maintain responsibility. Allows subclass to alter the type of objects that will be created.
Eg: Instead of creating the db connection inside class that will use it, we create a class responsible for creating this connection.

## Flyweight

If a class has multiple objects that repeat themselves, instead of instantiating each of them we can create a dictionary of objects and create methods to retrieve these objects. Then, instead of instantiating them every time, we'd just create one instance that will be retrieved when needed.
Eg: A music is made of several notes. If I am creating musical notes, a dictionary of notes can be used so they are instantiated once and each note can be retrieved when they are needed on the music.

## Previous States and Moment

A strategy to keep previous status of an object. Such as a contract with states like: New, InEvaluation, Ongoing, Closed. Here we can create a class that holds the current status every time the status is changed, they should be stored in the same order as the statuses change and we can either store an instance of a class to save the previous state or we can just save the state's information such as a string or enum.

## Interpreter

A pattern used when we have a expression tree we want to evaluate. It is for example useful when we have a math expression such (1 + 2) + ( 3 - 4) and we must create a tree like order of execution to evaluate lower operations first and then evaluate the upper portion.
Can also be performed with the C# resource `Expression`

## Visitor

A pattern where we separate algorithms from the object structure which they operate. Back to the calculator example, to print the expression, like a binary search we'd start printing nodes from left to right on left branch first, then print the upper add symbol, and print the numbers and symbol on the right branch.

## Bridge

Made to separate responsibilities in class hierarchies. In classes with more than 1 responsibility, this pattern helps us to create interfaces to abstract these responsibilities and facilitate their management. For example a class responsible for sending messages, that can be sent to a user or to an admin, via email or text. This patter will help us abstract each portion of these responsibilities, then we can do class composition to make the classes behave like we want to.

## Command

This pattern helps us create classes to store commands that will be executed later on. For example if we have a store that must process many orders, this will help us save the orders to be executed later in the case where there are too many to handle. This helps us manage memory better and ensure that data is not lost during the process.

## Adapter

The adapter pattern is use to adapt legacy code to fit oop new code base. This legacy code would not be able to fit in the software's logic or new functionalities then after the adapter pattern being applied to it it becomes integrated with the solution.

## Facades & Singleton

(older pattern) Facades are resources used to encapsulate complex logic into these facades, they they are the ones used to manage our object, instead of managing the object's methods themselves.
A singleton instance of a facade can be instantiated and this instance would be the only one on the program, allowing the facade's methods to be used at any time without the need to instantiate the facade again.

# Perguntas

Usar ou não usar 'var'?
