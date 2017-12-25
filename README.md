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
* Retrieving the class name `getClass().getSimpleName()`

### IntelliJ Shortcut:
* `sout` : System.out.println();
* `Command + Option + L` => reformat the code.
* `Command + /` => comment selected line.

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
