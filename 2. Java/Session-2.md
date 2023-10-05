# Session 2 | Language Fundamentals
## Part 1 Example: (Complex Number Code)
- Writing **throws Exception** in `public static void main(String[] args)`, throws outside the JVM
- Select source, generate getter and setter
- If a class doesn't have any constructure defined, java defines one known as *default constructor*
- Always use stringbuffer or stringbuilder
    - StringBuilder is a mutable variation of String class
    - DO NOT USE StringBuffer, use StringBuilder
- Values cannot be change in the Complex.java file, as everything is private; Immutable object
- StringBuilder is mutable whereas String is immutable
- In general in the projects we never use Scanner kind of thing.
 - Hard coding
- Is this object oriented code?
	- No, still all the methods are static, and those methods are in some other objects/class.

#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {
		Complex c1 = new Complex(10, 20);
		Complex c2 = new Complex(-20, 10);

		String c1AsString = asStringComplexNumber(c1);
		System.out.println(c1AsString);
		System.out.println(asStringComplexNumber(c2));

		String answer = addComplexNumbers(c1, c2);
		System.out.println(answer);
	}
	//static method
	public static String asStringComplexNumber(Complex complex) {
		String ans = complex.realPart + " +i" + complex.imagPart;
		return ans;
	}
	//static method
	public static String addComplexNumbers(Complex first, Complex other) {
		String ans = (first.realPart + other.realPart) + " +j" + (first.imagPart + other.imagPart);
		return ans;
	}
} 
```

- Moving those static methods to the object class, and removing the static keyword
- This pointer addresses to itself
- This as a reference always pointing to the current object
- The Complex class will have only one instance of the object any point of time, whosoever object is executing `this` object belongs to that object.
- Changing -> `String c1AsString = asStringComplexNumber(c1)` to `c1.asStringComplexNumber()` this because c1's object is called the asStringComplexNumber() method
- Since, this `public static String addComplexNumbers(Complex first, Complex other)` is in the another class, we are violating the object oriented priciple of **encapsulation**; *broken encapsulation*
	- The behaviours in in one class (ComplexCalulator.java) and the behavior on which it's working is on the other (Complex.java) 
	- So the class methods are using the details of the some other class
- The way to stop other classes from accessing is to make the attributes **private**
	- Data was not hidden, therefore anybody would have changed it, we were violating the rule of data hiding
- To give only the read access and not to give the write access, we can achieve this by making getters and setters methods
- Instead of allowing them to directly access the method, now we are allowing them to access those using the `get()` and `set()` methods.

### Modification Version - 3
#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {
		Complex c1 = new Complex(10, 20);
		Complex c2 = new Complex(-20, 10);

		String c1AsString = c1.asStringComplexNumber();
		System.out.prinln(c1AsString);
		System.out.println(c2.asStringComplexNumber());

		String answer = addComplexNumbers(c1, c2);
		System.out.println(answer);
	}
	//the code is still not purely object oriented: 
	//this method should belong to the Complex class, because it's dealing with Complex Nos
	public static String addComplexNumbers(Complex first, Complex other) {
		String ans =  (first.getRealPart() + other.getRealPart() + " +j" + (first.getImagPart() + other.getImagPart()));
		return ans;
 	}
}
```

#### Complex.java
```java
public class Complex {
	private int realPart;
	private int imagPart;

	public Complex(int realPart, int imagPart) {
		this.realPart = realPart;
		this.imagPart = imagPart;
	}

	public String asStringComplexNumber() {
		String ans = this.realPart + " +i" + this.imagPart;
	}
	//getter method
	public int getRealPart() { return realPart; }
	public int getImagPart() { return imagPart; }
	//setter method
	public void setRealPart(int realPart) { this.realPart = realPart; }
	public void setImagPart(int imagPart) { this.imagPart = imagPart; }
}
```
- If I write a paremterized constructor then java will not allow me to make an object of the class, and its constructor, if we remove the parametrized constructor, then we can write it
- If there is no constructor, then java makes its own construcstor with no parameter known as **default constructor**
- Remember Constructor Overloading...
- Encapsulation achieved

### Modification Version - 4
#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {		
		Complex c1 = new Complex(10, 20);
		Complex c2 = new Complex(-20, 10);
		//storing it in a local variable
		String c1AsString = c1.asStringComplexNumber();
		//and then printing
		System.out.println(c1AsString);
		//directly accessing and printing
		System.out.println(c2.asStringComplexNumber());
		
		Complex addition = c1.add(c2);
		System.out.println(addition.asStringComplexNumber());
	}
}
```

#### Complex.java
```java
public class Complex {
	private int realPart;
	private int imagPart;	
	//default constructor or normal constructor
	public Complex() {
		this.realPart = 0;
		this.imagPart = 0;
	}
	//parameterized constructor
	public Complex(int realPart, int imagPart) {
		this.realPart = realPart;
		this.imagPart = imagPart;
	}
	
