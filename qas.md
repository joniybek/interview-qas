

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
- Regression Testing

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

## 18. RDBMS: how to prepare copy of table which is under high update/insert load?
When working with batch data we need to load/export data into CSV files in tmp dir and then dowload

## 19. RDBMS: Normalization forms
- A primary is a single column value used to identify a database record uniquely. A primary key cannot be NULL and  must be unique.
- A composite key is a primary key composed of multiple columns used to identify a record uniquely.
-  Foreign Key references the primary key of another Table! It helps connect your Tables. It can be null and non unique. It ensures rows in one table have corresponding rows in another.
-  A transitive functional dependency is when changing a non-key column, might cause any of the other non-key columns to change
-  Indexing is a way to optimize the performance of a database by minimizing the number of disk accesses required when a query is processed. It is a data structure technique which is used to quickly locate and access the data in a database. In general, there are two types of file organization mechanism which are followed by the indexing methods to store the data:

Database Normal Forms
- 1NF (First Normal Form) Rules
    Each table cell should contain a single value.
    Each record needs to be unique.
- 2NF (Second Normal Form) Rules
    Rule 1- Be in 1NF
    Rule 2- Single Column Primary Key
- 3NF (Third Normal Form) Rules
    Rule 1- Be in 2NF
    Rule 2- Has no transitive functional dependencies
-4NF (Fourth Normal Form) Rules
If no database table instance contains two or more, independent and multivalued data describing the relevant entity, then it is in 4th Normal Form.
- 5NF (Fifth Normal Form) Rules
A table is in 5th Normal Form only if it is in 4NF and it cannot be decomposed into any number of smaller tables without loss of data.
- 6NF (Sixth Normal Form)
## 20. Transaction isolation levels
- READ UNCOMMITTED - no locks, read everything
    READ COMMITTED - with lock, default to many RDBMS, Suppose T2 reads some of the rows and T1 then change a row and commit the change, now T2 reads the same row set and gets a different result
    REPEATABLE READ - his isolation level returns the same result set throughout the transaction execution for the same SELECT run any number of times during the progression of a transaction. REPEATABLE READ is MySQL’s default transaction isolation level.
    SERIALIZABLE - completely isolates the effect of one transaction from others. It is similar to REPEATABLE READ with the additional restriction that row selected by one transaction cannot be changed by another until the first transaction finishes.
## 21. Compare SQL and NoSQL 
SQL databases are table based databases whereas NoSQL databases can be document based, key-value pairs, graph databases.
SQL databases are vertically scalable while NoSQL databases are horizontally scalable.
SQL databases have a predefined schema whereas NoSQL databases use dynamic schema for unstructured data.
SQL requires specialized DB hardware for better performance while NoSQL uses commodity hardware.


