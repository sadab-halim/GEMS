# Session - 1 | Introduction to Spring

- Connection with Core Java Code
- Why do we need Server, Spring?
	- Readability
- Window -> Preferences -> General -> Appearance -> Color and Font -> Basic -> Verdana, 12
- New -> Java Project -> SprintIntro
- Preferences -> Java -> Compiler -> Make sure it is 1.8
- POJO is a simple Java Class; Java Bean
- Never keep anything into a default package
- Every attribute always should be private

#### Engine.java
```java
package com.psl.intro.entity;

public class Engine {
    private double power;
    private int cylinder;
    //java bean is a standard convention; it says, u must have a constructor to it.
    //java beans dowes not believe in parametrized constructor
	//bcoz many times it may  not have parameters to instantiate the objects 

    //creating another constructor; constructor without parameters (default constructor)
    public Engine() {
        System.out.println("Inside Engine()")
    }
    //parametrized constructor; since it has a parameterized constructor
	//java compiler will not add, any other other (default) constructor to it
    public Enginge() {
        super();
        System.out.println("Inside Engine(parameters)");
        this.power = power;
        this.cyliner = cylinder;
    }
    //java beans also says that, create getters and setters for every attributes
    public double getPower() { return power; }
    public int getCylinder() { return cylinder; }

    public void setPower(double power) { this.power = power; }
    public void setCylinder(int cylinder) { this.cylinder = cylinder; }

    //for our convenienence adding toString methods
    @Override
    public String toString() {
        StringBuilder builder = new StringBuilder();
        builder.append(power);
        builder.append(",");
        builder.append(cylinder);
        return builder.toString();
    }
}
```

#### Vehicle.java
```java
package com.psl.intro.entity;

public class Vehicle {
    private String rtoNumber;
    private int capacity;
    private Enginge engine;//Relationship; reference type of an object
    //default constructor
    public Vehicle() {
		//this.engine = new Engine(82.5, 6);
	}
    //parameterized constructor
    public Vehicle(String rtoNumber, int capacity, Engine engine) {
        super();
        this.rtoNumber = rtoNumber;
        this.capacity = capacity;
        this.engine = engine;
    }
    //getter methods
    public String getRtoNumber() { return rtoNumber; }
    public int getCapacity() { return capacity; }
    public Engine getEngine() { return engine; }
    //setter methods
    public void setRtoNumber(String rtoNumber) { this.rtoNumber = rtoNumber; }
    public void setCapacity(int capacity) { this.capacity = capacity; }
    public void setEngine(Engine engine) { this.engine = engine; }
    
    //toString() methods
    @Override
    public String toString() {
        StringBuilder builder = new StringBuilder();
        builder.append("RTO Number:");
        builder.append(rtoNumber);
        builder.append("\nCapacity:");
        builder.append(capacity);
        builder.append("\nEngine:");
        builder.append(engine);

        return builder.toString();
    }
}
```
#### TestClient.java
```java
package com.psl.intro.client;

import com.psl.intro.entity.Engine;
import com.psl.intro.entity.Vehicle;

public class TestClient {
    public static void main(String[] args) {
        //1 way: creating an object
        Engine e1 = new Engine();
        System.out.println(e1); //it will call effectively, toString()
		//System.out.println(e1.toString()) ☝️ this is equivalent to the above code
		//prints: Engine [power=0.0, cylinder=0.0]

        //2 way: passing input using setter methods
        e1.setPower(62.5);
        e1.setCylinder(4);

        //passing the values as arguments
        Engine e1 = new Enginer(32.5, 3);
        //creating dependency
        Vehicle v1 = new Vehicle();
        v1.setCapacity(4);
        v1.setRtoNumber("WB 12A 4950");
        v1.setEngine(e1); //same as 👉 v1.setEngine(new Engine(62.5, 6));
        System.out.println(v1);
		/* Output
		   Engine [power=62.5, cylinder=6]
		   Vehicle[rtoNumber=MH 56 AB 1111, capcaity=5, engine=Engine [power=62.5, cylinder=6 ]]
		*/
    }
}
```