	public String asStringComplexNumber() {
		String ans = this.realPart + " + i" + this.imagPart;
		return ans;
	}
	
	public Complex add(Complex other) {
		Complex addition = new Complex();
		addition.setRealPart(this.getRealPart() + other.getRealPart());
		addition.setImagPart(this.getImagPart() + other.getImagPart());

		return addition;
	}
	//getter method
	public int getRealPart() { return realPart; }
	public int getImagPart() { return imagPart; }
	//setter method
	public void setRealPart(int realPart) { this.realPart = realPart; }
	public void setImagPart(int imagPart) { this.imagPart = imagPart; }
}
 ```

- Introducing *overriding* using **toString()** method
- Alawys use stringbuilder/stringbuffer
- Avoid string concatenation

### Modification Version - 5 (Final: adheres to Complete OO)
#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {		
		Complex c1 = new Complex(10, 20);
		Complex c2 = new Complex(-20, 10);
		
		String c1AsString = c1.toString();
		System.out.println(c1AsString);
		System.out.println(c2.toString());
		
		Complex addition = c1.add(c2);
		System.out.println(addition.toString());
	}
}
```

#### Complex.java
```java
//example of immutable class ✅
public class Complex {
	private int realPart;
	private int imagPart;
	//default constructor or normal constructor
	public Complex() {
		this.realPart = 0;
		this.imagPart = 0;
	}
	//parameterized constructor
	public Complex(int realPart, int imagPart) {
		this.realPart = realPart;
		this.imagPart = imagPart;
	}
//	public String toString() {
//		String ans = this.realPart + " + i" + this.imagPart;
//		return ans;
//	}
	//toString() generated by Right Click -> Source -> Generate toString()
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append(realPart);
		builder.append(", +i");
		builder.append(imagPart);
		return builder.toString();
	}
	
	public Complex add(Complex other) {
		Complex addition = new Complex();
		addition.setRealPart(this.getRealPart() + other.getRealPart());
		addition.setImagPart(this.getImagPart() + other.getImagPart());
		return addition;
	}
	//getter method
	public int getRealPart() { return realPart; }
	public int getImagPart() { return imagPart; }
	//setter method
	public void setRealPart(int realPart) { this.realPart = realPart; }
	public void setImagPart(int imagPart) { this.imagPart = imagPart; }
}
```

----------------------------------------------
## Part 2 Example: (Employee Code)
- Give notification to some other class, when someone creates an instance of the Employee class
 	- The best way to do is to intoduce the constructor itself, below super();
- From any of the three constructors, we need to avoid writing Employee created the same thing in all the constructors
	- To avoid this, the best way to do is to keep the sysout statement in that constructor which has more no of parameters
- And the other constructor will call the other constructor using the `this` keyword, Example: `this(id, "No name");` 
- Now when we run and call all the three, everytime the same msg we will get "Employee created"
- If I want to change the msg, I can do it at only one place
- `this` is called as **constructor chaining**, it's done using the "this" keyword
- **Constructor Chaining** :
	- Constructor chaining is the process of calling one constructor from another constructor with respect to current object.
	- One of the main use of constructor chaining is to avoid duplicate codes while having multiple constructor (by means of constructor overloading) and make code more readable.
	- Constructor chaining can be done in two ways:
		- **Within same class**: It can be done using `this()` keyword for constructors in the same class
		- **From base class**: by using `super()` keyword to call the constructor from the base class.
	- This process is used when we want to perform multiple tasks in a single constructor rather than creating a code for each task in a single constructor we create a separate constructor for each task and make their chain which makes the program more readable. 

#### TestClient.java
```java
public class TestClient {
	public static void main(String[] args) {
		Employee e1 = new Employee();
		Employee e2 = new Employee(100);
		Employee e3 = new Employee(200, "Suresh");		
	}
}
```

#### Employee.java
```java
public class Employee {
	private int id;
	private String name;
	//default constructor
	public Employee() {
		this(0, "No name");
	}
	//parameterized constructor
	public Employee(int id, String name) {
		System.out.println("Employee created.");
		this.id = id;
		this.name = name;
	}
	//overloded (parameterized) constructor
	public Employee(int id) {
		this(id, "No name");
		this.id = id;
	}
}
```

- If there are so many names, such as first name, middle name, last name, then it will be confusing for the attribute String name
- It is confusing which is the first name, middle name, last name
- So for that matter, modifying the code
- Now, imagine 4 attributes for name, and similarly for address
- This list goes huge one, and too many aspects are coming together, and so many get and set methods,

