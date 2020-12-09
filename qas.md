

## 1. Describe OOP principles


##### Inheritence
Inheritance is a mechanism in which one object acquires all the states and behaviors of a parent object.

##### Abstraction
Is process of hiding implementation details and showing functionality.

##### Polimorhphism
Is ability of class to take many forms. Overriding and overloading.

##### Encapsulation
Encapsulation is a process of wrapping code and data together into a single unit.

## Dependency Injection vs Inversion of Control
IoC is a generic term meaning that rather than having the application call the implementations provided by a library (also know as toolkit), a framework calls the implementations provided by the application.
DI is a form of IoC, where implementations are passed into an object through constructors/setters/service lookups, which the object will 'depend' on in order to behave correctly.


## 2. Programing principles

##### SOLID principles


    Single Responsibility
    Open/Closed
    Liskov Substitution
    Interface Segregation
    Dependency Inversion
 
 
	Single Responsibility - a class should only have one responsibility. Furthermore, it should only have one reason to change.
	Open/Closed - classes should be open for extension, but closed for modification. In doing so, we stop ourselves from modifying existing code and causing potential new bugs.
	Liskov Substitution - if class A is a subtype of class B, then we should be able to replace B with A without disrupting the behavior of our program. 
    Preconditions cannot be strengthened in a subtype.
    Postconditions cannot be weakened in a subtype.
    Invariants of the supertype must be preserved in a subtype.

	Interface Segregation - (Interface cohesion)larger interfaces should be split into smaller ones. By doing so, we can ensure that implementing classes only need to be concerned about the methods that are of interest to them.
	Dependency Inversion - The principle of Dependency Inversion refers to the decoupling of software modules. This way, instead of high-level modules depending on low-level modules, both will depend on abstractions.
	
##### KISS (Keep It Simple Stupid)
If you have a complex piece of code, always try to break it into smaller more maintainable code.

##### YAGNI (You Aren’t Gonna Need It)

##### DRY (Don’t Repeat Yourself)

##### SoC (Separation of Concerns)
SoC principle is all about minding your own business — literally. This principle advises you to partition your complex code into different sections or domains.

## 3. Class Relations

    Association : relationship where all objects have their own life-cycle & there is no ownership. eg: teacher-student
    Aggregation : special type of association, separate life cycle but there is ownership eg : department- teacher
    Composition : Special type of aggregation, no separate life cycle and if parent deleted, all child will get delete.
	
## 4. What is JVM
The JVM has two primary functions: to allow Java programs to run on any device or operating system (known as the "Write once, run anywhere" principle), and to manage and optimize program memory.  The JVM is the specification for a software program that executes code and provides the runtime environment for that code. There are many JVM, most popular is  Oracle’s HotSpot. 

## 5. What is JIT compiler
The Just-In-Time (JIT) compiler is a component of the runtime environment that improves the performance of Java™ applications by compiling bytecodes to native machine code at run time.
The JIT compiler is enabled by default. Often-used methods are compiled soon after the JVM has started, and less-used methods are compiled much later, or not at all. 
The JIT compiler can compile a method at different optimization levels: cold, warm, hot, veryHot, or scorching (see optlevel in -Xjit). Higher optimization levels are expected to provide better performance, but they also have a higher compilation cost in terms of CPU and memory. 

## 6. What is GC

Java garbage collection is the process by which Java programs perform automatic memory management. When Java programs run on the JVM, objects are created on the heap, which is a portion of memory dedicated to the program. Eventually, some objects will no longer be needed. The garbage collector finds these unused objects and deletes them to free up memory.
The heap is divided into three sections:
Young Generation: Newly created objects start in the Young Generation. The Young Generation is further subdivided into an Eden space, where all new objects start, and two Survivor spaces, where objects are moved from Eden after surviving one garbage collection cycle. When objects are garbage collected from the Young Generation, it is a minor garbage collection event.
Old Generation: Objects that are long-lived are eventually moved from the Young Generation to the Old Generation. When objects are garbage collected from the Old Generation, it is a major garbage collection event.
Permanent Generation: Metadata such as classes and methods are stored in the Permanent Generation. Classes that are no longer in use may be garbage collected from the Permanent Generation

