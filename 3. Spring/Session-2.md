# Session-2

- If the are two parameters of the same type, then in that case, we can use name and type..

## Constructor Confusion
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
		
		<bean id = "token-0" class = "com.psl.spring.a.didemo.entity.Token">
			<constructor-arg value = "abc@pqr"/>
		</bean>
		<bean id = "token-1" class = "com.psl.spring.a.didemo.entity.Token">
			<constructor-arg value = "123"/>
		</bean>
		<bean id = "token-2" class = "com.psl.spring.a.didemo.entity.Token">
			<constructor-arg value = "23"/>
		</bean>
		<bean id = "token-3" class = "com.psl.spring.a.didemo.entity.Token">
			<constructor-arg value = "ab34"/>
		</bean>
</beans>
```


#### Token.java
```java
package com.psl.spring.a.di.entity;

public class Token {
	private String email;
	private long code;
	
	public Token(String input) {
		System.out.println("String constructor called..");
		email = input;
	}
	
	public Token(int input) {
		System.out.println("int constructor called..");
		code = input;
	}
	
	public String getEmail() { return email; }
	public long getCode() { return code; }
	
	public void setEmail(String email) { this.email = email; }
	public void setCode(long code) { this.code = code; }
	
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Email: ");
		builder.append(email);
		builder.append("\nCode: ");
		builder.append(code);
		return builder.toString();
	}
}

```

#### App.java
```java
package com.psl.spring.a.di;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.psl.spring.a.didemo.entity.Token;

