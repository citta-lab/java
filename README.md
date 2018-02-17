java [ juggling pebbles ]
--------------------------
How we declare our classes, methods and variables affects the code behavior.  

### Tips
* A `public` method can be accessed from anywhere in our application where as `private` method can only be accessed within the class.
* Since java 7, we can write larger number ( example: 5765355) by using `_` seperation. i.e 57_65_355
* Data types range: byte < short < int < long i.e 127 < 32767 < 2billion plus < 2^63
* Ternary Operator example `boolean test = checkValue ? true : false;`
* Statements are everything from data types and expression including end of line. Example: `int checkValue = 20;` is a statement and `checkValue = 20` is a expression.
* Switch is useful over if and else when the same variable needs to be checked for different value.
* isPrimeNumber check using `Math.Sqrt(n)` has better performance than doing `n/2`. i.e the number of times the program takes to complete the loop is shorter when square root is used.
* Every `object` has `state` and `behavior`. variable, type defines the state of the object whereas method defines the behavior of the object. Example: If an ant is an object, then the color, size, number of legs can be state and the functionality it is performing ( eating, scrapping, carrying ) can be behavior.
* Access modifiers: `private`- (when defined declaring class or variable ) No other classes can access this class. `protected`- classes inside the same package can access the class. `public` - all classes have access and we can also define classes without access modifiers.
* Whenever we instantiate the class by using `new` it uses the class constructor to create the object. Example: `Book textBook = new Book();`
* Every declared class by default extends Object class.
* Retrieving the class name `getClass().getSimpleName()`.
* String is not a primitive type, but a class. Primitive types can't be used directly in ArrayList and hence all primitive types needs to be converted to class before saving.
* Type conversion during invocation based on the type declaration. Example: `MyGenericArrayList<E>` declare a generics class with a formal type parameter <E>. During an actual invocation, e.g., `MyGenericArrayList<String>`, a specific type <String>, or actual type parameter, replaced the formal type parameter <E>.
* Erasure, is the process in which compiler translates generic type class `public class MyGenericArrayList<E>` and/or generic type methods `public static <E> void ArrayToArrayList(E[] a, ArrayList<E> lst) { ... }` during compilation. All the generic types are replaced with type Object by default (or the upper bound of type). The translated version of method is as follows: `public static void ArrayToArrayList(Object[] a, ArrayList lst) { ... }`.
* `ListIterator<E>` is more efficient and useful than `Iterator<E>` in case of sorting LinkedList. The reason is using ListIterator we can go back in the list where as in Iterator we cannot. Example: ` LinkedList<String> names = new LinkedList<Strings>();` we can do `names.next()` to go to next element and `names.previous()` to go back to previous one.
* `static` method are accessed without initiating the class object. Example: `value = ClassName.method();`
* Java automatically upcast child class variable (subclass) to super class type (parent), Example: `Parent parent = new Child();`. Here parent ( child class variable ) to Parent class. But we can downcast by doing so, Example: `Parent p = new Parent(); (Child(p)).parentMethod();`
* Inner class can access all of it's parent class variable and methods but inner class cannot be accessed outside of the parents class.
* `Thread.sleep()` throws an InterruptedException, so be sure to surround it with a try/catch block.
* Check if object `maybeList` is ArrayList ? `maybeList instanceof List<?>` will return boolean value.


### IntelliJ Shortcut:
* `sout` : System.out.println();
* `Command + Option + L` => reformat the code.
* `Command + /` => comment selected line.
* `psvm` : public static void main.
* `Fn + Alt + F1` : Option to show file in project.
* `fn + option + Enter `: Auto import.

