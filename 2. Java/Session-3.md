# Session - 3 | Object Oriented Programming

- super() keyword calls the constructor of the superclass.
- Whenever control comes into the constructor, before it constructs the child class object, it is going to construct the super class object
- super() should be kept first, otherwise it'll throw an error
- The Car class extends Vehicle because of code reuse.
- Card IS-A Vehicle
- Car extends vehicle this means that if Vehicle has rtoNumber then Car will also have it.
- If Vehicle's rtoNumber is private then Car cannot access it directly
- But when the Car is going to be constructed through Constructor
	- We may pass this rtoNumber in super() as 👉 super(rtoNumber);
- Every Car will have rtoNumber as it extends it from the Vehicle,
- But only Car can have musicSystem not Vehicle.
- Their is a start method in Vehicle, but there is no start method in the Car
 - Rule: from any static method, we can call only other static methods or we can access only static variables
 	- Thus making the methoad `takeTestRide` as `static`
 - Since, our purpose is to check inheritance, that is why we have added static keyword, in general we should avoid it.
 - Whenever we are adhering an object of base type all the objects of class type should also adhere
 - Remember, for vehicle their is start method, but for car there is no start method
 	- Even though car doesn't have start method, but don't forget that car is a derived class of vehicle, and it's inheriting its property, hence it's start method also gets inherited
 
#### Vehicle.java
```java
package com.psl.vehicles;

public class Vehicle {
	private String rtoNumber;
	
	public Vehicle(String rtoNumber) {
		super();
		this.rtoNumber = rtoNumber;
	}
	
	public void start() {
		System.out.println("Vehicle Started.");
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("[");
		builder.append(rtoNumber);
		builder.append("]");
		
		return builder.toString();
	}
}
```

#### Car.java
```java
package com.psl.vehicles;

public class Car extends Vehicle {
	private String musicSystem;

	public Car(String rtoNumber, String musicSystem) {
		//passing rtoNumber as it belongs to Vehicle NOT Car
		super(rtoNumber);
		this.musicSystem = musicSystem;
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Car ");
		builder.append(" (musicSystem: ");
		builder.append(musicSystem);
		builder.append(" )");
		
		return builder.toString();
	}
	
}
```

### TestClient.java
```java
package com.psl.vehicles;

public class TestClient {
	public static void main(String[] args) {
		Vehicle vehicle = new Vehicle("MH12 AB 1234");
		System.out.println(vehicle);
		takeTestRide(vehicle);
		
		Car car = new Car("WB15 AB 2525", "Sony");
		System.out.println(car);
		takeTestRide(vehicle);
	}
	
	public static void takeTestRide(Vehicle vehicle) {
		vehicle.start(); 
	}
}
```

#### Output
```
[MH12 AB 1234]
Car [musicSystem=Sony, {WB15 AB 2525}]
```

- Source -> Generate toString -> Inherited Methods -> toString() ---> String Concatenation
- super.toString() is going to call toString method of parent class i.e., Vehicle
- We have overriden the start method in Car class
- Now what should we expect?
	- Now it will show, "Car Started" in the output
	- This is polymorphism (method overriding) at run time/late binding/dynamic binding
- Adding start method in Car class
- Creating a new class Truck and it's object along with the start method
- Open-Close Principle (SOLID Principle) means **Open** for *extension* and **Close** for *modification*

### Modification Version - 1

#### Vehicle.java
```java
package com.psl.vehicles;

public class Vehicle {
	private String rtoNumber;
	
	public Vehicle(String rtoNumber) {
		super();
		this.rtoNumber = rtoNumber;
	}
	
	public void start() {
		System.out.println("Vehicle Started.");
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("[");
		builder.append(rtoNumber);
		builder.append("]");
		
		return builder.toString();
	}
}
```

#### Car.java
```java
package com.psl.vehicles;

public class Car extends Vehicle {
	private String musicSystem;

	public Car(String rtoNumber, String musicSystem) {
		//passing rtoNumber as it belongs to vehicle
		super(rtoNumber);
		this.musicSystem = musicSystem;
	}
	//overriding the base class method
	@Override
	public void start() {
		System.out.println("Car Started.");
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Car ");
		builder.append(" (musicSystem: ");
		builder.append(musicSystem);
		builder.append(" )");
		
		return builder.toString();
	}
}
```