public class App_Constr_Conf {
	public static void main(String[] args) {
		try (ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beanconfig_constr_conf.xml")) {
			Token token0 = context.getBean("token-0", Token.class);
			Token token1 = context.getBean("token-1", Token.class);
			Token token2 = context.getBean("token-2", Token.class);
			Token token3 = context.getBean("token-3", Token.class);
			
			System.out.println(token0);
			System.out.println(token1);
			System.out.println(token2);
			System.out.println(token3);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```
#### Output
<img src = "../../../images/spring-01.png">

- Always a String Constructor is being called. 

- Even though we have overloaded constructor, still the String constructor is being called.
- `value` is not understanding what type of data type it is
- Default type of value is "String"
- **Rule:** string >>>> any-other-datatype
- It doesn't maintains a sequence
- XML always passess values as string, and then it is converted

### To Avoid Confusion: Mention Type
```xml
<bean id = "token-0" class = "com.psl.spring.a.di.entity.Token">
	<constructor-arg value = "abc@pqr"/>
</bean>
<bean id = "token-1" class = "com.psl.spring.a.di.entity.Token">
	<constructor-arg value = "123" type = "int" />
</bean>
<bean id = "token-2" class = "com.psl.spring.a.di.entity.Token">
	<constructor-arg value = "23" type="int"/>
</bean>
<bean id = "token-3" class = "com.psl.spring.a.di.entity.Token">
	<constructor-arg value = "ab34"/>
</bean>
```

#### Output
```
String constructor called
int contructor called
int constructor called
String constructor called
Email: abc@pqr
Code: 0
Email: null
Code: 123
Email: null
Code: 23
Email: ab34
Code: 0
```

---------------------------------

## Inner Bean
- Inner Beans are the Beans Inside the Beans, similar as Inner Class
- It is an instance
- instead of using a `ref`, it is describing a bean inside it
- This kind of bean resonates the principle of *Encapsulation* and *Data Hiding*.
- It describes the *relationship of composition*.
- Bean Visible Outside describes the *relationship of association, aggregation*

#### beanconfig_innerbean_inj.xml
```xml
<!-- Bean Visible Outside -->
	<bean id = "powerEngine" class = "com.psl.spring.a.di.entity.Engine">
		<property name = "power" value = "92.5" />
		<property name = "cylinder"> 
			<value>4</value>
		</property>
	</bean>
		
	<bean id = "officePickup" class = "com.psl.spring.a.di.entity.Vehicle">
		<property name = "engine">
			<!-- Bean Invisible Outside; Inner Bean -->
			<!-- instead of using a ref, it is describing a bean inside it -->
			<bean id = "economyEngine" class = "com.psl.spring.a.di.entity.Engine">
				<property name = "power" value = "42.5"/>
				<property name = "cylinder" value = "2"/>					
			</bean>
		</property>
			
		<property name = "rtoNumber" value = "MH56 AA 5555"/>
		<property name = "capacity" value = "4"/>
	</bean>
```

#### App_InnerBean_Inj.java
```java
package com.psl.spring.a.di;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.psl.spring.a.di.entity.Engine;
import com.psl.spring.a.di.entity.Vehicle;

public class App_Inner_Bean {
	public static void main(String[] args) {
		try (ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beanconfig_inner_bean.xml")) {

			Engine powerEngine = context.getBean("powerEngine", Engine.class);
			System.out.println("Power Engine : " + powerEngine);

			Vehicle officeVehicle = context.getBean("officePickup", Vehicle.class);
			System.out.println("Office Pickup with Inner Bean : " + officeVehicle);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### Output
```
Power Engine : Engine [power=92.5, cylinders=4]
Office Pickup with Inner Bean : Vehicle [rtoNumber=MH56 AB 5555, capacity=4, engine=Engine [power=42.5, cylinders=2]]
```

### Trying to access instance of inner bean, will throw an exception

#### App_InnerBean_Inj.java
```java
package com.psl.spring.a.di;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App_InnerBean_Inj {
	public static void main(String[] args) {
		try (ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beanconfig_innerbean_inj.xml")) {
			//Bean Invisible From Outside	
			Engine economyEngine = context.getBean("economyEngine", Engine.class);
			System.out.println("Economy Engine : " + economyEngine);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### Output
```
Error at org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'economyEngine' available
```

------------------------------------

## Using P and C Namespaces
Don't forget to include these..
```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```
### P Namespace

```xml
	<?xml version = "1.0" encoding = "UTF-8"?>
	<beans xmlns = "http://www.springframework.org/schema/beans"
		xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
		xmlns:context = "http://www.springframework.org/schema/context"
		xmlns:p="http://www.springframework.org/schema/p"
		.
		.
		.>
		<!-- Engine Bean Instance - 1; using p namespace; setter injection (property) -->
		<bean id = "powerEngine" class = "com.psl.spring.a.didemo.entity.Engine"
			p:power = "93.5" p:cylinder = "6"/>
		<!-- Vehicle Bean Instance - 1 with ref -->
		<bean id = "officialPickup" class = "com.psl.spring.a.didemo.entity.Vehicle"
			p:engine-ref = "powerEngine" p:rtoNumber = "MH56 AB 1111" p:capacity = "12"/>
	</beans>
```

### C Namespace
```xml
<?xml version = "1.0" encoding = "UTF-8"?>
	<beans xmlns = "http://www.springframework.org/schema/beans"
		xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
		xmlns:context = "http://www.springframework.org/schema/context"
		xmlns:c="http://www.springframework.org/schema/c"
		.
		.
		.>
		<!-- Engine Bean Instance-1; using c namespace; constructor injection (constructor-arg) -->
		<bean id = "powerEngine" class = "com.psl.spring.a.didemo.entity.Engine"
			c:power = "93.5" c:cylinder = "6"/>
		<!-- Vehicle Bean Instance-1 with ref -->
		<bean id = "officialPickup" class = "com.psl.spring.a.didemo.entity.Vehicle"
			c:engine-ref = "powerEngine" c:rtoNumber = "MH56 AB 1111" c:capacity = "12"/>
	</beans>
```

------------------------------------

## Bean Life Cycle
- Bean is an instance which we are not creating by calling the keyword `new`
- Whereas what we are doing is we are asking Spring Framework to give an instance that is called as a **managed bean**
- If at all I want to do some initialization, when the bean is created, Spring provides us with two callback methods:
	- After Create
	- Before Destroy

- There are two ways of creating the callback method:
	- One is the interface which is described by the Spring framework
		- and we are supposed to implement that interface, and when we implement that interface, we have to avoid some methods known as callback methods; but this way is discouraged, since tight coupling takes place.
	- Using XML Congiguration

##### beanconfig_lifecycle.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
		xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
		xmlns:context = "http://www.springframework.org/schema/context"
		xmlns:c="http://www.springframework.org/schema/c"
		xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans/xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd"
		
		default-init-method="init" default-destroy-method="destroy">
		<!-- 👆 this will work for all the methods in the bean -->
		
		<bean id = "lookupTable" class = "com.psl.spring.a.didemo.entity.Lookup"
			p:email="email"
			init-method="init" destroy-method="cleanup"/>
			<!-- 👆 overloading the destroy method-->
	</beans>
```

##### Lookup.java
```java
package com.psl.spring.a.di.entity;

public class Lookup {
	private String email;

	public Lookup() {}

	public Lookup(String email) {
		super();
		this.email = email;
	}

	public void intit(){
		System.out.println("init() called");
	}
	public void cleanup() {
		System.out.println("cleanup() called");
	}

	public String getEmail() { return email; }
	public void setEmail(String email) { this.email = email; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Lookup ( Email: ");
		builder.append(email);
		builder.append(" )");
		return builder.toString();
	}
}
```

##### App_LifeCycle_Methods.java
```java
package com.psl.spring.a.di;

import org.springframework.org.context.support.ClassPathXmlApplicationContext;
import com.psl.spring.a.di.entity.Lookup;

public class App_LifeCycle_Methods {
	public static void main(String[] args) {
		try (ClassPathXmlApplicationContext = new ClassPathXmlApplicationContext("beanconfig_lifecycle.xml")) {
			Lookup lookup = context.getBean("lookupTable", Lookup.class);
			System.out.println("lookup : " + lookup);
			lookup = null;
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### Output
```
init() called
lookup : Lookup ( Email: email )
cleanup() called
```

- If we remove the overriden methods, and call the defalt one, which is specified in the xml file, then we'll get the output like this: 
	- cleanup() method is not called, since the method name is different from the default method name: cleanup()
	- but if we create another method 👉 destroy() then destroy will be called.
- and if both are present, default and overruled one, then overruled will be called, will be given the priority

#### Output
```
init() called
lookup : Lookup ( Email: email )
```

#### App_LifeCycle_Methods.java
```java
package com.psl.spring.a.di;

import org.springframework.org.context.support.ClassPathXmlApplicationContext;
import com.psl.spring.a.di.entity.Lookup;

public class App_LifeCycle_Methods {
	public static void main(String[] args) {
		try (ClassPathXmlApplicationContext = new ClassPathXmlApplicationContext("beanconfig_lifecycle.xml")) {

			Lookup lookup1 = context.getBean("lookupTable", Lookup.class);
			Lookup lookup2 = context.getBean("lookupTable", Lookup.class);
			
			System.out.println("lookup : " + (lookup1 == lookup2));
			System.out.println("lookup : " + lookup1.equals(lookup2));
			System.out.println("lookup1 : " + lookup1.hashCode());	
			System.out.println("lookup2 : " + lookup2.hashCode());	
		} catch (Exception e) {
			e.printStackTrace();
		}
 	}
}
```

#### Output
```
init()
lookup : true
lookup : true
lookup1: 12345564
lookup2: 12345564
```

- Only one object is created, there are two aliases for the same object.
- lookup1 and lookup2 both are pointing to the same object.
- So, even if we are calling *n* no of times of same type of bean, it is providing us with the same bean.
- This is called **Singleton** Single Instance across the applications.
- **Singleton** is also a design pattern and it is set as default when a bean is created.
- If it is shareable then no prob, it is not going to change its properties.
- But, if it is non-shareable, then we may need a prototype and in that case, we may need to give *scope as a prototype*, in the bean description itself.
- If we want to create more no of objects for that bean and we want them to be *separate*, then we choose `prototype`
- If the objects are immutable then we can share them easily.
- All the beans which we have creaed without scope = protoype as attribute were all singleton.


#### beanconfig_autowiring.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
		xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
		xmlns:context = "http://www.springframework.org/schema/context"
		xmlns:c="http://www.springframework.org/schema/c"
		xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans/xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd"
		
		default-init-method="init" default-destroy-method="destroy">
		<!-- 👆 this will work for all the methods in the bean -->
		
		<bean id = "lookupTable" class = "com.psl.spring.a.didemo.entity.Lookup"
			p:email="email"
			init-method="init" destroy-method="cleanup"/>
			<!-- 👆 overloading the destroy method-->
	</beans>
```

-----------------------------------------

## Autowiring
- Autowiring is a feature of a Spring where the dependencies are automatically created and connected.
- Autowiring assigns properties to bean using matching options:
	- **byName** : wire each property to bean with the same name
	- **byType** : each property on target bean wired with bean of same type
	- **constructor** : like byType, uses constructor injection, tries to match greatest number of arguments.
	- **default** : chooses between constructor and byType automatically
	- **no** :  default

#### Email.java
```java
package com.psl.spring.a.di.entity;

public class Email {
	private String address;

	public Email() {}
	public Email(String address) {
		super();
		this.address = address;
	}

	public void init() {
		System.out.println("Email.init()");
	}

	public void destroy() throws Execption {
		System.out.println("Email.destroy()");
	}

	public String getAddress() { return address; }
	public void setAddress(String address) { this.address = address; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Email [address = ");
		builder.append(address);
		builder.append("]");

		return builder.toString();
	}
}
```

#### Customer.java
```java
package com.psl.spring.a.di.entity;

public class Customer {
	private Email officeEmail;
	private Email perEmail;
	private MobileNumber officeMobile;

	public Customer() {
		System.out.println("Customer() called");
	}

	public Customer(Email email) {
		this.perEmail = email;
		System.out.println("Customer(Email) called");
	}

	public Customer(Email email, MobileNumber officeMobile) {
		this.perEmail = email;
		this.officeMobile = officeMobile;
	}

	public getOfficeEmail() { return officeEmail; }
	public getPerEmail() { return perEmail; }
	public getOfficeMobile() { return officeMobile; }

	public void setOfficeEmail(Email officeEmail) { 
		this.officeEmail = officeEmail;
	}

	public void setPerEmail(Email perEmail) {
		this.perEmail = perEmail;
	}

	public void setofficeMobile(MobileNumber officeMobile) {
		this.officeMobile = officeMobile;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Customer ( ");
		builder.append("officeEmail: ");
		builder.append(officeEmail);
		builder.append(", perEmail: ");
		builder.append(perEmail);
		builder.append(", officeMobile");
		builder.append(officeMobile);

		return builder.toString();
	}
}
```

#### App_Autowiring.java
```java
public class App_Autowiring {
	public static void main(String[] args) {
		try (ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beanconfig_autowiring.xml")) {
			Customer customer = null;

			System.out.println("\nUsing By Name");
			customer = context.getBean("customerByName", Customer.class);

			System.out.println("\nUsing By Type");
			customer = context.getBean("customerByType", Customer.class);

			System.out.println("\nUsing Constructor");
			customer = context.getBean("customerConstructor", Customer.class);

		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### beanconfig_autowiring.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
		xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
		xmlns:context = "http://www.springframework.org/schema/context"
		xmlns:c="http://www.springframework.org/schema/c"
		xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans/xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd"
		default-init-method = "init" default-destroy-method = "destroy">

	<bean id = "officeEmail" class = "com.psl.spring.a.di.entity.Email"
		p:address="myname@email.com" lazy-init="true" init-method="init" destroy-method="destroy"/>

	<bean id = "officeMobile" class = "com.psl.spring.a.di.entity.MobileNumber"
	    p:number="9988877666" lazy-init="true" init-method="init" destroy-method="destroy"/>

	<bean id = "customerByName" autowire = "byName"
		class = "com.psl.spring.a.di.entity.Customer" lazy-init="true"/>
	<bean id = "customerByType" autowire = "byType"
		class = "com.psl.spring.a.di.entity.Customer" lazy-init = "true"/>
	<bean id = "customerConstructor" autowire = "constructor"
		class = "com.psl.spring.a.di.entity.Customer" lazy-init = "true"/>
</beans>
```

- Lazy Loading *(lazy-init)* if nobody asks the instance, it won't be created.
- **byName** - it will find out a bean with the same name, if it finds it, it'll attach it, if it doesn't finds it it'll omit it.
- **byType** - it will go for every attribute it'll check its type, if we get any bean which if of the same type, it'll use it otherwise it'll omit it.
- **constructor** this will find out is their any suitable constructor which matches to the properties, and then it'll choose it. it chooses the best possible constructor.

#### Output
```
Using By Name:
Customer() called
Email.init()
Mobile.init()
setOfficeEmail(Email officeEmail)
setOfficeMobile(MobileNumber officeMobile)

Using By Type:
Customer() called
setOfficeEmail(Email officeEmail)
setOfficeMobile(MobileNumber officeMobile)

Using Constructor
Customer(Email, Mobile) called
Mobile.Destroy()
Email.Destroy()
```

-------------------------------------------

## Annotations 

#### BeanConfig.java (Annotation Based Configuration)
```java
package com.psl.spring.a.di.config;

import org.springframework.context.annotation.Bean;

@Configuration //this is my configuration class
public class BeanConfig {
	@Bean //it defines this method is kind of factory method
	public Vehicle vehicle() {
		Vehicle vehicle = new Vehicle("MH23 PQ 3333", 6);
		return vehicle;
	}

	@Bean("engine")
	public Engine getEngine() {
		Engine engine = new Engine(77.7, 6);
		return engine;
	}

	@Bean("extraEngine")
	public Engine getExtraEngine() {
		Engine engine = new Engine(55.5, 10);
		return engine;
	}
}
```

#### Vehicle.java
```java
package com.psl.spring.a.di.entity;

import org.springframework.beans.factory.annotation.Autowired;

public class Vehicle {
	private String rtoNumber;
	private int capacity;

	@Autowired(required = false)
	private Engine engine;

	@Autowired(required = false)
	//👇is used to differentiate a bean among the same type of bean objects
	//If we have more than one bean of the same type and want to wire 
	//only one of them then use the @Qualifier annotation along with 
	//@Autowired to specify which exact bean will be wired.
	
	//@Qualifier //will match this with qualifier
	private Engine extraEngine;

	public Vehicle() {}
	public Vehicle(String rtoNumber, int capacity) {
		super();
		System.out.println("In parameter constructor");
		this.rtoNumber = rtoNumber;
		this.capacity = capacity;
	}

	public Engine getExtraEngine() { return extraEngine; }
	public Engine getEngine() { return engine; }
	public String getRtoNumber() { return rtoNumber; }
	public int getCapacity() { return capacity; }

	public void setExtraEngine(Engine extraEngine) { this.extraEngine = extraEngine; }
	public void setEngine(Engine engine) { this.engine = engine; }
	public void setRtoNumber(String rtoNumber) { this.rtoNumber = rtoNumber; }
	public void setCapacity(int capacity) { this.capacity = capacity; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("Vehicle (rtoNumber : ");
		builder.append(rtoNumber);
		builder.append(", capacity : ");
		builder.append(capacity);
		builder.append(", engine : ");
		builder.append(engine);
		builder.append(", extraEngine : ");
		builder.append(extraEngine);

		return builder.toString();
	}
}
```

#### App_Annotations.java
```java
package com.psl.spring.a.di;

import org.springframewoek.context.ApplicationContext;

public class App_Annotations {
	public static void main(String[] args) {
		try {
			ApplicationContext context = new AnnotationConfigApplicationContext(BeanConfig.class);
			Engine engineFromConfig = contextBeanConfig.getBean("extraEngine", Engine.class);
			System.out.println(engineFromConfig);

			Vehicle firstVehicle = contextBeanConfig.getBean("vehicle", Vehicle.class);
			System.out.println(firstVehicle);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### Output
```
In parameter constructor
Engine [power=55.5, cylinders=10]
Vehicle [rtoNumber=MH 23 PQ 3333, capacity=6, engine=Engine [power=77.7, cylinders=6], extraEngine=Engine [power=55.5, cylinders=10]]
```

