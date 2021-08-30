/***************************************************************************
Author: Ethan Coomber
Middlebury College

Compilation: javac Dealership.java
Execution: java Dealership Cars.txt CarsSold.txt

Implements a hash table to store a car dealership's inventory. Each month,
the dealer receives a new shipment of cars, and the dealer also sell several cars
each month. The updated inventory is printed.
 **************************************************************************/

import java.util.Scanner;
import java.lang.Math;
import java.io.File;
import java.util.ArrayList;

class Car{
 public String VINnumStr;
 public String yearStr;
 public int year;
 public String vehicleType;
 public String manufacturer;
 public String monthStr;
 public int month;


 Car(String info){
   //Initializes the information that is passed in
   String[] carInfo = info.split(" ");
   monthStr = carInfo[0];
   month = Integer.parseInt(monthStr);
   VINnumStr = carInfo[1];
   yearStr = carInfo[2];
   year = Integer.parseInt(yearStr);
   manufacturer = carInfo[3];
   vehicleType = carInfo[4];
 }

 //Allows us to print out the information for each car
 public String toString(){
   return(VINnumStr + " " + yearStr + " " + manufacturer + " " + vehicleType);
 }
}

class HashTable{
  public int capacity;
  //Stores the cars in the lot that can be sold
  public Node[] carsAvailable;

  //Initializes the hash table with an array of size 100
  HashTable(){
     capacity = 100;
     carsAvailable = new Node[capacity];
  }

  class Node{
    Car car;
    Boolean deletion;

    //Initializes the node with car as a parameter
    public Node(Car car_){
      car = car_;
      deletion = false;
    }

    // Sets the boolean deletion to true to indicate the car has been sold
    public void delete(){
      deletion = true;
    }

    // Checks to see if the car has been sold
    public boolean isDeleted(){
      if(deletion == true){
        return true;
      } else {
        return false;
      }
    }
  }

  //Assigns each VIN to a hash value (Given code)
  public long hashCode(String s){
    long hash = 0;
    for(int i = 0; i<s.length(); i++){
        hash = hash * 31 + s.charAt(i);
    }
    return hash;
  }

  //Adds a car to the array using the hash valule
  public void add(Car current){
    //These three values wind up giving us the index the car will go in
    long hashNum = Math.abs(hashCode(current.VINnumStr));
    long indexLong = hashNum % (long)capacity;
    int index = (int)indexLong;


    boolean placed = false;
    while(!placed){
      //Checks is the space in the array is empty or if the car there has been sold
      if(carsAvailable[index] == null || carsAvailable[index].isDeleted()){
        Node newCar = new Node(current);
        //Places the car
        carsAvailable[index] = newCar;
        placed = true;
      }
      else{
        //Otherwise we look for a spot to put the car
        index++;
        if(index == capacity){
          //Wraps around once we reach the end
          index = 0;
        }
      }
    }
  }

  public int delete(Car removed){
    /*This function should also return the number of indexes required to
    search in order to find and delete the object. */

    //Similarly to add, these three longs and ints wind up giving us the
    // index we are looking for
    long hashNum = Math.abs(hashCode(removed.VINnumStr));
    long indexLongRemove = hashNum % (long)capacity;
    int indexRemove = (int)indexLongRemove;


    boolean found = false;
    //checks will be used later to give us the search time
    int checks = 1;
    while(!found){
      //Checks to see if the index we're looking at has the car
      if(carsAvailable[indexRemove].car.VINnumStr.equals(removed.VINnumStr)){
        carsAvailable[indexRemove].delete();
        found = true;
      }
      else{
        //We keep looking
        indexRemove++;
        checks++;

        if(indexRemove == capacity){
          indexRemove = 0;
        }
      }
    }
    return checks;
  }

  public void resize(){
    //Temporarily copies our array of cars
    Node[] temp = carsAvailable;

    //Re-sies
    capacity = capacity*2;

    //Creates a new array for carsAvailable of the size we want
    carsAvailable = new Node[capacity];

    //Goes through and re-adds all the cars
    for(int i = 0; i < temp.length; i++){
      if(temp[i] != null){
        add(temp[i].car);
      }
    }
  }
}

class Dealership{

 public static void main(String[] args){
   //These scanners will be used later
   Scanner sc = new Scanner(System.in);
   Scanner sc1 = new Scanner(System.in);
   Scanner sc2 = new Scanner(System.in);

   System.out.println("Welcome to the CS 201 New/Used Car Dealership!");


   //These two files are read from the command line
   String fileName1 = args[0];
   String fileName2 = args[1];
   File file1 = new File(fileName1);
   File file2 = new File(fileName2);

   HashTable hash = new HashTable();

   String lines;
   String linesSold;
   int total = 0;

   //Checks each month in this for loop
   for(int ii = 1; ii <= 12; ii++){
     //These doubles are used when we print information about how many cars we add
     // and remove
     double temp = 0;
     double soldCars = 0;
     double newAddition = 0;
     //Checks to make sure there is a file we can read each time so we start from
     //The first line
     try {
       sc = new Scanner(file1);
     } catch (Exception e) {
       System.out.println("There is no file 1.");
       return;
     }

     try {
       sc1 = new Scanner(file2);
     } catch (Exception e) {
       System.out.println("There is no file 2.");
       return;
     }
     //Goes through each line of the file adding cars
     while(sc.hasNextLine()){
       lines = sc.nextLine();
       Car latest = new Car(lines);
       //Makes sure we are adding the right month
       if(latest.month == ii){
         //Checks if we need to resize
         if(total == hash.capacity){
           hash.resize();
         }
         hash.add(latest);
         newAddition++;
         total++;
       }
     }
     //Goes through the second file of cars we are selling
     while(sc1.hasNextLine()){
       linesSold = sc1.nextLine();
       Car sold = new Car(linesSold);
       //Deletes the cars of the month we are on
       if(sold.month == ii){
         temp = temp + hash.delete(sold);
         soldCars++;
         total--;
       }
     }
     System.out.println("");
     //Uses a switch case to determine which month to print
     switch(ii){
       case 1: System.out.println("January Inventory");
               break;
       case 2: System.out.println("February Inventory");
               break;
       case 3: System.out.println("March Inventory");
               break;
       case 4: System.out.println("April Inventory");
               break;
       case 5: System.out.println("May Inventory");
               break;
       case 6: System.out.println("June Inventory");
               break;
       case 7: System.out.println("July Inventory");
               break;
       case 8: System.out.println("August Inventory");
               break;
       case 9: System.out.println("September Inventory");
               break;
       case 10: System.out.println("October Inventory");
               break;
       case 11: System.out.println("November Inventory");
               break;
       case 12: System.out.println("December Inventory");
               break;
       default: System.out.println("There was a problem");
     }
     //Prints the intromation we want for each month
     System.out.println("New: " + newAddition + " Sold: " + soldCars +
     " Total: " + (total) + " Capacity: " + hash.capacity);
     System.out.println("Avg num. of keys indexed for retrieval: " + temp / soldCars);
   }

   System.out.print("\nPrint Inventory (y/n):  ");
   String inventory = sc2.next();
   //Goes through and prints the left over cars
   if(inventory.equals("y")){
     for(int i = 0; i < hash.carsAvailable.length; i++){
       //System.out.println(hash.carsAvailable[i].car);
       if(hash.carsAvailable[i] != null && hash.carsAvailable[i].isDeleted() == false){
         System.out.println(i + ": " + hash.carsAvailable[i].car);
       }
     }
   } else {
     System.out.println("Goodbye.");
   }

 }
}