### IntelliJ Build:
* If we need to build a standalone java project then `Build > Build Project`. This will create `out/production/project/..` directory with compiled files and manifest.
* If we need to build a jar file from the standalone java project then we need to perform two steps
  1.`File > ( Project Setting ) Artifacts > + > JAR > From modules with dependencies... > OK ` will set the details for creating artifacts.
  2.`Build > Build Artifacts > project name > (Action) Build ` this will create jar file inside `out/artifacts/project_name_jar/`
  3.Converting java project to java maven project [Converting a Java project to use Maven in IntelliJ IDEA](https://www.youtube.com/watch?v=s-nXWFQMXY0)

### Console Tricks:
* view jar files : `jar tf target/project_name-1.0-SNAPSHOT.jar`
* read file inside jar : `unzip -p target/project_name-1.0-SNAPSHOT.jar META-INF/MANIFEST.MF` here MANIFEST.MF is the file we are interested.
* setting mac environment variable `~/.bash_profile`



### 1. Method Overloading:

Have the same method name with in the class but have different parameters as arguments. We cannot do method overloading by changing the data type or return type ( declaring as void ) but can only be achieved by having different arguments in the method declaration. Below is the example of method overloading ( in ruby we cannot do method overloading as we are doing here ).
```java
# first method with 2 arguments
public static int checkScore (int value, String name){
  return value * 12;
}
# second method with 1 argument
public static int checkScore (int value){
  return value * 12;
}
```

### 2. Explicit Validation:

In many cases plain old java objects are basic setters and getters, however it is advisable to make the best use of setters by adding validation something like below,

2.1 Before:   
```java
public void setColor(String color) {
       this.color = color;
}
```
2.2 After:  
```java
public void setColor(String color) {
        String colorCheck = color.toLowerCase();
        if ( colorCheck.equals("red") || colorCheck.equals("blue")){
            this.color = color;
        }else{
            this.color = "black";
        }
}
```
The reason why getters and setters are defined by `public` access modifiers and their corresponding variables are in `private` because, not to let any classes access or set the variable directly instead go through the getters/setters. If we have validation check then we can prevent any harm from wrong data. There is also great article discussion on getters and setters ( data encapsulation ) in [StackExchange](https://softwareengineering.stackexchange.com/questions/21802/when-are-getters-and-setters-justified)

> Remember `private` access modifiers do not let any class to access those defined variables and methods.

### 3. Constructor:

In java every time we instantiate the class using `new` keyword we are calling a constructor of the class. We can override the default Constructor by defining the same classname method with just the access modifier. Constructor methods can be overloaded ( can created multiple method of same name by passing different parameters ). Example:

3.1 Checking default Constructor:   
```java
pubic class Person {
  /* no body*/
}
// public Person (){}; will be called by default.
Person bob = new Person();
/*instantiate the class, which calls default constructor Person */
```

3.2 Behind the Scene
```java
pubic class Person {
  public Person (){ /*same name as class and no data types */
    }
  /*explicitly defining the default Constructor*/
}
Person bob = new Person();
```
Default values can be set in default constructor and the default Constructor can call other Constructor ( method overloaded constructor). Typically its a good practice to set the variable values in the Constructor using `this` keyword instead of using setters and getters.

3.3 Method overloading in Constructor:
```java
pubic class Person {
  private String name;
  private int age;

  public Person (){
    this("Bob", 24);
  }
  /* this has to be first line in the default Constructor and calls the below method overloaded Constructor */

  // called by above Constructor
  public Person (String name, int age){
    this.name = name; // set to class name variable
    this.age = age; // set to class age variable
  }
}

Person bob = new Person();
// calling class without passing default values for Constructor and hence the Bob and 24 will be picked.
Person tom = new Person("Tom", 45);
//calls the second overloaded Constructor method in the class.
```
Here is the example of using [multiple constructors](https://github.com/citta-lab/java/blob/master/constructors/constructor.md). So Constructor are called whenever the class object has been instantiated, however Constructor methods are not considered member of the class and will always have same name as the class without any return type. When we explicitly define a constructor with parameters then we have overwritten the default constructor ( the one with no parameters ). If we want to make use of the default constructor then we need to define that explicitly by defining the empty constructor. [Constructor Overloading in Java with examples](https://beginnersbook.com/2013/05/constructor-overloading/).

### 4. Inheritance:

Inheritance can be implemented by extending the child class with another base class ( acts as parent ). Example: `public class Dog extends Animal`. In child class ( Dog ) we need to use `super` to call the default constructor from the parent class (Animal). We can access all public variables, methods of parent (Animal) via child class instance variable ( which looks child class first, and if it doesn't find then it will call parent class method etc). However we can by pass this by using `super`. Example: `super.parentClassMethod()`.

"Inheritance is the process that enables one class to acquire the properties (methods and variables) of another. With inheritance, the information is placed in a more manageable, hierarchical order."

4.1: Parent class:             
Lets assume the Animal can be called by instantiating the class with no parameters i.e `new Animal();` or passing two required parameters i.e `new Animal("jack",3);`. Also animal class has its unique method checkHealth.
```java
public class Animal {
  private String name;
  private int age;

  //constructor with 2 param
  public Animal(String name, int age){
    this.name = name;
    this.age = age;
  }

  //overwritten default constructor
  public Animal(){
    this("Default", 1);
  }

  //method in parent class
  public void checkHealth(){
    /*....some logic...*/
  }
}
```
4.2: Child class:     
Now we create child class inheriting the parent class, so by default all public property of the parent class will be available to the child.
```java
public class Dog extends Animal {
  private int color;

  // constructor with 3 parameter, passing two to parent class Animal by using super.
  public Dog(int color, String name, int age){
    super(name,age);
    this.color = color;
  }

  // calling parent method, we can also overwrite parent method.
  super.checkHealth();
}
```
>if we have need to overwrite parent class method in child class ( inheritance ) then its ideal to use parent class method directly in child class instead of using super. i.e super.parentMethod().

### 5. Composition & Encapsulation:

In java we cannot do multiple inheritance and can only extend one super class, to avoid this people often use interface to overcome this problem. Also subclasses must declare super class constructors and has to pass all the required parameters. So developers often use class composition to have more flexibility in the code layer, Composition is nothing but calling different class object with in another class to process the required functionality. Encapsulation is hiding variable and/or method access to other class by declaring them `private`. The best example for encapsulation is `setter and getter class`. The below example covers both class composition and encapsulation,

>To achieve encapsulation in Java, declare the class' variables as private and provide public setter and getter methods to modify and view the variables' values.

5.1: What ?  
Lets create person class which should get instantiated if the `sex` is passed as an argument and if the `sex` is valid then we can let other classes to access the `processPerson` method to pass the age of the person and respond with the message. `Sex` class we should only take gender string as `male` or `female` and people shouldn't be allowed to set gender directly or age directly in `Person` class ( encapsulation in both case ).

5.2: Sex Class  
```java
public class Sex {
    private String gender;

    // must have and hence in constructor
    public Sex(String gender) {
        if (gender.equals("male") || gender.equals("female")){
            this.gender = gender;
        }
    }

    //can read the gender
    public String getGender() {
        return gender;
    }
}
//note we don't have setter here and gender will be set at class instantiation
```
Now lets look at the Person class,
5.3: Person class   
```java
public class Person {
    private Sex sex;
    private int age = 0;

    public Person(Sex sex) {
        this.sex = sex;
    }

    public int getAge() {
        return age;
    }

    private void setAge(int age) {
        this.age = age;
    }

    public void processPerson(int age){
        if (sex.getGender() != null && age > 0 ){
            this.setAge(age);
        }
        if (sex.getGender().equals("male")){
            System.out.println(" Congrats you are great dude of "+getAge()+ " old !!");
        }
        if ( sex.getGender().equals("female")){
            System.out.println(" Congrats you are pretty princess of "+getAge()+ " old !!");
        }
    }
}
// Person object instantiated with Sex object ( composition )
//setAge is private and can't be accessed outside of this class ( encapsulation )
```
5.4: Main class   
```java
public class Main {
    public static void main(String[] args) {
        Sex sex = new Sex("female");
        Person person = new Person(sex);
        person.processPerson(5);
    }
}
```

> In java community programers debate on using between inheritance vs class composition. It's always a good practice to use class composition for the flexibility over inheritance.

### 6. Polymorphism :

When we inherit a subclass (example: BMW ) from parent class  (example: Car) we get an ability to retrieve all public methods and variables of the super class ( parent class ). Because of inheritance we get an option to override the super class methods or use the super class method without overriding in the subclass. If the child object calls the method not defined in child class but in super class then it calls the super class method, if the child and super class has same methods due to method overriding then object of child class will refer child class method.

>> Implementing parent class method in subclass based on it's requirement is called method overriding and having same method name with different parameters are called method overloading. Method overriding is also known as runtime polymorphism and method overloading is compile-time polymorphism.

### 7. Array and ArrayList :

We need to initialize an array with a size before using it, let's say we created an array of size 5 and assign 5 values to it ( you can't assign more than 5, as the maximum size is 5 ). But later we have a situation where we need to store two more elements in the same array and retain the previous value then there is no easy way but copy the original array to another temp array, change the size of original array from 5 to 7 and loop though the temp array to copy the elements back to original array. This will be tedious when we have to alter the size often, this can be solved by `List` and more specifically `ArrayList`. So ArrayList is an resizable array where it manages the resizing and it holds the object.

7.1 Array Resize

```java
int[] temp = originalArray;
originalArray = new int[7];
for ( int i=0; i< temp.length; i++ ){
  originalArray[i] = temp[i];
}
```
> ArrayList interface extends List interface, List interface extends collection interface, and extends Iterable. In array list we need to define type inside the < > by replacing Elements `ArrayList<E>`. So this would become `ArrayList<Integer>` instead of in array `int[] array`.

7.2 Creating new ArrayList vs Array  

```java
int[] newArray = new int[7];
ArrayList<Integer> newListArray = new ArrayList<Integer>();
//ArrayList<E> newListArray = new ArrayList<E>();
```
`int` is a primitive type and ArrayList only takes classes. for example we can use String (String is a class) inside ArrayList but not `int`.

7.3 Why `new` in Array vs ArrayList ?  

Arrays in java are objects, name of an array is an reference and `new` keyword creates an array in the heap and returns the reference to newly created array. In ArrayList, it is a class and we are instantiating new object by using keyword `key`.

7.4 Converting ArrayList to Array.   

It consist of two steps, creating an array of size equals to ArrayList and then converting the ArrayList to array by passing the variable it suppose to save.
```java
String[] newArray = new String[newListArray.getNewListArray().size()]; /* initialize to correct size */
newArray = newListArray.getNewListArray().toArray(newArray);
/* getNewListArray returns the ArrayList, toArray convert the arrayList to array and toArray(newArray) we are telling toArray method to update the variable newArray */
```

7.5 ArrayList size.

ArrayList size auto-increment in sequence of 10, the initial size has been set in default constructor upon creating ArrayList, when we try to assign more elements than 10 then it increases in increment of 10.
```java
public ArrayList(){
  this(10);
}
```

7.6 Create object (of type some class) from String.

More often we accept/receive string or int or other data type values and we need to pass it to method which only receives object of particular type class. So the way we would need to do is converting the string (in this example) to object of that class. Example: Let say `String name = "Jack"; String race = "White";` name & race needs to be saved in `ArrayList<People> newPeople = new ArrayList<People>();` ( array list of type People ) using method `addPeople` which takes People object.
```java
public void addPeople(People people){
  newPeople.add(people);
}
```
So we need to add `Jack` and `White` values via addPeople. to handle that we need to add `Factory Method` (i.e static method in pojo class to call default constructor). i.e
```java
public class People {
    private String name;
    private String race;

    public People(String name, String race){
       this.setRace(race);
       this.setName(name);
   }
   /* .... setter and getter methods here ... */
   public static People createPerson(String name, String race){
        return new People(name, race);
    }
  }
```
Now we need to pass these string values to createPerson,
```java
String name = 'Jack';
String race = 'White';
People person = People.createPerson(name, race);
addPeople(person);
```

### 8. LinkedList :

In ArrayList if we need to add an item to an existing list then there will be a shift of existing items to the right or down ( whatever direction preferred while displaying ). Similarly, we are removing an item from the list then existing items need to be shifted left or up to form newly formed list. This process is time consuming and takes lot of computer processing in large ArrayList, to rescue this we leverage LinkedList.

8.1 Why LinkedList ?  
Lets assume original players list was added with few names
```java
ArrayList<String> players = new ArrayList<String>();
players.add("Dan"); //index 0
players.add("Steve"); //index 1
players.add("Jack"); //index 2
```
If we are trying to add new player `Adam` in position 1 after `Dan` and before `Steve` then java would be rearranging the list.
```java
players.add(1, "Adam"); //adding with index position
```
So the new list would look like `Dan, Adam, Steve, Jack` in index position 0,1,2,3 respectively. In LinkedList each element is connected to it's next element, so in case of insertion the reference from `Dan` will change to `Adam` instead of `Steve` and forms the new list. So insertion and/or deletion will be much faster.

>>The ArrayList is better for storing and accessing data, as it is very similar to a normal array.
The LinkedList is better for manipulating data, such as making numerous inserts and deletes.

### 9. Abstract and Interface.

Abstract class is boiler plate class which defines the idea and let the subclasses ( child ) classes implement the idea according to their requirement. So we cannot instantiate Abstract class but we certainly can instantiate child/subclass which extends the Abstract class. Abstract class can have any number of constructors just like normal class and default constructor is used if not defined explicitly.

Example:
```java
abstract class Household{
  abstract void getCash();
}

class Parent extends Household{
  int cashLimit;
  Parent(){
    cashLimit = 10000;
  }
  void getCash(){
    System.out.println(" can get all money :"+cashLimit);
  }
}
```

An interface is a completely abstract class without explicit use of keyword `abstract`. In case of Abstract class the class needs to be defined as `Abstract class Book { }` and abstract method needs to be defined with abstract keyword as well `abstract bookColor( ){ }`. So each method in interface class is also abstract methods and we don't have to explicitly define using keyword abstract. The main advantage of interface is "A class can inherit from just one superclass, but can implement multiple interfaces!".

Example:
```java
interface Animal {
  public void eat();
  public void makeSound();
}

class Cat implements Animal {
  //Use 'implements' while implementing Interfaces
  //Don't use 'extends'
  public void makeSound() {
    System.out.println("Meow!!!!");
  }
  public void eat() {
    System.out.println("Ahouch!!!");
  }
}
```
When we implement interface we need to override all of it's methods. Every field of an interface is public, static and final (By default, every member of an interface is public and while implementing you should not reduce this visibility).

>> Interface will have only abstraction methods unlike Abstract class ( which can also have concrete methods ) and hence Interfaces show 100% abstractness.

### 10. Anonymous Class

Anonymous class is a way to extend the class on fly, typically we use @Override annotation to define the method has been overridden. Anonymous class examples in different scenarios are mentioned below,

#10.1 Overriding
```java
class A{
   public void methodA() {
      System.out.println("methodA");
    }
}
class B{
    A a = new A() {
     @Override public void methodA() {
        System.out.println("anonymous methodA");
     }
   };
}
```
the modification of methodA is only applied to class B and if we create new instance of class A then we would be refer back to the original methodA declaration from class A.

#10.2 Implementing Interface
```java
interface interfaceA{
   public void methodA();
}
class B{
   interfaceA a = new interfaceA() {
     @Override public void methodA() {
        System.out.println("anonymous methodA implementer");
     }
   };
}
```
#10.3 In Map
```java
Map map = new HashMap() {{
   put("key", "value");
}};
```
### 11. Threads

Life Cycle of threads ` New > Runnable > Running > Waiting > Dead `. Example of using thread in a class by extending the Thread class looks like below, when we create an object of class Loader it runs on new thread. Priority of thread ranges from 1 to 10 and by default all threads have set priority as 5. We can change the priority by calling setPriority(5).
      Override the run method in extending class and start the thread by calling Thread implemented class object with start method. Example: `obj.start();`
```java
class Loader extends Thread {
  public void run() {
    System.out.println("Hello");
  }
}

class MyClass {
  public static void main(String[ ] args) {
    Loader obj = new Loader();
    obj.start();
  }
}
```
Another way of implementing Thread is by implementing Runnable interface. As we know interface is implicit abstract class and with abstract methods, so we would need to override the Runnable class methods. i.e `run` method needs to be overwritten in implementing class and pass the instance of implemented class to thread object. Example: `Thread thread = new Thread(new Child());` where `class Child implements Runnable`.Please refer below example,

```java
class Loader implements Runnable {
  public void run() {
    System.out.println("Hello");
  }
}

class MyClass {
  public static void main(String[ ] args) {
    Thread t = new Thread(new Loader());
    t.start();
  }
}
```
 Implementing the Runnable interface is the preferred way to start a Thread, because it enables us to extend from another class, as well.

### 12. HashMap

ArrayList holds the values by adding index internally, whereas HashMap stores it by key value pair and hence the memory consumption is higher than List. But HashMap also stores values without any order unlike ArrayList ( order is maintained in ArrayList ) and the important benefit is no duplicates allowed in HashMap.

#### 12.1 Simple HashMap
```java
HashMap<String, Integer> hashMap = new HashMap< >();
hashMap.put("Bob", 22);
hashMap.put("Rob", 28);
```

#### 12.2 ArrayList in HashMap
```java
HashMap<String, ArrayList<Integer>> hashMap = new HashMap<>();
hashMap.put("Bob", new ArrayList<Integer>(Arrays.asList(0, 4, 8, 9, 12)));
//or
Integer[] intArray = {20, 11, 13};
hashMap.put("Rob", new ArrayList<Integer>(Arrays.asList(intArray)));
```

#### 12.3 Retrieve HashMap data
```java
HashMap<String, ArrayList<Integer>> arrayListHashMap = new HashMap<>();
Iterator it = arrayListHashMap.entrySet().iterator(); //entrySet is used to iterate keys
while(it.hasNext()){
  Map.Entry mapPair = (Map.Entry)it.next(); //will fetch key and value
  System.out.print(mapPair.getKey()); //will have key
  System.out.print(":"+mapPair.getValue()); //will have value
}
```
#### 12.4 HashMap with Object
Often we will have value as an object instead of simple Integer or String, In below scenario we will first create `HashMap<String, ClassName>` and later we will retrieve the value of an object using HashMap key. So we will be creating class Worker to hold worker name, age and skills. To minimize the code and for ease we will have `Worker` class constructer to initialize these values. Worker class will look like below,
```java
package com.company;

public class Worker {

    private String name;
    private String skill;
    private Integer age;

    public Worker(String name, String skill, int age) {
        this.name = name;
        this.skill = skill;
        this.age = (Integer) age;
    }

    public String getName() {
        return name;
    }

    public String getSkill() {
        return skill;
    }

    public Integer getAge() {
        return age;
    }
}
```
The main class will have two methods one for preparing the data by adding Worker object to the key and in second method processData we will retrieve the values ( here we have two different approach ).
```java
public static void prepData(HashMap<String, Worker> hashMap)
{
       //One way to add
        Worker itWorker = new Worker("Jack", "JavaDeveloper", 22);
        hashMap.put("Google", itWorker);
        //Another way
        hashMap.put("Yelp", new Worker("Emily", "SolutionDesigner", 27));
        hashMap.put("Google", new Worker("Mahesh", "Developer", 30));
        hashMap.put("Apple", new Worker("Jen", "Manager", 37));
    }
```
Here we will process the data / retrieve the values.
```java
public static void processData(HashMap<String, Worker> hashMap) {

        //One way to extract
        for (String key : hashMap.keySet()) {
            Worker worker = hashMap.get(key);
            System.out.print("");
            System.out.println(key + " : employee " + worker.getName() + " (" + worker.getAge() + ") " + "is a " + worker.getSkill());
        }

        //Another way to extract
        Iterator it = hashMap.entrySet().iterator();
        System.out.println(" ");
        while (it.hasNext()) {
            Map.Entry hashObject = (Map.Entry) it.next();
            String key = hashObject.getKey().toString();
            Worker worker = (Worker) hashObject.getValue();
            System.out.println(key + " > employee " + worker.getName() + " (" + worker.getAge() + ") " + "is a " + worker.getSkill());
        }
    }
```

### Code Review Checklist
Separate checklist to follow some of the best practices in programming [Code Review](https://github.com/citta-lab/java/blob/master/codereview.md)

### Error:

| # | Error Description | Root cause |
| --- | --- | --- |
| 1 | Theres is no default constructor available in 'inheritance.PARENTCLASS' | Add default constructor in child class or parent class |
| 2 | Show file differences that haven't been staged |  |
| 3 | Show file differences that haven't been staged |  |
| 4 | Show file differences that haven't been staged |  |
| 5 | Show file differences that haven't been staged |  |
