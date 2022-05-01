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


## Java Generic

Generic `Printer` class
```java
/**
 *
 * @param <T> is generic type (Double, Integer, or any Object...)
 */
public class Printer<T> {

    private T thingToPrint;

    public Printer(T thingToPrint){
        this.thingToPrint = thingToPrint;
    }

    public void print(){
        System.out.println(thingToPrint);
    }
}
```
Usage of `Printer` class.
```java
public class Main {

    public static void main(String[] args) {
	// write your code here

        Printer<Integer> printer1 = new Printer<>(23);
        Printer<Double> printer2 = new Printer<>(23.5);

        printer1.print();
        printer2.print();
    }
}
```
Bounded generic class example. In here `Printer` generic class is bounded by Animal Class.
```java

public class Animal {
    public void print(){
        System.out.println("animal");
    }
}
public class Cat extends Animal{

    @Override
    public void print(){
        System.out.println("cat");
    }
}
public class Dog extends Animal{

    @Override
    public void print(){
        System.out.println("dog");
    }
}
/**
 *
 * @param <T> is generic type (Double, Integer, or any Object...)
 */
public class Printer<T extends Animal> {

    private T thingToPrint;

    public Printer(T thingToPrint){
        this.thingToPrint = thingToPrint;
    }

    public void print(){
        thingToPrint.print();
    }
}
```
Usage of bounded `Printer` class.
```java
public class Main {

    public static void main(String[] args) {
	// write your code here

        Printer<Dog> printer1 = new Printer<>(new Dog());
        Printer<Cat> printer2 = new Printer<>(new Cat());

        printer1.print();
        printer2.print();
    }
}
```
It is possible to extend Generic Type with Multiple Interfaces but with only one class.
```java
    	public class Printer<T extends Serializable> {}
	public class Printer<T extends Animal & Serializable> {} // Always class come first
	public class Printer<T extends Class1 & Interface1 & Interface2 > {}
	
```

