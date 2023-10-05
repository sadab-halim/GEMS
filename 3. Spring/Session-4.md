# Session - 4 | Spring AOP

## ProxyIntro
- Aspect Oriented Programming is done by Spring by using *proxies*.

#### AccountDAO.java
```java
package com.psl.proxy;

public class AccountDAO implements DAO {
	private void addAccount() {
		System.out.println("Account added");
	}
}
```

#### DAO.java
```java
package com.psl.proxy;

public interface DAO {
	//interface methods are always abstract and public
	void addAccount(); 
}
```

#### AccountDAOProxy.java
```java
public class AccountDAOProxy implements DAO {
	private AccountDAO dao;

	public AccountDAOProxy (AccountDAO dao) {
		this.dao = dao;
	}
	//proxy has taked the control of the original object
	@Override
	public void addAccount() {
		//pre-processing
		System.out.println("Checked Authenticiy");
		dao.addAccount();
		//post-processing
		System.out.println("Log Request");
	}
}
```

#### TestClient.java
```java
package com.psl.proxy;

public class TestClient {
	public static void main (String[] args) {
		DAO dao = new AccountDAOProxy(new AccountDAO());
		process(dao);
	}
	private static void process(DAO dao) {
		dao.addAccount();
	}
}
```

#### Output
```
Checked Authenticity
Account Added
Log Request
```

-----------------------------------------------

## Aspect Oriented Programming
- Create Maven Project with following pom file dependencies
- NOTE: from *aspectjrt* and *aspectjweaver* dependency tags remove <scope>runtime</scope> if present. 
- **Aspect** : Advice which can be place **@Before** or **@After** specific method execution.
- **Advice** : The method to be invoked
	- **Before** : Before is called using Before advice, before a join point (target method) executes
	- **After-returning** : After returning advice is executed after the method invocation at join point (target method) has finished executing and has returned its value. After-returning is actually *after the value is returned* NOT before the value is returned
	- **After-finally** : After-returning if the method throws exception, this doesn't get called. But After-finally it'' definitely get called, even if throws exception
	- **Around** :

	- **Throws** :

- **Pointcut** : Predicate Expression deciding where advice needs to be applied. We use pointcut expression language { ? means optional }
	- execution (
		- modifiers-pattern? (Spring AOP supports only *public*)
		- return-type-patter (*void, int, List<..> etc*)
		- method-name-pattern (param-pattern)
		- throws-pattern?)

- Parameter type usage
- **()** : matches method without parameters
- **(*)** : matches method with one parameter
- **(..)** : matches a method with 0 or more arguments of any type
- **Com.*.*(..)** : matches any method in any class in sub-package of com