HotSpot has many GC on one default is G1

## 7. Exception handling in Java
The basic purpose of exception handling is to maintain the normal flow of the application.
There are mainly two types of exceptions as mentioned below
    Checked exception - are exceptions that are declared in the throws clause of a method
    Unchecked exception -  do not need to be declared in a throws clause. They extend RuntimeException.	It is irrecoverable like VM (virtual machine) error, out of memory, system failure etc.
	
## 8. Finally block with DB
Within your finally block you need a second, nested try that has a catch block which just logs any exception. I think that I would even put a try-catch around each of the close() method calls so that if one fails, the other resources will stll be released.

```java
finally {
              if(resultSet !=null) {
                 try{ resultSet.close();} catch(SQLException e) {//logt it.}
              }
              if(stmt !=null) {
                 try{ stmt.close();} catch(SQLException e) {//log it.}
              }
  
              if(conn !=null) {
                 try{ conn.close();} catch(SQLException e) {//log it.}
              }
    
}
```

## 9. Usage of lambda expressions in Java
A Java lambda expression is thus a function which can be created without belonging to any class. A Java lambda expression can be passed around as if it was an object and executed on demand. If not interface is given its type is `Function<T, R>`. A Java lambda expression is essentially an object.
 Java lambdas can capture the following types of variables:
- Local variables
- Instance variables
- Static variables

With lambda expressions the type can often be inferred from the surrounding code.
Java lambda expression can implement interfaces with more than one method - as long as the interface only has a single unimplemented (AKA abstract) method. We can decorete it with optional @FunctionalInterface.

`MyPrinter myPrinter = s -> System.out.println(s);`
`MyPrinter myPrinter = System.out::println;`

```java
public class StringConverter {
    public int convertToInt(String v1){
        return Integer.valueOf(v1);
    }
}
StringConverter stringConverter = new StringConverter();

Deserializer des = stringConverter::convertToInt;
```
## 10. Describe how to test your code in correct way. Unit, Integration, End to End
There are approaches like TDD where you can write you test before you implement code. Other technique are also used as Behaviour driven test (Cucumber) or Keyword driven testing. Also it is important to use CI to autotest what is pushed to SVC.

Some test methods include:
-Unit testing
- Static code analysis with SonarQube
-Integration testing - where we test interaction in components
-End-to-end testing - where we test whole flow of actions in regards to Use Case.
- Black box testing - is similar to End-To-End but we control input and output.
- Auto testing - is testing UI with WebDriver/Appium, commonly PageObject is used here
- Performance testing - using tools like Jmeter we can test perf.
- System integration testing

##### Given, When, Then
- Given (Input): Test preparation like creating data or configure mocks
- When (Action): Call the method or action that you like to test
- Then (Output): Execute assertions to verify the correct output or behavior of the action.
```java
@Test
public void findProduct() {
    insertIntoDatabase(new Product(100, "Smartphone"));

    Product product = dao.findProduct(100);

    assertThat(product.getName()).isEqualTo("Smartphone");
}
```
-Use the Prefixes “actual*” and “expected*”
- Use Fixed Data Instead of Randomized Data
- Write Small and Specific Tests
- Heavily Use Helper Functions
- Assert Only What You Want to Test
- Don’t Rewrite Production Logic
- Test Close To The Reality
- If your database contains SQL specifics binded to database use Docker container for testing
- Use descriptive text for assertions with  AssertJ
- Use Parameterized Tests 
```java
@ParameterizedTest
@ValueSource(strings = ["§ed2d", "sdf_", "123123", "§_sdf__dfww!"])
public void rejectedInvalidTokens(String invalidToken) {
    client.perform(get("/products").param("token", invalidToken))
            .andExpect(status().is(400))
}

@ParameterizedTest
@EnumSource(WorkflowState::class, mode = EnumSource.Mode.INCLUDE, names = ["FAILED", "SUCCEEDED"])
public void dontProcessWorkflowInCaseOfAFinalState(WorkflowState itemsInitialState) {
    // ...
}

@ParameterizedTest
@CsvSource({
    "1, 1, 2",
    "5, 3, 8",
    "10, -20, -10"
})
public void add(int summand1, int summand2, int expectedSum) {
    assertThat(calculator.add(summand1, summand2)).isEqualTo(expectedSum);
}

@ParameterizedTest
@MethodSource("validTokenProvider")
fun `parse valid tokens`(data: TestData) {
    assertThat(parse(data.input)).isEqualTo(data.expected)
}

private fun validTokenProvider() = Stream.of(
    TestData(input = "1511443755_2", expected = Token(1511443755, "2")),
    TestData(input = "151175_13521", expected = Token(151175, "13521")),
    TestData(input = "151144375_id", expected = Token(151144375, "id")),
    TestData(input = "15114437599_1", expected = Token(15114437599, "1")),
    TestData(input = null, expected = null)
)
```
- You are not testing all classes in integration and refactorings of the internals will break all tests, because there is a test for each internal class. Instead, I suggest focussing on integration tests. By “integration tests” (or “component test”) I mean putting all classes together (just like in production) and test a complete vertical slide going though all technical layers (HTTP, business logic, database). This way, you are testing behavior instead of an implementation. 
- Group the Tests - JUnit5’s @Nested is useful to group tests methods
- Readable Test Names with @DisplayName
```java
@Test
    @DisplayName("Design is removed from database")
    void designIsRemoved() {}
``` 

