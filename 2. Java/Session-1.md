# Session - 1 | Introduction to Java

## Introduction
- Java is Object Oriented Programming based language.
- We cannot write any code outside the class
- Object is the instance of the class
- Class is the template to create object
- Class is the smallest unit which can be compiled
- Java is a *higher-level* language
- **JVM** is NOT platform independent
- **Java** is platform independent
- WORA - Write Once Run Anywhere, How it happens?
	- It means that Java code can be compiled into bytecode, which is a platform-independent format. 
	- This bytecode can then be run on any device that has a Java Virtual Machine (JVM) installed.
- JVM: When the java programs runs, it translates it into a native code
- Java doesn't create .exe files, Java on the fly starts running on the JVM
- You cannot write anything outside class
- Program starts from the main method
- JRE is for just running the program
- JDK is for actual developing the program
- JDK is superset of JRE, JDK also contains JRE
- The program starts from the **main** method
- Who runs the program? 👉 JVM
- If we want the main method accessible to be JVM, it must be **public**
- Anything which is static is associated with the class, and it doesn't need object or instance of a class
- By declaring it **static**, we can access the complexcalculator
	- Because static requires only classname not its object
- `void` because in main method it is not going to return anything, as JVM doesn't expect anything to be returned
- Never terminate a program with a -ve value
- `main` is just a name, just like C and C++
- Alternate way of writing `String[] args` is **String ...args**
- JDK is superset of JRE

----------------------
- First import the Scanner before using it.
- We should always have a named package, not in a default package
- Package name should always be in the first line
- Next line shows the package from the other classes
- Naming of package: *reverseofthe url*
    - Example: com.psl.complex
- Move it using the refactor, option
- Delimeter means it will break it into pieces
- `//` this is a comment in java
- System.in means, input, output
- Class can be marked its visibilty ONLY as default or public, otherwise it'll not be accesible.
- ClassName must match with the fileName, if it is declared public
- If not declarded public, filename may vary
- Always use one file one class 
- If there are two classes, which one it will take?
    - In one file you cannot have two public classes
- import.java.util.* means every package which is their in the util package
    - never use * for the best practices
- Red square means prvs run is still going on
- BufferedReader reads a full line at a time
- Scanner s is a refenece type variable
- int rPart1 is a primitive type variable
- Variables which are declared inside the method --> Local Variable
	```java
		String input = "1 fish 2 fish red fish blue fish";
		Scanner s = new Scanner(input).useDelimiter("\\s*fish\\s*");
		System.out.println(s.nextInt());
		System.out.println(s.nextInt());
		System.out.println(s.next());
		System.out.println(s.next());
		s.close();
	```

--------------------------------------------------------------
- If **pvsm method** is not set to public, JVM will not be able to access it.
- What is required to run a program? 👉 JRE
- What is required to develop a program? 👉 JDK
- Anything which is `static`, can be accessed by the class name, and it does not requires instance
- `static` is associated with the class, not the object
- `void` because main method is not going to return anything
- Never return a -ve value
- `main` is just a name, as it used in such as C++, *int main...*
- `String[] args` JVM can pass values in to the given argument, which can be read inside the main method
- Alternate way of passing; pvsm `(String ...args)` 
- Read the parameters from the terminal; console
- To read, we require Scanner class
- We should never have any class in a default package
- We should always have a package which is properly named
- Package naming convention: reverse of the url
- The package name, should always be the first line in the code
- Delimeter, means it'll break it into pieces
- Class name must match with the filename
- If not written public, then class name might vary
- Either we can write public or not write anything at all
- Always use 1 file 1 class
- If there are two classes, which name should be given?
    - In one file there cannot be two public classes,
- we cannot s.close() in between
	```java
	package com.psl.complex;
	import java.util.Scanner;

	public class ComplexCalculator {
		public static void main(String[] args) {
			String input = "1 fish 2 fish red fish blue fish";
			java.util.Scanner s = new java.util.Scanner(System.in).useDelimiter("\\s*fish\\s*");
			//Scanner sc = new Scanner(System.in);
			System.out.println(s.nextInt());
			System.out.println(s.nextInt());
			System.out.println(s.next());
			System.out.println(s.next());
			s.close();
		}
	}
	```
- We are doing too many things, too many times
- Code reuse principle is violated

