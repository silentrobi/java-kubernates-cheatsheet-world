# Java Cheatsheet
## Spring 5 tutorial
https://www.concretepage.com/spring-5/

## Perform CRUD operation from spring java
You can use `RestTemplate` (syncronous library) or `WebClient` (asyncronous library).
## Run a method asyncronously
Use **CompletableFuture** from concurrent library
```java
public void method(){
    System.out.println("Async call")
}

CompletableFuture.runAsync(() -> method());
```
## Collections

## Java basic
- primitive types pass by value, Objects are pass by reference

- method overriding, method overloading, subclass, interface, abstract class, data encapsulation, enum,  upper casting, down casting, default method in interface, 
https://www.javatpoint.com/java-collections-interview-questions#:~:text=The%20differences%20between%20the%20Collection,and%20synchronize%20the%20collection%20elements. 

https://gist.github.com/psayre23/c30a821239f4818b0709