- Problem: Whenever other people would like to use vehicle, they would also need to use Engine also, and we need to always call **new**. Hence, dependent.. (vehicle depends on a Engine)

- *Instead of completely manufacturing things, and taking care of all the manufacturing spare parts*
	- *delicate this activity to someone else, and you just go on doing the assembly of it; this is the main idea behind making it a spring.*

- Beans are called as container managed Beans.

- New -> Maven Project -> maven-archetype-quickstart
- GroupId: com.psl
- ArtifactId: spring.a.di
- this gives standard java structure
- inside pom file add the necessary dependencies
- and make <maven.compiler.source>1.8</maven.compiler.source>

## Creating Spring Project
- Open STS 
- Create Maven Project
    - Select Archetype
        - Filter on maven-archetype-quickstart
        - Select GroupId = "org.apache.maven.archetypes"
        - Select ArtifactId = "maven-archetype-quickstart"

    - Provide GroupId : "com.psl"
    - Provide ArtifactId : "spring.a.di"

- This gives standard Java structure

- In case you face error, clear the Maven repository
    - Delete the contents of C:\Users\<username>\.m2\repository
- Add Spring Context Dependency
    - In Browser, Go to mvnrepository.com
    - Search for "Spring Context"
    - Select Spring Context Dependency 5.x.y (e.g. 5.3.25)
    - Copy the XML Snippet at the bottom and paste inside project 👉 pom.xml within <dependencies></dependencis>
    
- Add Spring Core Dependency
    - In Browser, Go to mvnrepository.com
    - Search for "Spring Core"
    - Select Spring Core Dependency 5.x.y (e.g. 5.3.25)
    - Copy the XML Snippet at the bottom and paste inside project 👉 pom.xml within <dependencies></dependencis>

- Add Java Compatibility to Java-8 to pom.xml properties.
    ```xml
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8<maven.compiler.target>
    </properties>
    ```
- Create "resources" folder inside src/main folder. [Maven treats all files inside resources as resource files]
- Right click on project 👉 Maven 👉 Update Project
- Select Project 👉 Properties 👉 Java 👉 JRE 👉 Confirm Java-8
- Right click src/main/resources 👉 New 👉 Spring Bean Configuration File
- Create beanconfig.xml
- If you don't see option for it:
    - Eclipse 👉 Help 👉 Market Place 👉 Search "Spring Tool 3 Add-on for Spring Tool 4" 👉 Apply Available Update
    - Right Click Project 👉 Maven 👉 Update Project
    - If it fails, you need to create xml file and paste sample header contents in beanconfig.xml

- This creates new Spring Project

## Creating and Configuring Beans in Spring Project:
- Spring Project Contains
    - IOC Container
	- Configuration
	- Beans

- Configuration can be done through
    - XML (bean configuration file)
	- Annotations
	- via Java Class