### Modification Version - 1
#### Employee.java
```java
public class Employee {
	private int id;
	private String title;
	private String firstName;
	private String middleName;
	private String lastName;
	
	private String localAptName;
	private String localRoad;
	private String localCity;
	private int localPincode;
	
	private String permAptName;
	private String permRoad;
	private String permCity;
	private int permPincode;
	//default constructor
	public Employee() {
		this(0, "No name");
	}
	//parameterized constructor
	public Employee(int id, String name) {
		System.out.println("Employee created.");
		this.id = id;
		this.name = name;
	}
	//parameterized constructor
	public Employee(int id) {
		this(id, "No name");
		this.id = id;
	}
}
```

- Since title has its own limited possible no of values,so title is an enumeration
- Therefore creating an enumeraton
- We write enum in uppercae

#### Title.java
```java
public enum Title {
	MR, MMRS, MISS, DR
}
```

- To avoid that, instead of keeping it all one place, make another class
- Creating another class for name
- When u make get and set methods() initially mark them as private

#### Name.java
```java
public class Name {
	private Title title;
	private String firstName;
	private String middleName;
	private String lastName;
	
	public Name(Title title, String firstName, String middleName, String lastName) {
		super();
		this.title = title;
		this.firstName = firstName;
		this.middleName = middleName;
		this.lastName = lastName;
	}
	
	private Title getTitle() { return title; }
	private String getFirstName() { return firstName; }
	private String getMiddleName() { return middleName; }
	private String getLastName() { return lastName; }
	
	private void setTitle(Title title) { this.title = title; }
	private void setFirstName(String firstName) { this.firstName = firstName; }
	private void setMiddleName(String middleName) { this.middleName = middleName; }
	private void setLastName(String lastName) { this.lastName = lastName; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Name [title=");
		builder.append(title);
		builder.append(", firstName=");
		builder.append(firstName);
		builder.append(", middleName=");
		builder.append(middleName);
		builder.append(", lastName=");
		builder.append(lastName);
		builder.append("]");
		return builder.toString();
	}
}
```

- Validating method, by creating boolean isValid
```java
	boolean isValid() {
		return true;
	}
```
- Now a data type name has been created, which can be tested

#### Name.java
```java
public class Name {
	private Title title;
	private String firstName;
	private String middleName;
	private String lastName;
	
	public Name(Title title, String firstName, String middleName, String lastName) {
		super();
		this.title = title;
		this.firstName = firstName;
		this.middleName = middleName;
		this.lastName = lastName;
	}
	
	private Title getTitle() { return title; }
	private String getFirstName() { return firstName; }
	private String getMiddleName() { return middleName; }
	private String getLastName() { return lastName; }
	
	private void setTitle(Title title) { this.title = title; }
	private void setFirstName(String firstName) { this.firstName = firstName; }
	private void setMiddleName(String middleName) { this.middleName = middleName; }
	private void setLastName(String lastName) { this.lastName = lastName; }

	boolean isValid() {
		return true;
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append(title);
		builder.append(" ");
		builder.append(firstName);
		builder.append(" ");
		builder.append(middleName);
		builder.append(" ");
		builder.append(lastName);
		return builder.toString();
	}
}
```

- Creating a different class Address
#### Address.java
```java
public class Address {
	private String aptName;
	private String road;
	private String city;
	private int pincode;

	public Address(String aptName, String road, String city, int pincode) {
		super();
		this.aptName = aptName;
		this.road = road;
		this.city = city;
		this.pincode = pincode;
	}

	private String getAptName() { return aptName; }
	private String getRoad() { return road; }
	private String getCity() { return city; }
	private int getPinCode() { return pincode; }

	private void setAptName(String aptName) { this.aptName = aptName; }
	private void setRoad(String road) { this.road = road; }
	private void setCity(String city) { this.city = city; }
	private void setPinCode() { this.pincode = pincode; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("[");
		builder.append(aptName);
		builder.append(", ");
		builder.append(road);
		builder.append(", ");
		builder.append(city);
		builder.append(" - ");
		builder.append(pincode);
		builder.append("]");
		return builder.toString();
	}
```

- Testing again
- NOTE: super() always must be the first line in the constructor

#### Modification Version
#### TestClient.java
```java
package com.psl.complex;

import com.psl.employee.Address;
import com.psl.employee.Name;
import com.psl.employee.Title;

//first importing Name class
public class TestClient {
	public static void main(String[] args) {
		//creating new instance of name
		Name myName = new Name(Title.MISS, "Seeta", "Vinay", "Lodha");
		System.out.println(myName);
		Address addr = new Address("Gulmohar", "MG Road", "Mumbai", 400001);
		System.out.println(addr);
	}
}
```

