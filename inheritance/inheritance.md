#### Inheritance Example:

#### Step 1: Parent Class ( Animal )
```java
public class Animal {

    private String name;
    private int brain;
    private int body;
    private int size;
    private int weight;

    public String getName() {
        return name;
    }

    public int getBrain() {
        return brain;
    }

    public int getBody() {
        return body;
    }

    public int getSize() {
        return size;
    }

    public int getWeight() {
        return weight;
    }

    public void eat(){
        System.out.println(" Called inside animal class : Eat ");
    }

    public void move(){
        System.out.println(" Called inside animal class : Move ");
    }

    public Animal(String name, int brain, int body, int size, int weight) {
        this.name = name;
        this.brain = brain;
        this.body = body;
        this.size = size;
        this.weight = weight;
    }
}
```

#### Step2: Child Class ( inherited from Animal )

```java
public class Dog extends Animal  {

    private int legs;
    private int eyes;

    public int getLegs() {
        return legs;
    }

    public int getEyes() {
        return eyes;
    }

    public int getEars() {
        return ears;
    }

    public int getTail() {
        return tail;
    }

    private int ears;
    private int tail;

    public Dog(String name, int brain, int body, int size, int weight, int legs, int eyes, int ears, int tail) {

        super(name, brain, body, size, weight); /* this calls the animal constructor with 4 param */

        this.legs = legs;
        this.eyes = eyes;
        this.ears = ears;
        this.tail = tail;
    }
}
```

#### Step 3: Testing the inheritance

```java
Dog dog = new Dog("Pedey", 1, 1, 4,15, 2,2,2,1 );
System.out.println(" Called Dog class and name from Animal class : "+dog.getName()+ " brain from Animal class : "+dog.getBrain()+ "" +" body from Animal class : "+dog.getBody()+ " size and weight from Animal class : "+dog.getSize()+ " and "+dog.getWeight()+ "" +" legs from dog class : "+dog.getLegs()+ " ears from dog class : "+dog.getEars()+ " eyes from dog class : "+dog.getEyes()+ " tail from " +
"dog class : "+dog.getTail());

dog.eat();
dog.move();
```
and the output

```java
Called Dog class and name from Animal class : Pedey brain from Animal class : 1 body from Animal class : 1 size and weight from Animal class : 4 and 15 legs from dog class : 2 ears from dog class : 2 eyes from dog class : 2 tail from dog class : 1
 Called inside animal class : Eat
 Called inside animal class : Move
```
