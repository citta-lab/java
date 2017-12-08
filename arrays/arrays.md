### Arrays

There are various ways we can declare an array, let's say we need to initialize an array name `checkNumbers` then we do it by declaring `int[] checkNumbers = new int[5];` Once the array is declared and values are assigned then we cannot add more values by changing the size with ease. Typical way of assigning values to an array is something like `checkNumbers[0] = 10; checkNumbers[1] = 14; checkNumbers[2] = 45;` etc. Another way is something like `int[] checkNumbers = { 12, 3, 34, 1, 55};`. In this case we told java we define the size and the values as well.

#### 1. Sort Array

Say we need to sort array in descending order, then we need to compare value of each array elements until all the array elements are complete, and keep continue until all elements are sorted.
```java
public static int[] sortArray(int[] array){
        int temp;
        boolean flag = true; /*execute by default*/
        while(flag){
            flag = false; /*stop while loop, change to true if we find sorting*/
            for ( int i = 0; i < array.length-1; i++){
                if ( array[i] < array[i+1]){
                    temp = array[i];
                    array[i] = array[i+1];
                    array[i+1] = temp;
                    flag = true; /* found sorting, so lets go through the array elements once for-loop ends */
                }
            }
        }
        return array;
    }
```
So if the user sends an array of elements something like `12,34,45,5,13` then in for loop it goes through each element by comparing and alter the array for next for-loop. At the end of first round of for-loop then the array will look like `34,45,12,13,5`. Below is the complete array sorting for first complete round of for-loop ( array.length-1 ).
```java
// 12,34,45,5,13  run i=0
// 34,12,45,5,13  run i=1
// 34,45,12,5,13  run i=2, i=3
// 34,45,12,13,5
```

#### 2. Copy Array

for-loop array copy
```java
int[] newArray = new int[originalArray.length];
for( int i=0; i<originalArray.length; i++){
  newArray[i] = originalArray[i];
}
```
inbuilt array copy
```java
int[] newArray = Arrays.copyOf(originalArray, originalArray.length);
            // = Arrays.copyOf(actual array, length of the array we need );
```

### ArrayList

#### 1.Add, Update, Remove
```java
String newItem = "Bob";
String updateItem = "Rob";
ArrayList<Strings> valueList = new ArrayList<Strings>();
valueList.add(newItem); //Added Bob
valueList.set(position, updateItem); // (0, updateItem) will replace newItem
valueList.get(position); // (0) will return Rob
valueList.remove(position); // (0) will remove Rob and valueList will be empty.
```

#### 2. Copy ArrayList

2.1
```java
ArrayList<String> newList = new ArrayList<String>(oldArrayList.getOldArrayList);
/* (oldArrayList.getOldArrayList) where getOldArrayList is getter method on oldArrayList */
```
2.2
```java
ArrayList<String> newList = new ArrayList<String>();
newList.addAll(oldArrayList.getOldArrayList);
/* (oldArrayList.getOldArrayList) where getOldArrayList is getter method on oldArrayList */
```

#### 3. Extract values from arrayList

If we have an arrayList of type some class and we want to retrieve values then we can use traditional for-loop or latest for each loop as mentioned below, and let us say Contact has constructor which initializes two variables `name` and `phoneNumber`. We also have getter and setter method for the same in Contact class.

3.1 for  
```java
ArrayList<Contact> contact = new ArrayList<Contact>();
// some code implementation which add's values to contact object.
for(int i=0; i<contact.size(); i++){
  String name = contact.get(i).getName(); // retrieving name from contact object by traversing through elements.
}
```

3.2 for each
```java
ArrayList<Contact> contact = new ArrayList<Contact>();
// some code implementation which add's values to contact object.
for(Contact c :contact){
  String name = c.getName();
}
```

#### 4. Factory Method & call constructor from Scanner.

4.1 Normal Way
If we are trying to pass values to constructors to initialize the object with default value then we do it while creating an object. let's say Contact class has default constructor which saves string values `name` and `number`. So the `Contact` class looks like,
```java
public class Contact {
    private String name;
    private String phoneNumber;

    public Contact(String name, String phoneNumber){
        this.setPhoneNumber(phoneNumber);
        this.setName(name);
    }

    public String getName() {
        return name;
    }

    private void setName(String name) {
        this.name = name;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    private void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```
So we can set Contact class variable by calling Constructor as follows,
```java
ArrayList<Contact> buildList = new ArrayList<Contact>();  //Created empty list of type Contact.
Contact contact = new Contact("Bob", "777-234-1122");     // Create new contact object with it's required property.
buildList.add(contact);                                   // Add to the empty list.
```

4.2 Factory Method

If we are trying set the values by taking user input via Scanner then the convenient way is by creating a factory method (static method which cannot be a instantiated) in `Contact` class which intern calls Constructor. i.e
```java
public static Contact createContact(String name, String phone){
  return new Contact(name, phone);
}
```
Adding above method in `Contact` class will let us access the method directly from Class when we accept user input from Scanner.
```java
ArrayList<Contact> buildList = new ArrayList<Contact>();  //Created empty list of type Contact.
private static void addContact(){
  System.out.println(" Enter Name: ");
  String name = scanner.nextLine();
  System.out.println(" Enter Phone Number: ");
  String phone = scanner.nextLine();
  Contact newContact = Contact.createContact(name, phone); //Factory method because createContact is static and we are not creating new class instance.
  buildList.add(newContact);
}
```