|         Parameter         |                                                                 SQL                                                                 |                                                                                               NOSQL                                                                                               |   |   |
|:-------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|---|---|
| Definition                | SQL   databases are primarily called RDBMS or Relational Databases                                                                  | NoSQL   databases are primarily called as Non-relational or distributed database                                                                                                                  |   |   |
| Design for                | Traditional   RDBMS uses SQL syntax and queries to analyze and get the data for further   insights. They are used for OLAP systems. | NoSQL   database system consists of various kind of database technologies. These   databases were developed in response to the demands presented for the   development of the modern application. |   |   |
| Query Language            | Structured   query language (SQL)                                                                                                   | No   declarative query language                                                                                                                                                                   |   |   |
| Type                      | SQL   databases are table based databases                                                                                           | NoSQL   databases can be document based, key-value pairs, graph databases                                                                                                                         |   |   |
| Schema                    | SQL   databases have a predefined schema                                                                                            | NoSQL   databases use dynamic schema for unstructured data.                                                                                                                                       |   |   |
| Ability to scale          | SQL   databases are vertically scalable                                                                                             | NoSQL   databases are horizontally scalable                                                                                                                                                       |   |   |
| Examples                  | Oracle,   Postgres, and MS-SQL.                                                                                                     | MongoDB,   Redis, , Neo4j, Cassandra, Hbase.                                                                                                                                                      |   |   |
| Best suited for           | An   ideal choice for the complex query intensive environment.                                                                      | It   is not good fit complex queries.                                                                                                                                                             |   |   |
| Hierarchical data storage | SQL   databases are not suitable for hierarchical data storage.                                                                     | More   suitable for the hierarchical data store as it supports key-value pair   method.                                                                                                           |   |   |
| Variations                | One   type with minor variations.                                                                                                   | Many   different types which include key-value stores, document databases, and graph   databases.                                                                                                 |   |   |
| Development Year          | It   was developed in the 1970s to deal with issues with flat file storage                                                          | Developed   in the late 2000s to overcome issues and limitations of SQL databases.                                                                                                                |   |   |
| Open-source               | A   mix of open-source like Postgres & MySQL, and commercial like Oracle   Database.                                                | Open-source                                                                                                                                                                                       |   |   |
| Consistency               | It   should be configured for strong consistency.                                                                                   | It   depends on DBMS as some offers strong consistency like MongoDB, whereas   others offer only offers eventual consistency, like Cassandra.                                                     |   |   |
| Best Used for             | RDBMS   database is the right option for solving ACID problems.                                                                     | NoSQL   is a best used for solving data availability problems                                                                                                                                     |   |   |
| Importance                | It   should be used when data validity is super important                                                                           | Use   when it's more important to have fast data than correct data                                                                                                                                |   |   |
| Best option               | When   you need to support dynamic queries                                                                                          | Use   when you need to scale based on changing requirements                                                                                                                                       |   |   |
| Hardware                  | Specialized   DB hardware (Oracle Exadata, etc.)                                                                                    | Commodity   hardware                                                                                                                                                                              |   |   |
| Network                   | Highly   available network (Infiniband, Fabric Path, etc.)                                                                          | Commodity   network (Ethernet, etc.)                                                                                                                                                              |   |   |
| Storage Type              | Highly   Available Storage (SAN, RAID, etc.)                                                                                        | Commodity   drives storage (standard HDDs, JBOD)                                                                                                                                                  |   |   |
| Best features             | Cross-platform   support, Secure and free                                                                                           | Easy   to use, High performance, and Flexible tool.                                                                                                                                               |   |   |
| Top Companies Using       | Hootsuite,   CircleCI, Gauges                                                                                                       | Airbnb,   Uber, Kickstarter                                                                                                                                                                       |   |   |
| Average salary            | The   average salary for any professional SQL Developer is $84,328 per year in the   U.S.A.                                         | The   average salary for "NoSQL developer" ranges from approximately   $72,174 per year                                                                                                           |   |   |
| ACID vs. BASE Model       | ACID(   Atomicity, Consistency, Isolation, and Durability) is a standard for RDBMS                                                  | Base   ( Basically Available, Soft state, Eventually Consistent) is a model of many   NoSQL systems                                                                                               |   |   |


## 22. Stored procedure, pros vs cons
##### Advantages :
- Better Performance – The procedure calls are quick and efficient as stored procedures are compiled once and stored in executable form.Hence the response is quick. The executable code is automatically cached, hence lowers the memory requirements.
- Higher Productivity – Since the same piece of code is used again and again so, it results in higher productivity.
- Ease of Use – To create a stored procedure, one can use any Java Integrated Development Environment (IDE). Then, they can be deployed on any tier of network architecture.
- Scalability – Stored procedures increase scalability by isolating application processing on the server.
- Maintainability – Maintaining a procedure on a server is much easier then maintaining copies on various client machines, this is because scripts are in one location.
- Security – Access to the Oracle data can be restricted by allowing users to manipulate the data only through stored procedures that execute with their definer’s privileges.