- Use Awaitility for Asserting Asynchronous Code
```java
private static final ConditionFactory WAIT = await()
        .atMost(Duration.ofSeconds(6))
        .pollInterval(Duration.ofSeconds(1))
        .pollDelay(Duration.ofSeconds(1));

@Test
public void waitAndPoll(){
    triggerAsyncEvent();
    WAIT.untilAsserted(() -> {
        assertThat(findInDatabase(1).getState()).isEqualTo(State.SUCCESS);
    });
}
```
- Don’t Use Static Access. Never. Ever
- Use Constructor Injection 
- Don’t Use Instant.now() or new Date(), Instead, use Java’s Clock class
- Separate Asynchronous Execution and Actual Logic
## 11. How to use CI/CD in projects
Using CI can help to increase speed of delivering products by automating routine steps such as running tests in different environments, building artifacts, storing artifacts in repos and deployment. It also may force to use better code standards.

## 12 Functional vs OOP
OOP Pros: It’s easy to understand the basic concept of objects and easy to interpret the meaning of method calls. OOP tends to use an imperative style rather than a declarative style, which reads like a straight-forward set of instructions for the computer to follow.

OOP Cons: OOP Typically depends on shared state. Objects and behaviors are typically tacked together on the same entity, which may be accessed at random by any number of functions with non-deterministic order, which may lead to undesirable behavior such as race conditions.

FP Pros: Using the functional paradigm, programmers avoid any shared state or side-effects, which eliminates bugs caused by multiple functions competing for the same resources. With features such as the availability of point-free style (aka tacit programming), functions tend to be radically simplified and easily recomposed for more generally reusable code compared to OOP.

FP also tends to favor declarative and denotational styles, which do not spell out step-by-step instructions for operations, but instead concentrate on what to do, letting the underlying functions take care of the how. This leaves tremendous latitude for refactoring and performance optimization, even allowing you to replace entire algorithms with more efficient ones with very little code change. (e.g., memoize, or use lazy evaluation in place of eager evaluation.)

Computation that makes use of pure functions is also easy to scale across multiple processors, or across distributed computing clusters without fear of threading resource conflicts, race conditions, etc…

FP Cons: Over exploitation of FP features such as point-free style and large compositions can potentially reduce readability because the resulting code is often more abstractly specified, more terse, and less concrete.

More people are familiar with OO and imperative programming than functional programming, so even common idioms in functional programming can be confusing to new team members.

FP has a much steeper learning curve than OOP because the broad popularity of OOP has allowed the language and learning materials of OOP to become more conversational, whereas the language of FP tends to be much more academic and formal. FP concepts are frequently written about using idioms and notations from lambda calculus, algebras, and category theory, all of which requires a prior knowledge foundation in those domains to be understood.