- Create following Java Beans inside src/main/java/*base-package*.entity
    - com.psl.spring.a.di.entity.Vehicle *(rtoNumber, capacity)*
    - com.psl.spring.a.di.entity.Engine *(power, cylinders)*

- class name should be fully qualified
### Project Structure

#### Engine.java 
*Same as previous*

#### Vehicle.java
*Samr as previous*

#### beanconfig.xml (Constructor Injection)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:aop="http://www.springframework.org/schema/aop"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:jee="http://www.springframework.org/schema/jee"
		xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/jee
		http://www.springframework.org/schema/jee/spring-jee-4.3.xsd">
	
	<!-- Bean of Type: Engine -->
	<bean id="powerEngine" class="com.psl.spring.a.di.entity.Engine">
		<constructor-arg name="power" value="92.5" />
		<constructor-arg name="cylinder" value="4" />
	</bean>
	
	<bean id="commonEngine" class="com.psl.spring.a.di.entity.Engine">
		<constructor-arg name="power" value="72.5" />
		<constructor-arg name="cylinder" value="4" />
	</bean>
	
	<!-- Beans of Type: Vehicle -->
	<bean id="officePickup" class="com.psl.spring.a.di.entity.Vehicle">
        <!-- bean: powerEngine is ref for the below 👇 -->
		<constructor-arg ref = "powerEngine"></constructor-arg> <!-- dependendent bean-->
		<constructor-arg name= "rtoNumber" value="MH56 AB 1111" />
		<constructor-arg name= "capacity" value = "6" />
	</bean>
	
	<bean id="commonPickup" class="com.psl.spring.a.di.entity.Vehicle">
		<constructor-arg ref = "commonEngine"></constructor-arg>
		<constructor-arg name="rtoNumber" value="MH56 AB 2222" />
		<constructor-arg name= "capacity" value = "17" />
	</bean>
</beans>
```

- for getting a bean 👉 `ClassPathXmlApplicationContext()` requires a file name which needs to be configured.
- `context` is a container for us
- ClassPathXmlApplicationContext is implementation of BeanFactory
- this context is providing a support entry to create the ApplicationContext object
- `getBean()` provides the object to us, and we are expecting to return a conctrete class, so it is a downcasting
    - Hence, we need to explicitly typecaste it
		```java
		Engine engine = (Engine)context.getBean("powerEngine")
		```
- Another and better way of typecasting, to avoid errors 👇
    ```java
    Engine engine = context.getBean("powerEngine", Engine.class);
    ```

#### App.java
```java
package com.psl.spring.a.didemo;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.psl.spring.a.didemo.entity.Engine;
import com.psl.spring.a.didemo.entity.Vehicle;

public class App {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beanconfig.xml");
        System.out.println("----- Engine -----");
        Engine powerEngine = context.getBean("powerEngine", Engine.class);
        System.out.println(powerEngine);
        
        System.out.println("------------------------");
        
        Engine commonEngine = context.getBean("commonEngine", Engine.class);
        System.out.println(commonEngine);
        
        System.out.println("\n----- Vehicle -----");
        Vehicle officePickup = context.getBean("officePickup", Vehicle.class);
        System.out.println(officePickup);
        
        System.out.println("------------------------");
        
        Vehicle commonPickup = context.getBean("commonPickup", Vehicle.class);
        System.out.println(commonPickup); 
    } 
}
```

#### Output
```
Inside Engine(parameters)
Inside Engine(parameters)
----- Engine -----
Engine [power=92.5, cylinders=4]
------------------------
Engine [power=72.5, cylinders=4]

----- Vehicle -----
Vehicle [rtoNumber=MH56 AB 1111, capcaity=6, engine=Engine [power=92.5, cylinders=4]]
------------------------
Vehicle [rtoNumber=WB CD 5555, capcaity=12, engine=Engine [power=72.5, cylinders=4]]
```

-----------------------------------------------------------------------

#### beanconfigsetter.xml (Setter Injection)
```xml
	<bean id = "powerEngineUsingSI" class = "com.psl.spring.a.di.entity.Engine">
		<property name="power" value="84.5"/>
		<property name="cylinder" value="3"/>
	</bean>

	<bean id = "commonEngineUsingSI" class = "com.psl.spring.a.di.entity.Engine">
		<property name="power" value="95.5"/>
		<property name="cylinder" value="6"/>
	</bean>
```

#### App.java
```java
package com.psl.spring.a.didemo;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.psl.spring.a.didemo.entity.Engine;
import com.psl.spring.a.didemo.entity.Vehicle;

public class App {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beanconfigsetter.xml");
        
        System.out.println("----- Engine -----");
        Engine powerEngine = context.getBean("powerEngineUsingSI", Engine.class);
        System.out.println(powerEngine);
        
        System.out.println("------------------------");
        
        Engine commonEngine = context.getBean("commonEngineUsingSI", Engine.class);
        System.out.println(commonEngine);

        Vehicle officePickup = context.getBean("officePickup", Vehicle.class);
    }
}
```
#### Output
```
Inside Engine()
Inside setPower()
Inside setCylinder()
Inside Engine()
Inside setPower()
Inside setCylinder()
Engine [power=92.5, cylinders=4]
Engine [power=72.5, cylinders=4]
Vehicle [rtoNumber=MH56 AB 1111, capacity=6, engine=Engine [power=92.5, cylinders=4]]
```

 