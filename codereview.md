### Code Review

#### Types:
1.Use primitive type `boolean` over `Boolean` if we only need to check true or false. `Boolean` comes with additional methods and avoiding will help in memory consumption.
```java  
//Boolean checkStatus = true;
boolean checkStatus = true;
```
#### Constructor:
2.Use `this.variableName = variableName;` over setter in POJO's if we are using the constructors. By doing this we can guarantee variable initialization when the class object is initialized.
```java
//public int setSize(int size) { this.setSize = size; };
this.size = size;
```
3.Using `super` vs `parentMethod` directly. In inherited child class we can refer parent class property / method using `super` keyword. This will work just fine, however if we ever want to overwrite parent class method in child class ( method overriding ) then all child class method will not be called if we have referenced using `super`.
```java
//super.parentClassMethod();
parentClassMethod(); //this will check if current (child) class has this method if not then it will refer parent class method.
```

### Try Catch
1. Write stack-trace in logger than printing it via printStackTrace(). Writing something in custom logger will give us an option to move, review or reuse in other places for troubleshooting. Also if we are printing in logger we don't have to do `e.printStackTrace();` in catch block.
```java
catch(Exception e){
  //e.printStackTrace(); can be avoided as we are capturing in the log.
  logger.error(" Problem while processing your payment", e);
}
```
2. If we need to Re-Throw exception
In few scenario we want to throw exception and not swallow it in catch block. Then for better logging / troubleshooting we can catch an exception and re-throw the same.
```java
try{
  ....
} catch (SQLException e){
  logger.e("Problem writing data to database", e);
  throw e; //this will throw the same exception to it's calling method/class. 
}
```
