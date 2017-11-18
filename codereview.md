### Code Review

#### Types:
1.Use primitive type `boolean` over `Boolean` if we only need to check true or false. `Boolean` comes with additional methods and avoiding will help in memory consumption.
```java  
//Boolean checkStatus = true;
boolean checkStatus = true;
```
#### POJO:
2.Use `this.variableName = variableName;` over setter in POJO's if we are using the constructors. By doing this we can guarantee variable initialization when the class object is initialized.
```java
//public int setSize(int size) { this.setSize = size; };
this.size = size; 
```