##### Disadvantages :
- Testing – Testing of a logic which is encapsulated inside a stored procedure is very difficult. Any data errors in handling stored procedures are not generated until runtime.
- Debugging – Depending on the database technology, debugging stored procedures will either be very difficult or not possible at all. Some relational databases such as SQL Server have some debugging capabilities.
- Versioning – Version control is not supported by the stored procedure.
- Cost – An extra developer in the form of DBA is required to access the SQL and write a better stored procedure. This will automatically incur added cost.
- Portability – Complex stored procedures will not always port to upgraded versions of the same database. This is specially true in case of moving from one database type(Oracle) to another database type(MS SQL Server)
    
## 23. How to profile slow queries and how to optimize
- Most SQL databases provide a Slow Query Log, a log where the database server registers all queries that exceed a given threshold of execution time. 
- Middleware and Application Logs - Write in directly code how much time took to execute query
- APM and Distributed Tracing - The way most APMs work is through a library or agent that is installed alongside your application and automatically instruments its client libraries, like http, grpc, sql, etc. to monitor and log transactions and queries.

##### To optimize
- SQL Server can efficiently filter a data set using indexes via the WHERE clause or any combination of filters that are separated by an AND operator. 
- OR is a different story. Because it is inclusive, SQL Server cannot process it in a single operation. Instead, each component of the OR must be evaluated independently. 
- Wildcard String Searches - Indexes are present on searched columns. If not, can we use full-text indexes? If not, can we use hashes, n-grams, or some other solution?
- A query hint is an explicit direction by us to the query optimizer. 

## 23. Design patterns from GoF
##### Builder
Lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.
```java
public final class Hero {
  private final Profession profession;
  private final String name;
  private final HairType hairType;
  private final HairColor hairColor;
  private final Armor armor;
  private final Weapon weapon;

  private Hero(Builder builder) {
    this.profession = builder.profession;
    this.name = builder.name;
    this.hairColor = builder.hairColor;
    this.hairType = builder.hairType;
    this.weapon = builder.weapon;
    this.armor = builder.armor;
  }
}
  public static class Builder {
    private final Profession profession;
    private final String name;
    private HairType hairType;
    private HairColor hairColor;
    private Armor armor;
    private Weapon weapon;

    public Builder(Profession profession, String name) {
      if (profession == null || name == null) {
        throw new IllegalArgumentException("profession and name can not be null");
      }
      this.profession = profession;
      this.name = name;
    }

    public Builder withHairType(HairType hairType) {
      this.hairType = hairType;
      return this;
    }

    public Builder withHairColor(HairColor hairColor) {
      this.hairColor = hairColor;
      return this;
    }

    public Hero build() {
      return new Hero(this);
    }
  }
  ```
