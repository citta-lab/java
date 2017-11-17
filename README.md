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

### IntelliJ Shortcut:
* `sout` : System.out.println();
* `Command + Option + L` => reformat the code.
* `Command + /` => comment selected line.

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
  /* no body*/  // public Person (){}; will be called by default.
}
Person bob = new Person(); /*instantiate the class, which calls default constructor Person */
```

3.2 Behind the Scene
```java
pubic class Person {
  public Person (){ /*same name as class and no data types */
    /*explicitly defining the default Constructor*/
  }
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
    this("Bob", 24); /* this has to be first line in the default Constructor and calls the below method overloaded Constructor */
  }

  // called by above Constructor
  public Person (String name, int age){
    this.name = name; // set to class name variable
    this.age = age; // set to class age variable
  }
}

Person bob = new Person(); // calling class without passing default values for Constructor and hence the Bob and 24 will be picked.
Person tom = new Person("Tom", 45); //calls the second overloaded Constructor method in the class.
```
Here is the example of using [multiple constructors](https://github.com/citta-lab/java/blob/master/constructors/constructor.md). So Constructor are called whenever the class object has been instantiated, however Constructor methods are not considered member of the class and will always have same name as the class without any return type. When we explicitly define a constructor with parameters then we have overwritten the default constructor ( the one with no parameters ). If we want to make use of the default constructor then we need to define that explicitly by defining the empty constructor. [Constructor Overloading in Java with examples](https://beginnersbook.com/2013/05/constructor-overloading/).

### 4. Inheritance:

Inheritance can be implemented by extending the child class with another base class ( acts as parent ). Example: `public class Dog extends Animal`. In child class ( Dog ) we need to use `super` to call the default constructor from the parent class (Animal). We can access all public variables, methods of parent (Animal) via child class instance variable ( which looks child class first, and if it doesn't find then it will call parent class method etc). However we can by pass this by using `super`. Example: `super.parentClassMethod()`. 




### Error:

| # | Error Description | Root cause |
| --- | --- | --- |
| 1 | Theres is no default constructor available in 'inheritance.PARENTCLASS' | Add default constructor in child class or parent class |
| 2 | Show file differences that haven't been staged |  |
| 3 | Show file differences that haven't been staged |  |
| 4 | Show file differences that haven't been staged |  |
| 5 | Show file differences that haven't been staged |  |