- Now changing the employess
#### Employee.java
```java
public class Employee {
	private int id;
	private Name name;
	private Address localAddr;
	private Address permAddr;
	
	public Employee(int id, Name name, Address localAddr, Address permAddr) {
		super();
		this.id = id;
		this.name = name;
		this.localAddr = localAddr;
		this.permAddr = permAddr;
	}

	private int getId() { return id; }
	private Name getName() { return name; }
	private Address getLocalAddr() { return localAddr; }
	private Address getPermAddr() { return permAddr; }

	private void setId(int id) { this.id = id; }
	private void setName(Name name) { this.name = name; }
	private void setLocalAddr(Address localAddr) { this.localAddr = localAddr; }
	private void setPermAddr(Address permAddr) { this.permAddr = permAddr; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Employee [id=");
		builder.append(id);
		builder.append(", name=");
		builder.append(name); //it interally call name.toString()
		builder.append(", localAddr=");
		builder.append(localAddr);
		builder.append(", permAddr=");
		builder.append(permAddr);
		builder.append("]");
		return builder.toString();
	}	
}
```

- The output gives the object model

### Final Code
#### TestClient.java
```java
import com.psl.employee.Address;
import com.psl.employee.Employee;
import com.psl.employee.Name;
import com.psl.employee.Title;

//first importing Name class
public class TestClient {
	public static void main(String[] args) {
		//creating new instance of name
		Name myName = new Name(Title.MISS, "Seeta", "Vinay", "Lodha");
		System.out.println(myName);
		Address localAddr = new Address("Gulmohar", "MG Road", "Mumbai", 400001);
		System.out.println(localAddr);
		//creating instance of an employee
		Employee employee1 = new Employee(230, myName, localAddr, localAddr);
		System.out.println(employee1); //internally, employee1.toString()
	}
}
```
#### Employee.java
```java
package com.psl.employee;

public class Employee {
	private int id;
	private Name name;
	private Address localAddr;
	private Address permAddr;
	
	public Employee(int id, Name name, Address localAddr, Address permAddr) {
		super();
		this.id = id;
		this.name = name;
		this.localAddr = localAddr;
		this.permAddr = permAddr;
	}

	private int getId() { return id; }
	private Name getName() { return name; }
	private Address getLocalAddr() { return localAddr; }
	private Address getPermAddr() { return permAddr; }

	private void setId(int id) { this.id = id; }
	private void setName(Name name) { this.name = name; }
	private void setLocalAddr(Address localAddr) { this.localAddr = localAddr; }
	private void setPermAddr(Address permAddr) { this.permAddr = permAddr; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Employee [id=");
		builder.append(id);
		builder.append(", name=");
		builder.append(name); //it interally call name.toString()
		builder.append(", localAddr=");
		builder.append(localAddr);
		builder.append(", permAddr=");
		builder.append(permAddr);
		builder.append("]");
		return builder.toString();
	}	
}
```
#### Name.java
```java
public class Name {
	private Title title;
	private String firstName;
	private String middleName;
	private String lastName;
	
	public Name(Title title, String firstName, String middleName, String lastName) {
		super();
		this.title = title;
		this.firstName = firstName;
		this.middleName = middleName;
		this.lastName = lastName;
	}
	
	private Title getTitle() { return title; }
	private String getFirstName() { return firstName; }
	private String getMiddleName() { return middleName; }
	private String getLastName() { return lastName; }
	
	private void setTitle(Title title) { this.title = title; }
	private void setFirstName(String firstName) { this.firstName = firstName; }
	private void setMiddleName(String middleName) { this.middleName = middleName; }	
	private void setLastName(String lastName) { this.lastName = lastName; }

	boolean isValid() {
		return true;
	}
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append(title);
		builder.append(" ");
		builder.append(firstName);
		builder.append(" ");
		builder.append(middleName);
		builder.append(" ");
		builder.append(lastName);
		return builder.toString();
	}
}
```
#### Adress.java
```java
package com.psl.employee;

public class Address {
	private String aptName;
	private String road;
	private String city;
	private int pincode;
	
	public Address(String aptName, String road, String city, int pincode) {
		super();
		this.aptName = aptName;
		this.road = road;
		this.city = city;
		this.pincode = pincode;
	}

	private String getAptName() { return aptName; }
	private String getCity() { return city; }
	private int getPincode() { return pincode; }
	private String getRoad() { return road; }

	private void setAptName(String aptName) { this.aptName = aptName; }
	private void setRoad(String road) { this.road = road; }
	private void setCity(String city) { this.city = city; }
	private void setPincode(int pincode) { this.pincode = pincode; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("[");
		builder.append(aptName);
		builder.append(", ");
		builder.append(road);
		builder.append(", ");
		builder.append(city);
		builder.append("-");
		builder.append(pincode);
		builder.append("]");
		return builder.toString();
	}
}
```

#### Title.java
```java
package com.psl.employee;
// we write enum in uppercae
public enum Title {
	MR, MMRS, MISS, DR
}
```





