### Code for Complex Calculator
#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {
		//input: 20,10,-15,5,
		//use Delimeter; if the input is a normal nd not from array
		Scanner sc = new Scanner(System.in).useDelimiter(",");
		System.out.println("Enter the number: ");
		int realPart1 = sc.nextInt();
		int imagPart1 = sc.nextInt();
		
		int realPart2 = sc.nextInt();
		int imagPart2 = sc.nextInt();
		
		sc.close();
		
		System.out.println(realPart1 + " + i" + imagPart1);
		System.out.println(realPart2 + " + i" + imagPart2);
		
		int ansReal = realPart1 + realPart2;
		int ansImag = imagPart1 + imagPart2;
		
		System.out.println(ansReal + " + i" + ansImag);
	}
}
```

- Return type of *asStringComplexNumbers()* will be String
- Concatenating strings toegether and storing it in ans
- Reusable code is written
- This program is not called on instance, this progeam is called on class name
- `public String asStringComplex()` can ONLY be called on instance methods,
    - so making it static, `public static String asStringComplex()`
- From static methods, you can only call static methods/variables
- If i make an instance of the main class, then we can access it, otherwise we can't

### Modification Version - 1
#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in).useDelimiter(",");
		System.out.println("Enter the number: "); //input: 20,10,-15,5,
		
		int realPart1 = sc.nextInt();
		int imagPart1 = sc.nextInt();
		
		int realPart2 = sc.nextInt();
		int imagPart2 = sc.nextInt();
		
		sc.close();
		// method call: asStringComplexNumbers
		System.out.println(asStringComplexNumbers(realPart1, imagPart1));
		System.out.println(asStringComplexNumbers(realPart2, imagPart2));
		
		int ansReal = realPart1 + realPart2;
		int ansImag = imagPart1 + imagPart2;
		
		System.out.println(asStringComplexNumbers(ansReal, ansImag));
	}
	
	// method definition: asStringComplexNumbers
	public static String asStringComplexNumbers(int realPart, int imagPart) {
		String ans = realPart + " + i " + imagPart;
		return ans;
	}
}
```

- We have not created instances of classes, we have just use a method, this is not complete Object Oriented
- This is more of a procedural program
- Java languages is not suitable for procedural program
- Instance variables, we cannot access in the static method
- Whenever we access an instance variable, we must call it with `this` keyword
- Whenever their is an ambuguity local one is always prefered

### Modification Version - 2
#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in).useDelimiter(",");
		System.out.println("Enter the number: "); //input: 20,10,-15,5,
				
		int realPart1 = sc.nextInt();
		int imagPart1 = sc.nextInt();
		
		int realPart2 = sc.nextInt();
		int imagPart2 = sc.nextInt();
		
		Complex c1 = new Complex(realPart1, imagPart1);
		Complex c2 = new Complex(realPart2, imagPart2);
		
		String c1AsString = asStringComplexNumber(c1);
		System.out.println(c1AsString);
		System.out.println(asStringComplexNumber(c2));
	}
	
	public static String asStringComplexNumber(Complex complex) {
		String ans = complex.realPart + " + i" + complex.imagPart;
		return ans;
	}
	// this method is not using any object oriented feature, more of a primitive type; procedural
	public static String asStringComplexNumbers(int realPart, int imagPart) {
		String ans = realPart + " + i " + imagPart;
		return ans;
	}
}
```

### Modficitation Version - 3
#### Complex.java
```java
public class Complex {
	public int realPart;
	public int imagPart;
	//constructor
	public Complex(int realPart, int imagPart) {
		this.realPart = realPart;
		this.imagPart = imagPart;
	}
}
```

#### ComplexCalculator.java
```java
public class ComplexCalculator {
	public static void main(String[] args) {

		Scanner sc = new Scanner(System.in).useDelimiter(",");
		System.out.println("Enter the number: "); //input: 20,10,-15,5,
				
		int realPart1 = sc.nextInt();
		int imagPart1 = sc.nextInt();
		
		int realPart2 = sc.nextInt();
		int imagPart2 = sc.nextInt();
		
		Complex c1 = new Complex(realPart1, imagPart1);
		Complex c2 = new Complex(realPart2, imagPart2);
		
		String c1AsString = asStringComplexNumber(c1);
		System.out.println(c1AsString);
		System.out.println(asStringComplexNumber(c2));
		
		String ans = addComplexNumbers(c1, c2);
		System.out.println(ans);
	}
	
	public static String asStringComplexNumber(Complex complex) {
		String ans = complex.realPart + " + i" + complex.imagPart;
		return ans;
	}
	// method definition of addComplexNumbers which adds and gives the resultant
	public static String addComplexNumbers(Complex first, Complex other) {
		String ans = (first.realPart + other.realPart) + " + j " + (first.imagPart + other.imagPart);
		return ans;
	}
}
```