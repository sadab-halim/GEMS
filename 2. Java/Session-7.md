# Session - 7 | Cloning

### Methods
| Modifier and Type | Method and Description |
| ----------------- | ---------------------- |
| *protected* Object | **clone()** : creates an object and returns a copy of this object |
| boolean | **equals(Object obj)** : indicates whether some other object is "equal to" this one |
| *protected* void | **finalize()** : called by the garbage collector on an object when garbage collection determines that there are not more references to the object |
| Class<?> | **getClass()** : returns the runtime class of this Object |
| int | **hashCode()** : returns a hash code value for the object |
| void | **notify()** : wakes up a single thread that is waiting on this object's monitor |
| void | **notifyAll()** : wakes up all threads that are waiting on this object's monitor |
| String | **toString()** : returns a string representation of the object |
| void | **wait()** : causes the current thread to wait until another thread invokes the notify() method or the notifyAll() method for this object |
| void | **wait(long timeout)** : causes the current thread to wait until either another thread invokes the notify() method or the notifyAll() method for this object, or a specified amount of time has elapsed |
| void | **wait(long timeout, int nanos)** : causes the current thread to wait until another thread invokes the notify() method or the notifyAll() method for this object, or some other thread interrupts the current thread, or a certain amount of real time has elapsed. |

Clone is a copy of the object

#### Flat.java
```java
package com.psl.cloning;

public class Flat {
	private String hall = "Hall";
	private String bedRoom = "BR";
	private String commonParking = "Common Parking";
}
```

 - f1 is an Object of Flat owned by Person 1
 - f2 is an Object of Flat owned by Person 2
 - these two are different flats, and are copy of each other, diff instances
 - two diff instances should be created for f1 and f2
 - whereas common parking should be sharing same resources
 - both f1 and f2 will be pointing to the same object

This is Shallow Clone <br/>

**Shallow Clone** means *references are copied*.
    <img src = "../../../images/cj-shallow-clone.png">

**Deep Clone**, objects of F1 and F2 are Shallow Clone, whereas Playground is Deep Clone.
    <img src = "../../../images/cj-deep-clone.png">

- The default implementation of clone() is the Shallow Clone
    - because objects class does not know hierarchy of objects.

-----------------------------------------------

## Example of Shallow Clone
#### Name.java
```java
package com.psl.cloning;

public class Name {
	private String firstName;
	private String lastName;
	
	public Name(String firstName, String lastName) {
		super();
		this.firstName = firstName;
		this.lastName = lastName;
	}
	
	public String getFirstName() { return firstName; }
	public String getLastName() { return lastName; }

	public void setFirstName(String firstName) { this.firstName = firstName; }
	public void setLastName(String lastName) { this.lastName = lastName; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((firstName == null) ? 0 : firstName.hashCode());
		result = prime * result + ((lastName == null) ? 0 : lastName.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj) return true;
		if (obj == null) return false;
		if (getClass() != obj.getClass()) return false;
		
		Name other = (Name) obj;
		if (firstName == null) {
			if (other.firstName != null)
				return false;
		} else if (!firstName.equals(other.firstName))
			return false;
		if (lastName == null) {
			if (other.lastName != null)
				return false;
		} else if (!lastName.equals(other.lastName))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Name [firstName=");
		builder.append(firstName);
		builder.append(", lastName=");
		builder.append(lastName);
		builder.append("]");
		return builder.toString();
	}
}
```

#### Employee.java
```java
package com.psl.cloning;

public class Employee {
	//emp2 value is exactly copied by this.. in the copy()
	private Name name;

	public Employee(Name name) {
		super(); // it is inheriting from object's class
		//if we remove it, compiler is going to add it
		System.out.println("Constructor called");
		this.name = name;
	}
	//copy constructor method
	public Employee copy() {
		return new Employee(this.name);
	}
	
	public Name getName() { return name; }
	public void setName(Name name) { this.name = name; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj) return true;
		if (obj == null) return false;
		if (getClass() != obj.getClass()) return false;
		
		Employee other = (Employee) obj;
		
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Employee [name=");
		builder.append(name);
		builder.append("]");
		return builder.toString();
	}
}
```

