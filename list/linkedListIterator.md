#### LinkedList Iterator

In below example we will be adding items to the List, however we will prevent adding "duplicates" and add items in order so we don't have to sort list later.

```java
package com.company;

import java.util.Iterator;
import java.util.LinkedList;
import java.util.ListIterator;
import java.util.logging.Level;
import java.util.logging.Logger;
import java.util.logging.ConsoleHandler;

public class Main {

    private static final Logger logger = Logger.getLogger(Main.class.getName());

    public static void main(String[] args) {

        // making logger print message in console.
        ConsoleHandler handler = new ConsoleHandler();
        handler.setLevel(Level.ALL);

        // initializing our list
        LinkedList<String> newList = new LinkedList<>();

        // sending to method to add the items in order and to prevent duplicates
        addItemInOrder(newList, "Banana");
        addItemInOrder(newList, "Orange");
        addItemInOrder(newList, "Apple");
        addItemInOrder(newList, "Blueberry");
        addItemInOrder(newList, "grapes");
        addItemInOrder(newList, "Mango");
        addItemInOrder(newList, "Apple");

        // let us verify if the items are added in order
        printListItems(newList);

    }

    public static void printListItems(LinkedList<String> linkedList) {
        Iterator<String> iterator = linkedList.listIterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }

    public static boolean addItemInOrder(LinkedList<String> linkedList, String newItem) {
        // instantiate the listIterator to be used.
        ListIterator<String> listIterator = linkedList.listIterator();

        while (listIterator.hasNext()) {
            // check if we have newItem in the list ( prevent duplicates )
            int compareItem = listIterator.next().compareTo(newItem);
            if (compareItem == 0) {
                // we found the matching, so lets do nothing about it.
                logger.info(" we already have item : " + newItem + " in the list. ");
                return false;
            } else if (compareItem > 0) {
                // this means the value of newItem is greater than previous, so need to add before
                listIterator.previous();
                listIterator.add(newItem);
                return true;
            } else {
                // compareItem < 0 i.e we should add normally, that is handled at last hence no return
            }
        }

        // normal addition of item to the list
        listIterator.add(newItem);
        return true;
    }
}
```
Resulting output would be

```java
Apple
INFO:  we already have item : Apple in the list.
Banana
Blueberry
Mango
Orange
grapes
```
