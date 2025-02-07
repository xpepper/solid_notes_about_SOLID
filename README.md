# Design principles: SOLID

Personal digest of [Clean Architecture: A Craftsman's Guide to Software Structure and Design by Robert C. Martin (2017)](https://g.co/kgs/554Z2i), [Design Principles and Design Patterns by Robert C. Martin (2000)](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf) & [else around the internet](#links) about SOLID principles.

## Content
1. [1st: Symptoms of rotting design - poor architecture](#symptoms)
2. [2nd: Causes to rot](#causes)
3. [3rd: Solution - Principles of Object Oriented Class Design](#solution)  
3.1 [What is SOLID](#what_is_solid)  
3.2 [Goal of SOLID](#goal_of_solid)  
3.3 [SRP - Single Responsibility Principle](#srp)  
3.4 [OCP - Open-Closed Principle](#ocp)  
3.5 [LSP - Liskov Substitution Principle](#lsp)  
3.6 [ISP - Interface Segregation Principle](#isp)  
3.7 [DIP - Dependency Inversion Principle](#dip)  
4. [Concepts :bulb:](#concepts)
5. [Links](#links)



## <a name="symptoms">1st: Symptoms of rotting design - poor architecture</a>

:triangular_flag_on_post: ***Rigidity*** *(stiffness, fixed)*  
When easy change causes cascade of changes in dependent modules. If manager’s start to fear to fix problems or refuse to allow changes - design deficiency becomes an adverse management policy.  
:triangular_flag_on_post: ***Fragility***  
Breaks everytime changed. Impossible to maintain & probability of breakage increases with time.  
:triangular_flag_on_post: ***Immobility***  
Inability to reuse. Module has too much baggage it depends upon. Safer is to rewrite than reuse.  
:triangular_flag_on_post: ***Viscosity*** *(swamp)*  
Easy to do the wrong thing, hard to do the right thing. Viscosity of environment - long compiles, source code control system requires a long time to check-in - engineers' temptation to todo as few check-in’s as possible/make changes that don’t force large recompiles.

## <a name="causes">2nd: Causes to rot</a>

- Changing requirements - initial design left behind - design not resilient to changes.
- New unplanned dependencies between modules of software. Need of ependency management - creation of dependency firewalls.

## <a name="solution">3rd: Solution - Principles of Object Oriented Class Design</a>
## <a name="what_is_solid">3.1 What is SOLID</a>

The SOLID principles - principles, that tell us how to arrange our functions and data structures into classes, and how those classes should be interconnected.

## <a name="goal_of_solid">3.2 Goal of SOLID</a>

The goal of the principles is the creation of [mid-level💡](#mid-level) software structures that: 
1. Tolerate change
2. Are easy to understand
3. Make components to be reused in many software systems


## <a name="srp">3.3 SRP - Single Responsibility Principle</a>

> There should never be more than one reason for a class to change.

or

> Each software module should have one and only one reason to change.

or

> A module should be responsible to one, and only one, actor. 

### What SRP is NOT:  
“A function should do one, and only one, thing.” - it’s a refactoring principle, to refactor large functions into smaller functions; we use it at the lowest levels. But it’s NOT the SRP.

### What SRP IS:
- SRP is the reason we separate concerns.
- Gather together the things that change for the same reasons. Separate those things that change for different reasons.
- We want to increase the [cohesion💡](#cohesion) between things that change for the same reasons, and we want to decrease the [coupling💡](#coupling) between those things that change for different reasons.
- An active corollary to [Conway’s law💡](#conways-law): The best structure for a software system is heavily influenced by the social structure of the organization that uses it so that each software module has one, and only one, reason to change.

### What is the reason to change?
- When you write a software module, you want to make sure that when changes are requested, those ***changes can only originate from a single person***, or rather, a single tightly coupled group of people representing a single narrowly defined business function. You want to isolate your modules from the complexities of the organization as a whole, and design your systems such that each module is responsible (responds to) the needs of just that ***one business function***.

### Symptoms of SRP violation

1. Accidental duplication. These problems occur because we put code that different actors depend on into close proximity. The SRP says to separate the code that different actors depend on.

2. Merges. Multiple people changing the same source file for different reasons. To avoid this problem is to separate code that supports different actors. Solution - move the functions into different classes. 

SRP appears at two different levels:
- Level of components (relates to [Common Closure Principle :link:](https://ericbackhage.net/clean-code/the-common-closure-principle/))
- Architectural level (relates to [Axis of Change💡](#axis-of-change))

## <a name="ocp">3.4 OCP - Open-Closed Principle (goal of OO architecture)</a>

> A module should be open for extension but closed for modification.

### Main idea
Software must be designed to allow the behavior of it to be changed by adding new code, rather than changing existing code.

### Goal of OCP
To make the system easy to extend without incurring a high impact of change. Modules are extensible without being changed, ability to add new features without changing existing code.

If simple extensions to the requirements force massive changes to the software, then the architects of that software system have engaged in a spectacular failure.

### Take in account
- Directional control (make sure that the dependencies between the components pointed in the correct direction)
- Information hiding (protect from knowing too much)

***Technique***: Abstraction.  
***Solutions***:  

| Solution  | Description |
| ------------- | ------------- |
| Dynamic polymorphism  | Run time. Overriding, virtual functions (depending on interfaces, new types get new class, no main function that handles types with switch…case, no modification when adding new type)  |
| Static polymorphism  | Compile time. Overloading (templates or generics)  |   

Here is an illustration on how using Open-Closed Principle looks like when adding new feature:
<img src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*Cuafrk_ZgQtKK6n4HBT_fQ.png" width="700" height="400"/>

## <a name="lsp">3.5 LSP - Liskov Substitution Principle</a>

> Subclasses should be substitutable for their base classes.

or

> Methods that use references to base classes must be able to use objects of derived classes without knowing it.

<img src="https://media.giphy.com/media/l36kU80xPf0ojG0Erg/giphy.gif" width="600" height="400"/>

### About LSP
Substitutable - capable of being exchanged.
Users of a base class should continue to function properly if a derivative of that base class is passed to it.
Violations of LSP are latent (hidden) violations of OCP.
Repercussions (unintended consequences) of LSP violation can be using if/else statements as a fix for such violations.

### Contracts

| Contract  | Description |
| ------------- | ------------- |
| Implicit contract | Set of rules or constraints that a subclass must adhere to in order to be used as a substitute for its superclass. This contract is implicit because it is not explicitly defined, but rather is implied by the behavior of the superclass and the relationship between the subclass and superclass. |  
| Explicit contract | Set of rules or constraints that a subclass must adhere to in order to be used as a substitute for its superclass. These rules or constraints are explicitly defined in the code, documentation, or other forms of communication. This allows other developers to understand how to use the subclass and what behavior to expect from it. By adhering to an explicit contract, the subclass can be used in place of the superclass without causing any unexpected behavior or errors in the code. |  

To build software from interchangeable parts, those parts must adhere to a contract that allows those parts to be substituted one for another.

### Solutions:
1. Design by contract (contract of the base class must be honored by the derived class)
2. Derived class is substitutable (capable of being exchanged) for its base class if:
   * Its preconditions (declarations that are true before the method is called) are no stronger than the base class method.
   * Its postconditions (declarations what method guarantees will be true once its completed) are no weaker than the base class method.
5. Derived methods should expect no more and provide no less.

## <a name="isp">3.6 ISP - Interface Segregation Principle</a>

> Many client specific interfaces are better than one general purpose interface.

***Technique***:  
- If you have a class that has several clients, rather than loading the class with all the methods that the clients need, create specific interfaces for each client and multiply inherit them into the class.
- Without ISP components and classes would be much less useful and portable.
- Avoid depending on things that you don’t use.

### Solutions:

| Solution  | Description |
| ------------- | ------------- |
| Client Specific | Categorizing clients by their type, and interfaces for each type of client should be created. If two or more different client types need the same method, the method should be added to both of their interfaces. |  
| Changing Interfaces | Clients of the old interface that wish to access methods of the new interface, can query the object for that interface (but do not overdo it). |  

## <a name="dip">3.7 DIP - Dependency Inversion Principle (primary mechanism of OO architecture)</a>

> Depend upon Abstractions. Do not depend upon concretions.

### About DIP
It’s a strategy of depending upon interfaces or abstract functions & classes, rather than upon concrete functions & classes.

### Problem
Procedural architecture - dependency structure - like hierarchy in organisation management. High level modules deal with the high level policies of application. High level modules directly depend on implementation modules.

### Practices
- Don’t refer to volatile concrete classes. Don’t refer to volatile concrete classes.
- Don’t derive from volatile concrete classes
- Don’t override concrete functions. Concrete functions often require source code dependencies. When you override those functions, you do not eliminate those dependencies—indeed, you inherit them. To manage those dependencies, you should make the function abstract and create multiple implementations.
- Never mention the name of anything concrete and volatile

### Solution: Object-oriented architecture
1. Dependencies point towards abstractions
2. Dependencies are inverted (upside-down)
3. Modules with implementation are not depended upon (main modules -> mid modules -> detail) but depend themselves upon abstractions (high level  -> abstraction <- detail)
4. Depending upon abstractions - every dependency in design should target an interface, or an abstract class. No dependency should target a concrete class.
5. Abstractions are “hinge points”, they represent the places where the design can bend or be extended, without themselves being modified (OCP).
6. Abstract factory

### Motivation
- Concrete things change a lot, abstract - less frequently.
- DIP prevents from depending upon volatile (nepastovių) modules.
- Anything concrete is volatile (exceptions in early development)
- Non-volatility is not a replacement for the substitutability of an abstract interface. Consistency < interface.
- Concrete class design creates instances -> littering architecture with dependencies upon abstract classes.

##

## <a name="concepts">Concepts :bulb:</a>

**<a name="mid-level">💡 Mid-level</a>**  
Principles are applied by programmers working at the module level. Applied just above the level of the code and help to define the kinds of software structures used within modules and components. 

**<a name="conways-law">💡 Conway’s law</a>**  
An adage that states organizations design systems that mirror their own communication structure. It’s an observation that the architectures of software systems look remarkably similar to the organization of the development team that built it.

**<a name="cohesion">💡 Cohesion (bond)</a>**  
The degree to which the elements inside a module belong together. In one sense, it is a measure of the strength of relationship between the methods and data of a class and some unifying purpose or concept served by that class. The force that binds together the code responsible to a single actor. 

**<a name="coupling">💡 Coupling</a>**  
Degree of interdependence between software modules.

**<a name="axis-of-change">💡 Axis of Change</a>**  
An idea that changes in a system tend to occur around the axis of a class's responsibility. When a change is needed, it's likely to be in the area of a class's responsibility. By having each class focus on a single responsibility, it becomes easier to manage changes because they are likely to be localized to specific classes.

## <a name="links">Links of references</a>
1. [Clean Architecture: A Craftsman's Guide to Software Structure and Design by Robert C. Martin](https://g.co/kgs/554Z2i)
2. [Design Principles and Design Patterns by Robert C. Martin (2000)](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf)
3. [Wikipedia about Cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science))
4. [Wikipedia about Coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming))
5. [Wikipedia about Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law)
6. [Conway's Law by Martin Fowler](https://martinfowler.com/bliki/ConwaysLaw.html)
7. [Common Closure Principle](https://ericbackhage.net/clean-code/the-common-closure-principle/)
8. [Common Closure Principle: The story of an evolving architecture](https://blog.devgenius.io/common-closure-principle-the-story-of-an-evolving-architecture-6919b452c8db)
9. [SOLID Principles Swift](https://github.com/Vinodh-G/SOLID-Principles-Swift)