#### TestClient.java
```java
package com.psl.cloning;

public class TestClient {
	public static void main(String[] args) {
		Name name = new Name("Shiv", "Singh");
		
		Employee emp1 = new Employee(name);
		System.out.println(emp1);
		//Employee emp2 = new Employee(name);
		Employee emp2 = emp1.copy();
		System.out.println(emp2);
		//chaning name of emp2
		emp2.getName().setFirstName("Shivam");
		
		System.out.println(emp1 == emp2); //false; emp1 and emp2 are diff instances
		System.out.println(emp1.equals(emp2)); //true
        //emp2 is a clone of emp1; different objects but are copy of each other
		System.out.println(emp1);
		System.out.println(emp2);
	}
}
```

#### Output
```
Constructor called 👈 emp1
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Constructor called 👈 emp2
Employee [name=Name [firstName=Shiv, lastName=Singh]]
false
true
Employee [name=Name [firstName=Shivam, lastName=Singh]]
Employee [name=Name [firstName=Shivam, lastName=Singh]]
```

- *We have not used a clone yet, we've used copy constructor and copy constructor is not a clone.*
- first both was Shiv, but later we changed the code, and both firstName are now Shivam whereas lastName remain as the prvs
- emp2 is copied by value
- another obj is algo going to point the same name
	<img src = "../../../images/cloning-1.png">

### Modification - 1 (Making it Clone)
- instead of sayinng `emp1.copy()` modifiying it by writing `emp1.clone()`
- `clone()` is a protected method.
	- to use this method, override it and make it public
#### Employee.java
```java
package com.psl.cloning;

public class Employee {
	//emp2 value is exactly copied by this.. in the copy()
	private Name name;

	public Employee(Name name) {
		super(); // it is inheriting from object's class
		//if we remove it, compiler is going to add it
		System.out.println("Constructor called");
		this.name = name;
	}

	public Employee copy() {
		return new Employee(this.name);
	}
	
	/* --- Clone Method --- */
	@Override
	//we cannot make the access modifier private or default of clone(); rule of inheritance method or overriding
	public Object clone() throws CloneNotSupportedException { //change the access modifier from protected to public
		//it is going to return clone of the object, but as a ref of obj class
		return super.clone(); //bcoz clone() is of obj class
	}
	
	public Name getName() { return name; }
	public void setName(Name name) { this.name = name; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Employee other = (Employee) obj;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Employee [name=");
		builder.append(name);
		builder.append("]");
		return builder.toString();
	}
}
```

#### TestClient.java
```java
package com.psl.cloning;

public class TestClient {
	public static void main(String[] args) {
		Name name = new Name("Shiv", "Singh");
		
		Employee emp1 = new Employee(name);
		Employee emp2 = null;

		try {
            //creating clone
			//emp1.clone() is a obj ref in the RHS
			//whereas in LHS is a class instance
			emp2 = (Employee)emp1.clone(); //typecasting to avoid mismatch and handling exception
		} catch (CloneNotSupportedException e) { //if the try block fails, it's going to throw exception
			e.printStackTrace();
		} 
		
		System.out.println(emp1);
		System.out.println(emp2);
		
		emp2.getName().setFirstName("Shivam");
		
		System.out.println(emp1);
		System.out.println(emp2);
	}
}
```
#### Output
```
Constructor called
java.lang.CloneNotSupportedException: com.psl.cloning.Employee
	at java.lang.Object.clone(Native Method)
	at com.psl.cloning.Employee.clone(Employee.java:23)
	at com.psl.cloning.TestClient.main(TestClient.java:10)
Employee [name=Name [firstName=Shiv, lastName=Singh]]
null
Exception in thread "main" java.lang.NullPointerException
	at com.psl.cloning.TestClient.main(TestClient.java:18)
```

- getting exception; CloneNotSupportedException and NullPointerException
- CloneNotSupportedException is a checked Exception
- emp2 has become null, as nothing was assigned to it.
- The reason we are getting the CloneNotSupportedException is because we haven't implemented Cloneable interface in our POJO class.
- why ClontNotFoundException ?
    - clone() requires a class to implement interface known as **Cloneable**
- it forces to implement Cloneable Interface, **Cloneable Interface** is the *Marker Interface*.
- **Marker Interface** : An interface withoud methods, fields and constands is known as marker or tagged interface. This type of an interface is an empty interface. It delivers runtime information about an object. This information is delivered to both the JVM and the compiler.
- Cloneable interface does not have the object clone but it comes from the object class
- if we do not implement Cloneable our clone() method does not work