##### Abstract Factory
is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.
```java
public interface KingdomFactory {
  Castle createCastle();
  King createKing();
  Army createArmy();
}

public class ElfKingdomFactory implements KingdomFactory {
  public Castle createCastle() {
    return new ElfCastle();
  }
  public King createKing() {
    return new ElfKing();
  }
  public Army createArmy() {
    return new ElfArmy();
  }
}

public class OrcKingdomFactory implements KingdomFactory {
  public Castle createCastle() {
    return new OrcCastle();
  }
  public King createKing() {
    return new OrcKing();
  }
  public Army createArmy() {
    return new OrcArmy();
  }
}
```
##### Factory methods
is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.
```java
/**
 * Base factory class. Note that "factory" is merely a role for the class. It
 * should have some core business logic which needs different products to be
 * created.
 */
public abstract class Dialog {

    public void renderWindow() {
        // ... other code ...

        Button okButton = createButton();
        okButton.render();
    }

    /**
     * Subclasses will override this method in order to create specific button
     * objects.
     */
    public abstract Button createButton();
}

public class Demo {
    private static Dialog dialog;

    public static void main(String[] args) {
        configure();
        runBusinessLogic();
    }

    /**
     * The concrete factory is usually chosen depending on configuration or
     * environment options.
     */
    static void configure() {
        if (System.getProperty("os.name").equals("Windows 10")) {
            dialog = new WindowsDialog();
        } else {
            dialog = new HtmlDialog();
        }
    }
```
##### Adapter
Allows objects with incompatible interfaces to collaborate.
```java,

/**
 * RoundHoles are compatible with RoundPegs.
 */
public class RoundHole {
    private double radius;

    public RoundHole(double radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }

    public boolean fits(RoundPeg peg) {
        boolean result;
        result = (this.getRadius() >= peg.getRadius());
        return result;
    }
}

/**
 * RoundPegs are compatible with RoundHoles.
 */
public class RoundPeg {
    private double radius;

    public RoundPeg() {}

    public RoundPeg(double radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }
}

/**
 * SquarePegs are not compatible with RoundHoles (they were implemented by
 * previous development team). But we have to integrate them into our program.
 */
public class SquarePeg {
    private double width;

    public SquarePeg(double width) {
        this.width = width;
    }

    public double getWidth() {
        return width;
    }

    public double getSquare() {
        double result;
        result = Math.pow(this.width, 2);
        return result;
    }
}

/**
 * Adapter allows fitting square pegs into round holes.
 */
public class SquarePegAdapter extends RoundPeg {
    private SquarePeg peg;

    public SquarePegAdapter(SquarePeg peg) {
        this.peg = peg;
    }

    @Override
    public double getRadius() {
        double result;
        // Calculate a minimum circle radius, which can fit this peg.
        result = (Math.sqrt(Math.pow((peg.getWidth() / 2), 2) * 2));
        return result;
    }
}

/**
 * Somewhere in client code...
 */
public class Demo {
    public static void main(String[] args) {
        // Round fits round, no surprise.
        RoundHole hole = new RoundHole(5);

        SquarePeg smallSqPeg = new SquarePeg(2);
        SquarePeg largeSqPeg = new SquarePeg(20);
        // hole.fits(smallSqPeg); // Won't compile.

        // Adapter solves the problem.
        SquarePegAdapter smallSqPegAdapter = new SquarePegAdapter(smallSqPeg);
        SquarePegAdapter largeSqPegAdapter = new SquarePegAdapter(largeSqPeg);
        if (hole.fits(smallSqPegAdapter)) {
            System.out.println("Square peg w2 fits round hole r5.");
        }
        if (!hole.fits(largeSqPegAdapter)) {
            System.out.println("Square peg w20 does not fit into round hole r5.");
        }
    }
}
```
##### Decorator
Lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.
```java
public interface Troll {
  void attack();
  int getAttackPower();
  void fleeBattle();
}

public class SimpleTroll implements Troll {

  private static final Logger LOGGER = LoggerFactory.getLogger(SimpleTroll.class);

  @Override
  public void attack() {
    LOGGER.info("The troll tries to grab you!");
  }

  @Override
  public int getAttackPower() {
    return 10;
  }

  @Override
  public void fleeBattle() {
    LOGGER.info("The troll shrieks in horror and runs away!");
  }
}

public class ClubbedTroll implements Troll {

  private static final Logger LOGGER = LoggerFactory.getLogger(ClubbedTroll.class);

  private final Troll decorated;

  public ClubbedTroll(Troll decorated) {
    this.decorated = decorated;
  }

  @Override
  public void attack() {
    decorated.attack();
    LOGGER.info("The troll swings at you with a club!");
  }

  @Override
  public int getAttackPower() {
    return decorated.getAttackPower() + 10;
  }

  @Override
  public void fleeBattle() {
    decorated.fleeBattle();
  }
}

// simple troll
var troll = new SimpleTroll();
troll.attack(); // The troll tries to grab you!
troll.fleeBattle(); // The troll shrieks in horror and runs away!

// change the behavior of the simple troll by adding a decorator
var clubbedTroll = new ClubbedTroll(troll);
clubbedTroll.attack(); // The troll tries to grab you! The troll swings at you with a club!
clubbedTroll.fleeBattle(); 
```