#### Truck.java
```java
package com.psl.vehicles;

public class Truck extends Vehicle {
	private int load;
	
	public Truck(String rtoNumber, int load) {
		super(rtoNumber);
		this.load = load;
	}
	
	@Override
	public void start() {
		System.out.println("Truck Started.");
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(Load: ");
		builder.append(load);
		builder.append(")");
		return builder.toString();
	}
}
```

#### TestClient.java
```java
package com.psl.vehicles;

public class TestClient {
	public static void main(String[] args) {
		Vehicle vehicle = new Vehicle("MH12 AB 1234");
		System.out.println(vehicle);
		takeTestRide(vehicle);
		
		Car car = new Car("WB15 AB 2525", "Sony");
		System.out.println(car);
		takeTestRide(vehicle);
		
		Truck truck = new Truck("UK01 BF 1593", 3);
		System.out.println(truck);
		takeTestRide(truck);
	}
	//this code is taking care of all the future classes; Open-Close Principle
	public static void takeTestRide(Vehicle vehicle) {
		vehicle.start(); 
	}
}
```

#### Output
```
[MH12 AB 1111]
Vehicle Started
Car [musicSystem=Sony, {[WB15 AB 2525]}]
Car Started.
Truck [Load=3, {[UK01 BF 1593]}]
Truck Started.
```
### Modification Version - 2
#### TestClient.java
```java
package com.psl.vehicles;

public class TestClient {
	public static void main(String[] args) {
//		Vehicle vehicle = new Vehicle("MH12 AB 1234");
//		System.out.println(vehicle);
//		takeTestRide(vehicle);
		
		Car car = new Car("WB15 AB 2525", "Sony");
		System.out.println(car);
		takeTestRide(vehicle);
		
//		Truck truck = new Truck("UK01 BF 1593", 3);
//		System.out.println(truck);
//		takeTestRide(truck);
	}
	//this code is taking care of all the future classes; Open-Close Principle
	public static void takeTestRide(Vehicle vehicle) {
		vehicle.start(); 
		vehicle.playMusic();
	}
}
```

#### Car.java
```java
package com.psl.vehicles;

public class Car extends Vehicle {
	private String musicSystem;

	public Car(String rtoNumber, String musicSystem) {
		//passing rtoNumber as it belongs to vehicle
		super(rtoNumber);
		this.musicSystem = musicSystem;
	}
	//overriding the base class method
	@Override
	public void start() {
		System.out.println("Car Started.");
	}
	
	public void playMusic() {
		System.out.println("Playing music in the car.");
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Car ");
		builder.append(" (musicSystem: ");
		builder.append(musicSystem);
		builder.append(" )");
		
		return builder.toString();
	}	
}
```

- Writing empty method is not a good practice to code; fragile code
- Whenever you require to add meaningless method, do not add where we had added previously
	- Rather use `instanceof` shown in example
	- And we will also have to downcast it
#### Vehicle.java
```java
public class TestClient {
	public static void main(String[] args) {
		// Vehicle v = new Vehicle("MH12 AB 1234");
		// System.out.println(v);
		// takeTestRide(v);
		//car has got two things, RTO Number & Music System
		Car c = new Car("MH12 AB 2222", "Sony");
		System.out.println(c);
		takeTestRide(c);
		//car has got two things, RTO Number & load
		// Truck t = new Truck("MH12 AB 2222", 3);
		// System.out.println(t);
		// takeTestRide(t);
	}
	//modification is done here 👇
	public static void takeTestRide(Vehicle vehicle) {
		vehicle.start();
		if(vehicle instanceof Car) {
			//downcasting👇
			((Car)vehicle).playMusic();
		}
	}
}
```

#### Output	
```
Car [musicSyste=Sony, {[MH12 AB 2222]}]
Car Started.
Playing musinc in the Car.
```

- Why is it the case that it showing an error, while calling the playMusic of Car?
- When we were calling the start method of car then it was calling, but why does it throws an error while calling the `playMusic()`?
- 👉 Even though Car has got `playMusic()` but it's not guaranteed that the Vehicle which will come in will be a Car.
	- So, inside the method `takeTestRide()` we cannot guarantee what type of vehicle will be passed, it can be anything, any object can be passed
	- Hence, we cannot call a method, which is not their in the base class i.e., Vehicle
	- To overcome this problem, we are using `instanceof` keyword.
	- OR, we can add an empty implementation of playMusic() in the Vehicle class. (avoid this)
- Problem arises, why are we taking Car separately when we are already calling it under vehicle object
	- this leads to a fragile code
	```java
	if(vehicle instanceof Car) {
			//downcasting👇
			((Car)vehicle).playMusic();
		}
	```