### Modification - 2 (Shallow Clone)
Created clone of emp2
### Employee.java
```java
package com.psl.cloning;

public class Employee implements Cloneable {
	//emp2 value is exactly copied by this.. in the copy()
	private Name name;

	public Employee(Name name) {
		super(); // it is inheriting from object's class
		//if we remove it, compiler is going to add it
		System.out.println("Constructor called");
		this.name = name;
	}

	public Employee copy() {
		return new Employee(this.name);
	}
	
	//clone method
	@Override
	//we cannot make the access modifier private or default of clone(); rule of inheritance method or overriding
	public Object clone() throws CloneNotSupportedException { //change the access modifier from protected to public
		//it is going to return clone of object, but as a ref of obj class
		//always call super class clone()
		return super.clone();
		//DO NOT TRY BY CREATING A COPY BY CALLING A CONSTRUCTOR; THAT IS NOT CLONE👇
		// Object clonedEmp = super.clone() ❌

	}
	
	public Name getName() { return name; }
	public void setName(Name name) { this.name = name; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Employee other = (Employee) obj;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Employee [name=");
		builder.append(name);
		builder.append("]");
		return builder.toString();
	}
}

```

#### Name.java
```java
package com.psl.cloning;

public class Name {
	private String firstName;
	private String lastName;
	
	public Name(String firstName, String lastName) {
		super();
		this.firstName = firstName;
		this.lastName = lastName;
	}
	
	public String getFirstName() { return firstName; }
	public String getLastName() { return lastName; }

	public void setFirstName(String firstName) { this.firstName = firstName; }
	public void setLastName(String lastName) { this.lastName = lastName; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((firstName == null) ? 0 : firstName.hashCode());
		result = prime * result + ((lastName == null) ? 0 : lastName.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj) return true;
		if (obj == null) return false;
		if (getClass() != obj.getClass()) return false;
		
		Name other = (Name) obj;
		if (firstName == null) {
			if (other.firstName != null)
				return false;
		} else if (!firstName.equals(other.firstName))
			return false;
		if (lastName == null) {
			if (other.lastName != null)
				return false;
		} else if (!lastName.equals(other.lastName))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Name [firstName=");
		builder.append(firstName);
		builder.append(", lastName=");
		builder.append(lastName);
		builder.append("]");
		return builder.toString();
	}
}
```
#### Output
```
Constructor called
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shivam, lastName=Singh]]
Employee [name=Name [firstName=Shivam, lastName=Singh]]
```

- we should have got constructor called twice, but we got it called only once.
- Constructor is called only for creating new object; clone() does not calls contructor
- clone() creates a clone without calling the constructor of an object of a class
- but this clone is a *Shallow Clone* since it has changed the name of both the object (*Shiv* 👉 *Shivam*)
- Default implmenetation of clone() is always a *Shallow Clone*

## Example of Deep Clone
### Modification - 3 (Deep Clone)
- Now if i change emp1 name, emp2 name will not be changed.
- Remember, always call super classes clone(), do not try by creating a copy of the constructor

#### Name.java
```java
package com.psl.cloning;

public class Name implements Cloneable {
	private String firstName;
	private String lastName;
	
	public Name(String firstName, String lastName) {
		super();
		this.firstName = firstName;
		this.lastName = lastName;
	}
	
	@Override
	public Object clone() throws CloneNotSupportedException {
		return super.clone();
	}

	public String getFirstName() { return firstName; }
	public String getLastName() { return lastName; }

	public void setFirstName(String firstName) { this.firstName = firstName; }
	public void setLastName(String lastName) { this.lastName = lastName; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((firstName == null) ? 0 : firstName.hashCode());
		result = prime * result + ((lastName == null) ? 0 : lastName.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Name other = (Name) obj;
		if (firstName == null) {
			if (other.firstName != null)
				return false;
		} else if (!firstName.equals(other.firstName))
			return false;
		if (lastName == null) {
			if (other.lastName != null)
				return false;
		} else if (!lastName.equals(other.lastName))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Name [firstName=");
		builder.append(firstName);
		builder.append(", lastName=");
		builder.append(lastName);
		builder.append("]");
		return builder.toString();
	}
}
```

