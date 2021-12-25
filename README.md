# Java Cheatsheet
## Spring 5 tutorial
https://www.concretepage.com/spring-5/
## Run a method asyncronously
Use **CompletableFuture** from concurrent library
```java
public void method(){
    System.out.println("Async call")
}

CompletableFuture.runAsync(() -> method());
```
## Collections