## 13 JS diff between class inheritence and prototype inheritence
Class Inheritance: instances inherit from classes (like a blueprint — a description of the class), and create sub-class relationships: hierarchical class taxonomies. Instances are typically instantiated via constructor functions with the new keyword. Class inheritance may or may not use the class keyword from ES6.

Prototypal Inheritance: instances inherit directly from other objects. Instances are typically instantiated via factory functions or Object.create(). Instances may be composed from many different objects, allowing for easy selective inheritance.

## 14 Monolith vs microserveses

A monolithic architecture means that your app is written as one cohesive unit of code whose components are designed to work together, sharing the same memory space and resources.

A microservice architecture means that your app is made up of lots of smaller, independent applications capable of running in their own memory space and scaling independently from each other across potentially many separate machines.

Monolithic pros: The major advantage of the monolithic architecture is that most apps typically have a large number of cross-cutting concerns, such as logging, rate limiting, and security features such audit trails and DOS protection.

When everything is running through the same app, it’s easy to hook up components to those cross-cutting concerns.

There can also be performance advantages, since shared-memory access is faster than inter-process communication (IPC).

Monolithic cons: Monolithic app services tend to get tightly coupled and entangled as the application evolves, making it difficult to isolate services for purposes such as independent scaling or code maintainability.

Monolithic architectures are also much harder to understand, because there may be dependencies, side-effects, and magic which are not obvious when you’re looking at a particular service or controller.

Microservice pros: Microservice architectures are typically better organized, since each microservice has a very specific job, and is not concerned with the jobs of other components. Decoupled services are also easier to recompose and reconfigure to serve the purposes of different apps (for example, serving both the web clients and public API).

They can also have performance advantages depending on how they’re organized because it’s possible to isolate hot services and scale them independent of the rest of the app.

Microservice cons: As you’re building a new microservice architecture, you’re likely to discover lots of cross-cutting concerns that you did not anticipate at design time. A monolithic app could establish shared magic helpers or middleware to handle such cross-cutting concerns without much effort.

In a microservice architecture, you’ll either need to incur the overhead of separate modules for each cross-cutting concern, or encapsulate cross-cutting concerns in another service layer that all traffic gets routed through.
Eventually, even monolthic architectures tend to route traffic through an outer service layer for cross-cutting concerns, but with a monolithic architecture, it’s possible to delay the cost of that work until the project is much more mature.

Microservices are frequently deployed on their own virtual machines or containers, causing a proliferation of VM wrangling work. These tasks are frequently automated with container fleet management tools.

## 15 What is Monad in FP
Let's be clear: A Monad is not a class or a trait; it is a concept.

A Monad is an object that wraps another object in Scala. In Monads, the output of a calculation at any step is the input to other calculations, which run as a parent to the current step.
Monads are containers. That means they contain some sort of elements. Instead of allowing us to operate on these elements directly, the container itself has certain properties. We can then work with the container and the container works with the element within. Composing functions is the reason why we want to use monads and should care about monads.
## 16 What is deadlock
A deadlock occurs when the waiting process is still holding on to another resource that the first needs before it can finish.
## 17 Thread pools
When you use a thread pool, you write your concurrent code in the form of parallel tasks and submit them for execution to an instance of a thread pool.
This instance controls several re-used threads for executing these tasks.
```java
ExecutorService executorService = Executors.newFixedThreadPool(10);
Future<String> future = executorService.submit(() -> "Hello World");
// some operations
String result = future.get();

ScheduledExecutorService executor = Executors.newScheduledThreadPool(5);
executor.schedule(() -> {
    System.out.println("Hello World");
}, 500, TimeUnit.MILLISECONDS);

ExecutorService executorService = Executors.newCachedThreadPool();
ListeningExecutorService listeningExecutorService = 
  MoreExecutors.listeningDecorator(executorService);

ListenableFuture<String> future1 = 
  listeningExecutorService.submit(() -> "Hello");
ListenableFuture<String> future2 = 
  listeningExecutorService.submit(() -> "World");

String greeting = Futures.allAsList(future1, future2).get()
  .stream()
  .collect(Collectors.joining(" "));
assertEquals("Hello World", greeting);
```