#### Employee.java
```java
package com.psl.cloning;

public class Employee implements Cloneable {
	//emp2 value is exactly copied by this.. in the copy()
	private Name name;

	public Employee(Name name) {
		super(); // it is inheriting from object's class
		//if we remove it, compiler is going to add it
		System.out.println("Constructor called");
		this.name = name;
	}

	public Employee copy() {
		return new Employee(this.name);
	}	
	//clone method
	@Override
	//we cannot make the access modifier private or default of clone(); rule of inheritance method or overriding
	public Object clone() throws CloneNotSupportedException { //change the access modifier from protected to public
		Object cloned = super.clone(); //is going to return emp's instance, this method is in the object class
        //making deep clone
		Employee clonedEmployee = (Employee)cloned; //typecasting
		
		clonedEmployee.setName((Name)this.getName().clone());
		return clonedEmployee;
	}
	
	public Name getName() { return name; }
	public void setName(Name name) { this.name = name; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Employee other = (Employee) obj;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Employee [name=");
		builder.append(name);
		builder.append("]");
		return builder.toString();
	}
}
```
#### Output
```
Constructor called
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shivam, lastName=Singh]]
```

After cloning, first object has retain it's name, where second has changes it's name.

### Modification - 4 (Deep Clone)
- Removing typecasting by modifying the previous code
- In a base class if it is return an object, then in the child class we can override and return by the Employee class; It's a valid overriding.

#### Name.java
```java
package com.psl.cloning;

public class Name implements Cloneable {
	private String firstName;
	private String lastName;
	
	public Name(String firstName, String lastName) {
		super();
		this.firstName = firstName;
		this.lastName = lastName;
	}
	
	@Override
	public Name clone() throws CloneNotSupportedException {
		return (Name)super.clone(); //typecasting
	}

	public String getFirstName() { return firstName; }
	public String getLastName() { return lastName; }

	public void setFirstName(String firstName) { this.firstName = firstName; }
	public void setLastName(String lastName) { this.lastName = lastName; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((firstName == null) ? 0 : firstName.hashCode());
		result = prime * result + ((lastName == null) ? 0 : lastName.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Name other = (Name) obj;
		if (firstName == null) {
			if (other.firstName != null)
				return false;
		} else if (!firstName.equals(other.firstName))
			return false;
		if (lastName == null) {
			if (other.lastName != null)
				return false;
		} else if (!lastName.equals(other.lastName))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Name [firstName=");
		builder.append(firstName);
		builder.append(", lastName=");
		builder.append(lastName);
		builder.append("]");
		return builder.toString();
	}
}
```

#### Employee.java
```java
package com.psl.cloning;

public class Employee implements Cloneable {
	//emp2 value is exactly copied by this.. in the copy()
	private Name name;

	public Employee(Name name) {
		super(); // it is inheriting from object's class
		//if we remove it, compiler is going to add it
		System.out.println("Constructor called");
		this.name = name;
	}

	public Employee copy() {
		return new Employee(this.name);
	}
	
	@Override
	public Employee clone() throws CloneNotSupportedException { //valid overriding; child class
		//Shallow Clone
		//never create a new Employee obj here 👇
		Object cloned = super.clone();
		//Making it Deep Clone👇
		Employee clonedEmployee = (Employee)cloned; 
		clonedEmployee.setName(this.getName().clone());
		return clonedEmployee;
	}
	
	public Name getName() { return name; }
	public void setName(Name name) { this.name = name; }

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Employee other = (Employee) obj;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Employee [name=");
		builder.append(name);
		builder.append("]");
		return builder.toString();
	}
}
```

#### TestClient.java
```java
package com.psl.cloning;

public class TestClient {
	public static void main(String[] args) {
		Name name = new Name("Shiv", "Singh");
		
		Employee emp1 = new Employee(name);
		Employee emp2 = null;
		try {
			emp2 = emp1.clone(); //now, no need to tyepcast it; bcoz internally we have typecased it.
		} catch (CloneNotSupportedException e) { //if the try block fails, it;s going to throw exception
			e.printStackTrace();
		} 
		
		System.out.println(emp1);
		System.out.println(emp2);
		
		emp2.getName().setFirstName("Shivam");
		
		System.out.println(emp1);
		System.out.println(emp2);
	}
}
```

#### Output
```
Constructor called
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shiv, lastName=Singh]]
Employee [name=Name [firstName=Shivam, lastName=Singh]]
```
