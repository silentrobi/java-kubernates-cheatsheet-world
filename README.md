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

**Simple generic class**

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
**Bounded generic class**

In here `Printer` generic class is bounded by Animal Class.
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
**Generic Method**

```java
public class Main {

    public static void main(String[] args) {
	// write your code here
        shout("Jhon");
        shout(56);
        shout(new Cat());

    }

    private static <T> void shout (T thingToShout){ // Here <T> is necessary because java needs to know that method T type is a generic type
        System.out.println(thingToShout + "!!!");
    }
}
```
**Generic methid with multiple generic types**

```java
public class Main {

    public static void main(String[] args) {
	// write your code here
        shout("Jhon", "Robi");
        shout(56, 78.2);
        shout(new Cat(), new Dog());

    }

    private static <K, V> void shout (K thingToShout, V otherThingToShout ){
        System.out.println(thingToShout + "!!!");
        System.out.println(otherThingToShout + "!!!");
    }
}
```
**Wildcard(?) generic type example.**

Following gives compile time error because List<Integer> is not subclass of List<Object> though Integer is subclass of Object.
```java
public class Main {

    public static void main(String[] args) {
        // write your code here
        List<Integer> integerList = new ArrayList<>();

        integerList.add(3);

        printList(integerList); // It gives compile time error because List<Integer> is not subclass of List<Object> though Integer is subclass of Object. 
    }

    private static void printList(List<Object> myList) {
        System.out.println(myList.size());
    }
}
```
To solve this we can use `?` wildcard. We should use wildcard when we don't know the what the type would be.
```java
	public class Main {

    public static void main(String[] args) {
        // write your code here
        List<Integer> integerList = new ArrayList<>();

        integerList.add(3);

        printList(integerList);
    }

    private static void printList(List<?> myList) {
        System.out.println(myList.size());
    }
}
```
**Bounded wildcard `?`**

Now we can bound our wildcard List by extend Animal class to it. This means we can pass list of subclasses that extend Animal class. E.g Dog, Cat class.

- Upper Bound example.
```java
public class Main {

    public static void main(String[] args) {
        // write your code here
        List<Cat> catList = new ArrayList<>();

        catList.add(new Cat());

        printList(catList);
    }

    private static void printList(List<? extends Animal> myList) { // Upper bound is Animal
        System.out.println(myList.size());
    }
}
```
- Lower Boundary example.
```java
    public static void main(String[] args) {
        // write your code here
        List<Number> numberList = new ArrayList<>();
        numberList.add(2);

        printList(numberList);
    }

    private static void printList(List<? super Double> myList) { // lower bound is Double
        System.out.println(myList.size());
    }
```
## Lambda Expression
Lambda is useful when you want to shorten your code. In order to use lambda we need to define **functional interface**. Functional interface should contains only one method signature. 
```java
@FunctionalInterface
public interface Episode {
    void show();
}

```
Now One way to use the interface method would be to use annonymous innerclass to implement the interface as follow:
```java
    public static void main(String[] args) {
        // write your code here
        Episode episode = new Episode() { //Annonymous inner class that implements Episode interface.
            @Override
            public void show() {
                System.out.println("Showing the episode!");
            }
        };

        episode.show();
    }
```
However using lambda function it is much simpler. 🙂

```java
    public static void main(String[] args) {
        // write your code here
        Episode episode = () -> System.out.println("Showing the episode!");

        episode.show();
    }
```
Example of passing lambda function as argument.
```java
    public static void main(String[] args) {
        // write your code here
        showEpisode(() -> System.out.println("Showing the episode!"));
    }

    private static void showEpisode(Episode episode){
        episode.show();
    }
```
Another example.
```java
    public static void main(String[] args) {
        List<Integer> integerList = Arrays.asList(1,2,3,4);

        integerList.forEach(i -> System.out.println(i)); // Here lambda function is the implementation of accept method of Consumer Interface.
    }
```

## Why do I need to override the equals and hashCode methods in Java?

https://stackoverflow.com/questions/2265503/why-do-i-need-to-override-the-equals-and-hashcode-methods-in-java#:~:text=contract%20for%20Object.-,hashCode()%2C%20which%20will%20prevent%20your%20class%20from%20functioning%20properly,HashMap%2C%20HashSet%2C%20and%20Hashtable.&text=By%20defining%20equals()%20and,keys%20in%20hash%2Dbased%20collections.