- Examples:
	- @Before (execution(public void com.abc.AccountDAO.addAccount())")
	- @Before (execution(public void add*())")
	- @Before (execution(public void add*(com.xyz.Account))")
	- @Before (execution(public void add*(com.xyz.Account, ..))")

- Adding Java Class Based Bean Configuration
#### DemoConfig.java
```java
package com.psl.spring.d.aop.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

import com.psl.spring.d.aop.dao.MyAccountDAO;

@Configuration
//👇enables support for handling components marked with AspectJ's @Aspect
@EnableAspectJAutoProxy //Spring AOP Proxy Support
//component scan is required so that it go inside that folders
//and scan the components
//whatever package is inside this base package will be scanned for components
@ComponentScan("com.psl.spring.d.aop") //which packages to scan; base package
public class DemoConfig {
	//if the method name is different than what we have given
	//we need to describe it as a bean
	@Bean
	public MyAccountDAO accountDAO() {
		return new MyAccountDAO();
	}
}
```

#### MyAccountDAO.java
```java
package com.psl.spring.d.aop.dao;

import org.springframework.stereotype.Component;

//when we declare this as component, then spring goes to this class,
//and automatically derives its attributes and convert it into a bean
//👇it allows Spring to detect our custom beans automatically
@Component
public class MyAccountDAO {
	public void addAccount() {
		System.out.println(getClass() + " -> addAccount()");
	}
}
```

#### LoggingAspect.java
```java
package com.psl.spring.d.aop.aspect;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

//👇 declares the class as an aspect
@Aspect
@Component
public class LoggingAspect {
	//this is applicable to multiple methods 👇
	@Before("execution (* add*())") //generic type
	//☝️ it should run this method, before the target method is called, i.e., add*()
	public void beforeAddAccountAdvice() {
		System.out.println("---------- Pre-Processing Advice on addAccount() ----------");
	}
	//this is applicable to only one method 👇
	@After("execution (public void addAccount())") //specific type
	//☝️ method execution it will run the method after the execution of the addAccount()
	public void afterAddAccountAdvice() {
		System.out.println("---------- Post-Processing Advice on addAccount() ----------");
	}
}

```

#### App.java
```java
package com.psl.spring.d.aop;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.psl.spring.d.aop.config.DemoConfig;
import com.psl.spring.d.aop.dao.MyAccountDAO;
import com.psl.spring.d.aop.dao.MyBankDAO;
import com.psl.spring.d.aop.dao.MyCustomerDAO;

public class App {
    public static void main(String[] args) {
    	//read spring configuration
    	try (AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DemoConfig.class)) {
    		//get the bean
    		MyAccountDAO dao = context.getBean("accountDAO", MyAccountDAO.class);
    		//call the business method
    		dao.addAccount();
    	}
    }
}
```

#### Output
```
---------- PRE-PROCESSING ADVICE ON addAccount() ----------
class com.psl.spring.d.aop.dao.MyAccountDAO -> addAccount()
---------- POST-PROCESSING ADVICE ON addAccount() ----------
```

-------------------------------------------

### Modification Version - 1 
#### MyCustomerDAO.java
```java
package com.psl.spring.d.aop.dao;

import org.springframework.stereotype.Component;

@Component
public class MyCustomerDAO {
	public void addCustomer() {
		System.out.println(getClass() + " -> addCustomer()");
		addNumber();
	}
	//even though this qualified for add()* but since it's private; it won't get printed
	private void addNumber() {
		System.out.println(getClass() + " -> addNumber()");
	}
}
```

#### MyBankDAO.java
```java
package com.psl.spring.d.aop.dao;

import org.springframework.stereotype.Component;

@Component
public class MyBankDAO {
	public void addBank() {
		System.out.println(getClass() + " -> addBank()");
	}
}
```

#### App.java
```java
package com.psl.spring.d.aop;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.psl.spring.d.aop.config.DemoConfig;
import com.psl.spring.d.aop.dao.MyAccountDAO;
import com.psl.spring.d.aop.dao.MyBankDAO;
import com.psl.spring.d.aop.dao.MyCustomerDAO;

public class App {
    public static void main(String[] args) {
    	//read spring configuration
    	try (AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DemoConfig.class)) {
    		//get the bean
    		MyAccountDAO dao = context.getBean("accountDAO", MyAccountDAO.class);
    		dao.addAccount(); //call the business method
    		
    		MyCustomerDAO cDao = context.getBean("myCustomerDAO", MyCustomerDAO.class);
    		cDao.addCustomer(); //call the business method
    		
    		MyBankDAO bDao = context.getBean("myBankDAO", MyBankDAO.class);
    		bDao.addBank(); //call the business method
    		//whenever these methods are called, Aspect gets triggered
    	}
    }
}
```

#### Output
```
---------- PRE-PROCESSING ADVICE ON addAccount() ----------
class com.psl.spring.d.aop.dao.MyAccountDAO -> addAccount()
---------- POST-PROCESSING ADVICE ON addAccount() ----------

---------- PRE-PROCESSING ADVICE ON addAccount() ----------
class com.psl.spring.d.aop.dao.MyCustomerDAO -> addCustomer()
class com.psl.spring.d.aop.dao.MyCustomerDAO -> addNumber()

---------- PRE-PROCESSING ADVICE ON addAccount() ----------
class com.psl.spring.d.aop.dao.MyBankDAO -> addBank()
```

-------------------------------------------

### Modification Version - 2
#### LoggingAspect.java
```java
package com.psl.spring.d.aop.aspect;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {
	@Before("execution (* add*())") 
	//☝️ it should run this method, before the target method is called, i.e., add*()
	public void beforeAddAccountAdvice() {
		System.out.println("---------- PRE-PROCESSING ADVICE ----------");
	}
	@After("execution (* add*())") 
	//☝️ method execution it will run the method after the execution of the add*()
	public void afterAddAccountAdvice() {
		System.out.println("---------- POST-PROCESSING ADVICE ----------");
	}
}
```

#### Output
```
---------- PRE-PROCESSING ADVICE ----------
class com.psl.spring.d.aop.dao.MyAccountDAO -> addAccount()
---------- POST-PROCESSING ADVICE ----------

---------- PRE-PROCESSING ADVICE ----------
class com.psl.spring.d.aop.dao.MyCustomerDAO -> addCustomer()
class com.psl.spring.d.aop.dao.MyCustomerDAO -> addNumber()
---------- POST-PROCESSING ADVICE ----------

---------- PRE-PROCESSING ADVICE ----------
class com.psl.spring.d.aop.dao.MyBankDAO -> addBank()
---------- POST-PROCESSING ADVICE ----------
```

-------------------------------------------

### Modification Version - 3
- we can define Pointcut expressions, to apply the same at multiple methods
- Pointcut express says that, if the given expression matches *(given in the parenthesis) then only apply the below method

#### LoggingAspect.java
```java
package com.psl.spring.d.aop.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {
	@Before("execution (* add*())") //generic type
	//it should run this method, before the target method is called, i.e., add*()
	public void beforeAddAccountAdvice() {
		System.out.println("---------- Pre-Processing Advice on addAccount() ----------");
	}

	@After("execution (public void addAccount())") //specific type
	//method execution it will run the method after the execution of the addAccount()
	public void afterAddAccountAdvice() {
		System.out.println("---------- Post-Processing Advice on addAccount() ----------");
	}

	@Pointcut("execution (* addBank())")
	public void pointCutMethod() {
		System.out.println("---------- In PointCut Method ----------");
	}

	// this pointCutMethod() 👇 is only applicable to addBank()
	@Around("pointCutMethod()") //means some portion before, some portion after
	//whenever this condition ☝️ matches, this method 👇 will be applied
	public void aroundAddAccountAdvice(ProceedingJoinPoint pjp) throws Throwable {
		//when the method is applied
		//first of all, it'll execute till this point 👇
		System.out.println("////////// AROUND : PRE-PROCESSING ADVICE - BEFORE ON addBank()");
		//and then it'll come here 👇
		pjp.proceed(); //👈 this calls the method, as we can see in the output
		//and then it'll come here 👇
		System.out.println("////////// AROUND : POST-PROCESSING ADVICE - AFTER ON addBank()");
	}
}
```

#### Output
```
---------- PRE-PROCESSING ADVICE ----------
class com.psl.spring.d.aop.dao.MyAccountDAO -> addAccount()
---------- POST-PROCESSING ADVICE ----------

---------- PRE-PROCESSING ADVICE ----------
class com.psl.spring.d.aop.dao.MyCustomerDAO -> addCustomer()
class com.psl.spring.d.aop.dao.MyCustomerDAO -> addNumber()
---------- POST-PROCESSING ADVICE ----------

////////// AROUND : PRE-PROCESSING ADVICE - BEFORE ON addBank()
---------- PRE-PROCESSING ADVICE ----------
class com.psl.spring.d.aop.dao.MyBankDAO -> addBank()
---------- POST-PROCESSING ADVICE ----------
////////// AROUND : POST-PROCESSING ADVICE - AFTER ON addBank()
```

----------------------------------------------------




