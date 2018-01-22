#### Constructor Example:

BankCustomer class has three constructors and if nothing is passed we would be calling the first constructors and which in turn calls the third constructor to initialize with the default values `bob, bob@gmail.com, 2400`. Similarly, if we call the class with two parameters then the third parameter `creditLimit` will be set to default value.

#### Step 1: Defining a class with constructors
```java
public class BankCustomer {

    private String name;
    private String email;
    private int creditLimit;

    /* #1: default constructor with no parameter */
    public BankCustomer (){
        this("Bob", "bob@gmail.com", 2400);
    }

    /* #2: constructor with two parameter */
    public BankCustomer(String name, String email){
        this(name, email, 2400);
    }

    /* #3: constructor with all three parameter */
    public BankCustomer(String name, String email, int creditLimit){
        this.name = name;
        this.email = email;
        this.creditLimit = creditLimit;
    }

    /* hence the setter is already taken care by constructor initialization, we just need to
  create getter method to access.
   */
    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    public int getCreditLimit() {
        return creditLimit;
    }
}
```

#### Step 2: Calling the class

```java
/* step1: checking the default constructor */
BankCustomer bCustomer = new BankCustomer();
System.out.println(" Name : "+bCustomer.getName()+ " Email : "+bCustomer.getEmail()+ " Credit : "+bCustomer.getCreditLimit()); // Name : Bob Email : bob@gmail.com Credit : 2400

/* step2: checking constructor with two param */
BankCustomer cCustomer = new BankCustomer("Jack", "jackie@hotmail.com");
System.out.println(" Name : "+cCustomer.getName()+ " Email : "+cCustomer.getEmail()+ " Credit : "+cCustomer.getCreditLimit()); // Name : Jack Email : jackie@hotmail.com Credit : 2400
```