### Modification Version - 3
- If I make vehicle class as absstract it'll not allow to make instance of vehicle in the TestClient.java file, will throw an error
- Marking class abstract we are telling compiler, we don't have any intention to create instance of it
- As the best programming practice, all the base classes needs to be abstract
- If their is any abstract method in base class, then it must be implemented in one of its child class, and child class must override it
- Even if their is a single method declared as abstract in the class, then the class must be declared as abstract
- Therefore, abstract keyword class level means you cannot make instance
- And abstract keyword at method level means you cannot write anything under the method but it must be overriden by the child classes

#### Vehicle.java
```java
public abstract class Vehicle {
	private String rtoNumber;
	
	public Vehicle(String rtoNumber) {
		super();
		this.rtoNumber = rtoNumber;
	}
	//this method will never get called, because vehicle has been marked as abstract
//	public void start() {
//		System.out.println("Car started.");
//	}
	//for the above problem the best soln is to make the method abstract
	public abstract void start(); //if i say abstract, i don't need to provide any implementation
	//👆 it's just a placeholder for the child class to override.
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("[");
		builder.append("rtoNumber=");
		builder.append(rtoNumber);
		builder.append("]");
		return builder.toString();
	}
}
```
#### TestClient.java
```java
public class TestClient {
	public static void main(String[] args) {
		Vehicle v = new Vehicle("MH12 AB 1234");
		System.out.println(v);
		takeTestRide(v);
		
		//car has got two things, RTO Number & Music System
		Car c = new Car("MH12 AB 2222", "Sony");
		System.out.println(c);
		takeTestRide(c);
		
		Truck t = new Truck("MH12 AB 2222", 3);
		System.out.println(t);
		takeTestRide(t);
	}
	
	public static void takeTestRide(Vehicle vehicle) {
		vehicle.start();
	}
}
```

#### Output
*Same as previous*

----------------------------------------------

- **Interface** is used to make to contact with the client
- Creating Class: AirCraft, Animal, Interface: FlyingObject
- We want AirCraft to fly, we can add this functinality using the `implements` keyword; *attaching interface to class*
- Since, Bird is also able to fly, hence **implementing**
- Bird get properties from Animal and implements FlyingObject interface

#### FlyingObject.java
```java
package com.psl.vehicles;

public interface FlyingObject {
	//no need to wrtie public
	//bcoz whatever we write in an interface it is considered to be as puble nd abstract
	void fly();
}
```

#### AirCraft.java
```java
package com.psl.vehicles;

public class AirCraft extends Vehicle implements FlyingObject {
	public AirCraft() {
		super("Not Required");
	}

	@Override
	public void start() {
		System.out.println("AirCraft Started.");
	}

	@Override
	public void fly() {
		System.out.println("Flying AirCraft");
	}
}
```

#### Animal.java
```java
package com.psl.vehicles;

public abstract class Animal {
	public void eat() {
		System.out.println("Animal is eating.");
	}
}
```

#### Bird.java
```java
package com.psl.vehicles;

public class Bird extends Animal implements FlyingObject{
	@Override
	public void eat() {

	}

	@Override
	public void fly() {
		System.out.println("Bird is flying.");
	}
}
```

#### TestClient.java
```java
public class TestClient {
	public static void main(String[] args) {
		Vehicle v = new Vehicle("MH12 AB 1234");
		System.out.println(v);
		takeTestRide(v);
		
		//car has got two things, RTO Number & Music System
		Car c = new Car("MH12 AB 2222", "Sony");
		System.out.println(c);
		takeTestRide(c);
		
		Truck t = new Truck("MH12 AB 2222", 3);
		System.out.println(t);
		takeTestRide(t);

		// FlyingObject flying = new Bird();
		// flying.fly();
		// flying.eat(); //error

		// Animal animal = new Bird();
		// animal.eat();
		// animal.fly(); //error

		FlyingObject bird = new Bird();
		//it'll work bcoz; their is a method of fly() in Bird
		testInAir(fly);
		//fails👇 bcoz Truck doesn't implement FlyingObject
		FlyingObject truck = new Truck();
		//it'll work
		FlyingObject airCraft = new AirCraft();
		Bird bird = new Bird();
		//it works👇
		feed(bird);

	}
	//showing different behaviours👇
	public static void takeTestRide(Vehicle vehicle) {
		vehicle.start();
	}

	public static void testInAir(FlyingObject fo) {
		//i can only fly; bcoz FlyingObject has that method
		fo.fly();
	}

	public static void feed (Animal animal) {
		animail.eat();
	}
}
```

----------------------------------------------------------

#### StringHandler.java
```java
public class StringHandler {
	String s1 = "ABC";
	//avoid using this way👇
	String s2 = new String("PQR");
	//concat means add two string together
	System.out.println("ABC");
	//string is immutable; once it's created it'll never change
	s1.concat("DEF");
	System.out.println(s1); //ABC
	//on LHS: concatenating it and on RHS:capturing it
	String s3 = s1.concat("DEF");
	System.out.println(s3);
}
```

---------------------------------------------------------

## DigitalDrawingBoard Example
#### Shape.java
```java
public abstract class Shape {
	public abstract void draw();
	public double computeArea();

	public Shape() {
		super();
	}

	public void move() {
		System.out.println("Shape Moved.");
	}

	public boolean isOverlapping() {
		return false;
	}

}
```

If i declare the RegularShape class as abstract, then i don't need to have the responbility of implemting methods, rather i am pushing it to the child classes.

#### RegularShape.java
```java
public abstract class RegularShape extends Shape {
	//referring to the Point class
	private Point center;

	public RegularShape(Point center) {
		super();
		this.center = center;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("RegularShape ( center : ");
		builder.append(center);
		builder.append(" )");

		return builder.toString();
	}
}
```

#### Point.java
```java
public class Point {
	private double x;
	private double y;

	Point() {
		super();
		this.x = x;
		this.y = y;
	}

	private double getX() { return x; }
	private double getY() { return y; }

	private void setX(double x) { this.x = x; }
	private void setY(double y) { this.y = y; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Point ");
		builder.append("(");
		builder.append(x);
		builder.append(", ");
		builder.append(y);
		builder.append(")");

		return builder.toString();
	}
}
```

#### Square.java
```java
public class Square extends RegularShape {
	private double side;

	public Sqaure(Point center, double side) {
		super(center);
		this.side = side;
	}

	@Override
	public void draw() {
		System.out.println("Square drawn");
	}

	@Override
	public void computeArea() {
		return side * side;
	}
}
```

#### Rectanlge.java
```java
public class Rectangle extends RegularShape {
	private double length;
	private double width;

	public Rectangle(Point center, double length, double width) {
		super(center);
		this.length = length;
		this.width = width;
	}

	@Override
	public void draw() {
		System.out.println("Rectangle drawn.");
	}

	@Override
	public double computeArea() {
		return length * width;
	}
}
```

#### Polygon.java
```java
package com.digiboard.shape;

import java.util.List;
import java.util.ArrayList;
import java.util.Set;
import java.util.HashSet;

public class Polygon extends Shape {
	private int noOfSides;
	privatet List<Point> points;

	public Polygon() {
		vertices = new ArrayList<Point>();
	}

	public void add(Point vertex) {
		vertices.add(vertex);
	}

	@Override 
	public void draw() {
		System.out.println("Polygon drawn");
		noOfSides = vertices.size();
	}

	@Override
	public double computeArea() {
		return 1000;
	}

	public boolean isValid() {
		return vertices.size() > 2;
	}

	private int getNoOfSides() { return vertices.size(); }
	private void setNoOfSides(int noOfSides) { this.noOfSides = noOfSides; }
}
```

#### DrawingBoard.java
```java
public class DrawingBoard {
	private List<Shape> shapes;

	public DrawingBoard() {
		super();
		shapes = new ArrayList<Shape>();
	}

	public void addShape() {
		if(!shape.isOverlapping())
			shapes.add(shape);
	}

	public double computeTotalArea() {
		double totalArea = 0;
		for(Shape shape: shapes) {
			totalArea += shape.computeArea();
		}
		return totalArea;
	}
}
```

#### Draftsman.java
```java
package com.psl.digiboard.board;

public class Draftsman {
	private String empId;
	private DrawingBoard board;

	public void assignDrawingBoard(DrawingBoard board) {
		this.board = board;
	}

	public void draw() {
		Point sqCenter = new Point(20, 30);
		Square s1 = new Square(sqCenter, 20);
		board.add(s1);
		Rectangle r1 = new Square(sqCenter, 22, 12);
		board.add(r1);
		System.out.println(board.computeTotalArea());
	}
}
```

#### TestClient.java
```java
public class TestClient {
	public static void main (String[] args) {
		Draftsman drafts = new Draftsman();
		d.assignDrawingBoard(new DrawingBoard());
		d.draw();
	}
}
```

#### Output
```
664.0
```













