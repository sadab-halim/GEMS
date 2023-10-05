# Session - 3 | Collections, JDBC, ORM

## Injecting Collections
- Setter Injections first create the empty objects; and then it'll go on creating the properties..
#### beanconfig_collections.xml
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

		<!-- User Defined Data Type -->
		<bean id = "someEntity" class = "com.psl.spring.a.di.entity.collection.SomeEntity"/>
		<bean id = "injectCollection" class = "com.psl.spring.a.di.entity.collection.CollectionInjection">
			<!-- Map -->
			<property name = "map">
				<map>
					<entry key = "key-1" value = "value1"/>
					<entry key = "key-2">
						<value>value2</value>
					</entry>
					<entry key = "key-3">
						<value>value3</value>
					</entry>
				</map>
			</property>
			
			<!-- Set -->
			<property name = "set">
				<set>
					<value>Rice</value>
					<value>Dal</value>
					<value>Chappati</value>
					<value>Chappati</value>
					<value>Chappati</value>
					<value>Vegetables</value>
					<!-- Since this is not a String; it'll throw an exception 👇 -->
					<!-- <ref bean = "someEntity"/> -->
				</set>
			</property>
			
			<!-- List -->
			<property name = "list">
				<list>
					<value>Suresh</value>
					<value>Ramesh</value>
					<value>Haresh</value>
					<value>Geeta</value>
					<value>Geeta</value>
					<value>Geeta</value>
					<ref bean = "someEntity"/>
				</list>
			</property>	
		</bean>
</beans>
```

#### SomeEntity.java
```java
package com.psl.spring.a.di.entity.collection;

public class SomeEntity {
	private String value = "Test Value";

	public SomeEntity() {}
	public SomeEntity(String value) {
		super();
		this.value = value;
	}
	
	public String getValue() { return value; }
	public void setValue(String value) { this.value = value; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("SomeEntity (Value : ");
		builder.append(value);
		builder.append(")");
		return builder.toString();
	}
}
```

#### CollectionInjection.java
```java
package com.psl.spring.a.di.enity.collection;

import java.util.List;
import java.util.Map;
import java.util.Set;

public class CollectionInjection {
	private Map<String, Object> map;
	private Set<String> set;
	private List<Object> list;	//objects mean we can add any objects to it

	public Set<String> getSet() { return set; }
	public List<Object> getList() { return list; }
	public Map<String, Object> getMap() { return map; }
	
	public void setMap(Map<String, Object> map) { this.map = map; }
	public void setSet(Set<String> set) { this.set = set; }
	public void setList(List<Object> list) { this.list = list; }
	
	public void displayInfo() {
		System.out.println("\nMap Contents:");
		map.entrySet().stream().forEach(e -> System.out.println("{ " + e.getKey() + " - " + e.getValue() + "}"));
		
		System.out.println("\nSet Contentss:");
		set.forEach(obj->System.out.println("(" + obj + ")"));
		
		System.out.println("\nList Contents:");
		list.forEach(obj->System.out.println("(" + obj + ")"));
	}
}
```

#### App_Collections.java
```java
package com.psl.spring.a.di;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.psl.spring.a.di.entity.collection.CollectionInjection;

public class App_Collections {
	public static void main(String[] args) {
		try (ClassPathXmlApplicationContext context = new ClassPathXmlAPplicationContext("beanconfig_collections.xml")) {
			CollectionInjection instance = context.getBean("injectCollection", CollectionInjection.class);
			instance.displayInfo();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### Output
```
Map Contents:
{ key1 - value1 }
{ key2 - value2 }
{ key3 - SomeEntity (value = Test Value)}

Set Contents:
[Rice]
[Dal]
[Chappati]
[Vegetables]

List Contents:
(Suresh)
(Ramesh)
(Haresh)
(Geeta)
(Seeta)
(Geeta)
(SomeEntity (value = Test Value))
```

---------------------------------------------------------------------------

## Using Persistent Layer through JDBC
- All the business logic which we run is present inside the business layer.
- When we say something store in database, it will be managed by its own servers.
- SQL provides a Driver to the Relational Database.
	- This SQL Driver (*jar file*) is a sql implementation, but we have interfaces in Java.
- Business Layer can access SQL Driver in order to access its RDBMS.
	- But the prob is that, the code gets *tightly coupled*.
	- To avoid this prob, such an access is never done.
	- Instead what we do is, we use a *layer of direction* - **Service Layer**
	- Service Layer as the name suggests it provides services.
- Service Layer internally may use a DAO layer.
- Service might be involving multiple operations, whereas in the **DAO layer** it'll just communicate to the database.
- DAO Layer is very specific to database.

```
|-------------------|     |-------------------|     |-------------------|            |------------|
|                   |     |                   |     |                   |            |            |
|   Business Layer  | --> |   Service Layer   | --> |     DAO Layer     |            |    RDBMS   |
| (busingess logic) | --> |  (transferAmt())  | --> |   (Account DAO)   | --> |-------            |
|                   |     |                   |     |                   |     |  SQL |            |
|-------------------|     |-------------------|     |-------------------|     |-------------------|
```

#### pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.spring.jdbc</groupId>
  <artifactId>dataacess</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>
  <name>SpringDataAccessJDBC</name>
  <description>Spring DataAccess using JDBC</description>
  
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>

	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>5.3.22</version>
	</dependency>
	
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-orm</artifactId>
	    <version>5.3.22</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-jdbc</artifactId>
	    <version>5.3.22</version>
	</dependency>
	
	<!-- MySQL Connector/J -->
	<!-- This Connector Converts the Java Code into SQL 👇 -->
	<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	    <version>8.0.30</version>
	</dependency>
	
	<!-- Database Connection -->
	<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
	<dependency>
	    <groupId>org.apache.commons</groupId>
	    <artifactId>commons-dbcp2</artifactId>
	    <version>2.1.1</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
	<dependency>
	    <groupId>org.hibernate</groupId>
	    <artifactId>hibernate-core</artifactId>
	    <version>6.1.2.Final</version>
	    <type>pom</type>
	</dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  
  <build>
	  <plugins>
		  <plugin>
			  <groupId>org.apache.maven.plugins</groupId>
			  <artifactId>maven-compiler-plugin</artifactId>
			  <version>3.10.0</version>
			  <configuration>
				  <source>1.8</source>
				  <target>1.8</target>
			  </configuration>
		  </plugin>
	  </plugins>
  </build>
</project>
```

- The credentials of database are stored in the **application.properties** file
- All the resources are kept inside the resources folder.

#### application.properties
```
jdbc.url=jdbc:mysql://localhost:3306/employees
jdbc.username = root
jdbc.password = admin
```
### DAO Layer
- Whenever we create a DAO, it's a good practice to first create it's Interface.
#### EmployeeDAO.java 
```java
import java.util.List;

public interface EmployeeDAO {
	public Employee getEmployeeById(int id); //business methods
	public List<Employee> getEmployees(); //business methods
	public boolean createEmployee(Employee emp); //business methods
}
```

- EmployeeDAOImpl is the class which implements the EmployeeDAO interface, it's a convention.
- We use Autowired, Beans and other annotations, for resources and NOT for business objects.
- JdbcTemplate is a resource which is going to help us to connect with the Jdbc
- DAO layer is the most nearest layer to the database

#### EmployeeDAOImpl.java
```java
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.dao.DataAccessException;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class EmployeeDAOImpl implements EmployeeDAO {
	
	@Autowired
	private JdbcTemplate jdbcTemplate; //this will help to connect with jdbc
	//business methods
	public Employee getEmployeeById(int id) {
		//normal MySQL query
		String sql = "SELECT * FROM EMPLOYEE WHERE EMPID = ?";
		//generating sql query; using `queryForObject` to get a simple employee by passing the sql
		//BeanPropertyRowMapper is a readymade thing given by Spring Framework to recieve an object and iterate over it.
		Employee emp = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Employee>(Employee.class), id);
		return emp;
	}
	
	public List<Employee> getEmployees() {
		//firing sql query
		String sql = "SELECT * FROM EMPLOYEE";
		//this BeanPropertyRowMapper automatically converts everything into a list of employees
		//BeanPropertyRowMapper also does the task of ResultSet by iterating over the data..
		List<Employee> allEmployees = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Employee>(Employee.class));
		return allEmployees;
	}
	
	public boolean createEmployee(Employee emp) {
		try {
			//firing query for inserting records
			String sql = "ISNERT INTO EMPLOYEE VALUES (?, ?, ?, ?)";
			//update query does two things:
			//   a. if objects exists, it will update it
			//   b. if it doesn't exists, it will create it
			jdbcTemplate.update(sql, emp.getEmpid(), emp.getEmpname(), emp.getCity(), emp.getJoindate());
			return true;
		} catch (DataAccessException e) {
			e.printStackTrace();
			return false;
		}
	}
}
```

### Service Layer
- Service requires the DAO to execute it, Services is going to delicate the activity to DAO and whatever the DAO returns it is going to return it.
- Service Layer is isolating the dao from the business layer
- When we are going to write DAO, we might have some certain things such as JdbcTemplate or any sql kind of imports may come at the DAO layer.
- But at the Service layer we should not have any dependency on specific database.
	- because this layer is independent of specific database.
- Service Layer isolates the business layer from database layer, this is known as **layer of indirection**.

#### EmployeeService.java
```java
package database.service;

import java.util.List;

public interface EmployeeService {
	public Employee getEmployeeById(int id);
	public List<Employee> getEmployees();
	public void createEmployees(Employee emp);
}
```

#### EmployeeServiceImpl.java
```java
package dataacess.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import dataacess.dao.EmployeeDAO;
import dataccess.model.Employee;
//can handle transactions
@Service
public class EmployeeServiceImpl implements EmployeeService {
	//dao is obtained from autowiring
	@Autowired
	private EmployeeDAO dao;
	
	public Employee getEmployeeById(int id) { 
		return dao.getEmployeeById(id)
	}
	
	public List<Employee> getEmployees() {
		return dao.getEmployees();
	}
}
```

- Bean Configuration is a java based configuration for all the beans which we are going to use.

#### BeanConfig.java
```java
package dataacess.config;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

//👇indicates that the class has @Bean definition methods
//So Spring container can process the class 
//and generate Spring Beans to be used in the app
@Configuration 
//👇it specifies that from basePackages it goes to every package of data acess, and scans
//every annotations which are given for the entities, service, dao
@ComponentScan(basePackages = "dataaccess") 
//👇it is used to provide properties file to Spring Environment.
//this annotation is used with @Configuration classes
@PropertySource("classpath:/application.properties")
public class BeanConfig {
	//establishing connection
	//👇is used to assign default values to variables and method arguments
	@Value("${jdbc.url}") //it reads from the properties file, and set it here
	private String url;
	
	@Value("${jdbc.username}") //it reads from the properties file, and set it here
	private String username;
	
	@Value("${jdbc.password}") //it reads from the properties file, and set it here
	private String password;
	
	//👇is a method-level anntotation
	//it is applied on a method to specify that 
	//it returns a bean to be managed by Spring context
	@Bean
	public DataSource getDataSource() { //DataSource works as a factory for providing database connections.
		DriverManagerDataSource dataSource = new DriverManagerDataSource();
		dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
		dataSource.setUrl(url);
		dataSource.setUsername(username);
		dataSource.setPassword(password);
		return dataSource;
	}
	
	//helps to execute the queries
	@Bean
	public JdbcTemplate getJdbcTemplate() {
		JdbcTemplate jdbcTemplate;
		jdbcTemplate = new JdbcTemplate();
		jdbcTemplate.setDataSource(getDataSource());
		return jdbcTemplate;
	}
}
```

#### TestClient.java
```java
package dataaccess.client;

import java.text.ParseException;

public class TestClient {
	public static int empId = Maths.abs((int)System.currentTimeMills());

	public static void main(String[] args) {
		try (AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(BeanConfig.class)) {
			EmployeeService service = context.getBean(EmployeeService.class);

			Employee emp = new Employee(empId++, "Rober Pirsirg", "Rome", new SimpleDateFormat("yyyy-MM-dd").parse("2020-02-02"));
			service.createEmployee(emp);

			List<Employee> allEmployees = service.getEmployees();
			allEmployees.forEach(System.out::println);
			//same as above
			/*
			allEmployees.forEach(e -> System.out.println(e));
			*/
			Employee fetchedEmployee = service.getEmployeeById(110);
			System.out.println(fetchedEmployee);
		
		} catch (BeansException | PatseException e) {
			e.printStackTrace();
		} 
	}
}
```

#### Output
```
Employee [empid=992811928, empname=Robert Pirsig, city=Rome, joindate=2020-02-02 00:00:00.0]
Employee [empid=992888572, empname=Robert Pirsig, city=Rome, joindate=2020-02-02 00:00:00.0]
Employee [empid=992987579, empname=Robert Pirsig, city=Rome, joindate=2020-02-02 00:00:00.0]
Employee [empid=1008683945, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
Employee [empid=1008811486, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
Employee [empid=1008864195, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
Employee [empid=1008864195, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
```

-------------------------------------------------------------------------------------------

## Spring with Hibernate / ORM (Object Relational Mapping)
- ORM Framework is independent of business logic as well as database.
- ORM is a facility which we get by Hibernate (framework)
- Spring is a framework using which we can connect our applications.
- jaxb is a connectivity between XML file and Java Code.
	- It creates classes for XML and suitable Java code
- ORM is mapping between Objects and Database Queries.

```
|-------------------|     |-------------------|     |-------------------|     |------|            |------------|
|                   |     |                   |     |                   |     |      |            |            |
|   Business Layer  | --> |   Service Layer   | --> |     DAO Layer     |     |      |            |    RDBMS   |
| (busingess logic) | --> |  (transferAmt())  | --> |   (Account DAO)   | --> | ORM  | --> |-------            |
|                   |     |                   |     |                   |     |      |     |  SQL |            |
|-------------------|     |-------------------|     |-------------------|     |------|     |-------------------|
```

#### pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.psl.training</groupId>
  <artifactId>spring-hibernate-mod</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>spring-hibernate-mod</name>
  <url>http://maven.apache.org</url>

  <properties>
    <jdk.version>1.8</jdk.version>
  </properties>

  <dependencies>

	 <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
	 <dependency>
		 <groupId>org.springframework</groupId>
		 <artifactId>spring-context</artifactId>
		 <version>5.3.14</version>
	 </dependency>

	 <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
	 <dependency>
		 <groupId>org.springframework</groupId>
		 <artifactId>spring-core</artifactId>
		 <version>5.3.25</version>
	 </dependency>

	 <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	 <dependency>
		 <groupId>mysql</groupId>
		 <artifactId>mysql-connector-java</artifactId>
		 <version>8.0.28</version>
	 </dependency>

	 <!-- Apache Commons DBCP -->
	 <dependency>
		 <groupId>org.apache.commons</groupId>
		 <artifactId>commons-dbcp2</artifactId>
		 <version>2.1.1</version>
	 </dependency>

	 <!-- Hibernate 5.3.6 Final -->
	 <dependency>
		 <groupId>org.hibernate</groupId>
		 <artifactId>hibernate-core</artifactId>
		 <version>5.3.6.Final</version>
	 </dependency>

	 <!-- Spring ORM https://mvnrepository.com/artifact/org.springframework/spring-orm -->
	 <dependency>
		 <groupId>org.springframework</groupId>
		 <artifactId>spring-orm</artifactId>
		 <version>5.2.6.RELEASE</version>
	 </dependency>
	 
	 <dependency>
		 <groupId>javax.xml.bind</groupId>
		 <artifactId>jaxb-api</artifactId>
		 <version>2.3.0</version>
	 </dependency>
</dependencies>
</project>
```

#### application.properties
```
mysql.driver.classname=com.mysql.cj.jdbc.Driver
mysql.url=jdbc:mysql://localhost:3306/employees
mysql.username=root
mysql.password=admin
```

- Jaxb is connectivity between xml files and java codes.
- Session Factory is for the Session

#### mybeansConfig.xml
```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
	xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation = "http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- THIS CONFIG IS ONLY FOR REF AND NOT USED
	<bean id = "v1" class = "com.psl.training.Vehicle">
		<property name = "name" value = "Activa" />
		<property name = "color" value = "Black" />
	</bean>

	<bean id = "v2" class = "com.psl.training.Vehicle">
		<property name = "name" value = "Shine" />
		<property name = "color" value = "Red" />
	</bean>
	-->
</beans>
```

#### BeanConfig.java
```java
package com.psl.training.config;

import java.util.Properties;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.hibernate5.LocalSessionFactoryBean;

//this annotation will tell spring that this class provide configuration
@Configuration
@ComponentScan(basePackages = "com.psl.training")
@PropertySource(value = "classpath:/application.properties")
public class BeanConfig {
	//establishing connection
	//responsible for making connection with database
	 @Value("${mysql.driver.classname}")
	 String driverClassName;
	 
	 @Value("${mysql.url}")
	 String url;
	 
	 @Value("${mysql.username}")
	 String username;
	 
	 @Value("${mysql.password}")
	 String password;
	 
	 @Bean //(name = "dataSource")
	 public DataSource getDataSource() {
		 DriverManagerDataSource datasource = new DriverManagerDataSource();
		 datasource.setDriverClassName(driverClassName);
		 datasource.setUrl(url);
		 datasource.setUsername(username);
		 datasource.setPassword(password);
		 
		 return datasource;
	 }
	 
	 @Bean
	 //sessionFactory is for the sessions 👇
	 public LocalSessionFactoryBean getSessionFactoryBean() {
		 LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
		 sessionFactory.setDataSource(getDataSource());
		 //setting the scan path where JPA entity annotations are present
		 sessionFactory.setPackagesToScan("com.psl.training.model");
		 //setting hibernate specific properties like show_sql
		 Properties props = new Properties();
		 //to print all the sql statements generated by hibernate
		 props.put("hibernate.show_sql", "true");
		 //create tables at the backend with the help of entity annotations present
		 //it'll create table if not present and alter table if fields are not present as mentioned 
		 props.put("hibernate.hb,2ddl.auto", "update");
		 
		 sessionFactory.setHibernateProperties(props);
		 return sessionFactory;
	 }
}
```

- Every class will be associated with one table
- And every instance of that class will be associated with one row of that table

#### Employee.java
```java
package com.psl.training.model;

import java.sql.Date;

import javax.persistence.Id;

import org.hibernate.annotations.Entity;
import org.hibernate.annotations.Table;

//👇specifies that the class is an entity and is mapped to a database table
@Entity
//mapping table with Java Object 
//(name = "employee") --> optional, if u want to give another name to ur table other than the class name.
//by default, it takes the class name, in the camelCase format.
//👇specifies the name of the database table to be used for mapping
@Table(name = "employee")
public class Employee {
	//for generating primary key values automatically; optional
	//@GeneratedValue(strategy = GenerationType.IDENTITY) 
	@Id //indices the member field below is a primary key of the current entity
	private int empid;
	private String empname;
	private String city;
	private Date joindate;
	
	public Employee() {}
	public Employee(int empid, String empname, String city, Date joindate) {
		super();
		this.empid = empid;
		this.empname = empname;
		this.city = city;
		this.joindate = joindate;
	}
	
	public int getEmpid() { return empid; }
	public String getEmpname() { return empname; }
	public String getCity() { return city; }
	public Date getJoindate() { return joindate; }

	public void setEmpid(int empid) { this.empid = empid; }
	public void setEmpname(String empname) { this.empname = empname; }
	public void setCity(String city) { this.city = city; }
	public void setJoindate(Date joindate) { this.joindate = joindate; }
	
	@Override
	public String toString() {
		return "Employee (empid=" + empid + ", empname=" + empname + ", city=" + city + ", joindate=" + joindate + ")";
	}
}
```

#### EmployeeServiceImpl.java
```java
package com.psl.training.service;

import java.util.List;
//may handle transactions
@Service
public class EmployeeServiceImpl implements EmployeeService {
	@Autowired
	EmployeeDAO1 dao;

	public boolean insertEmployee(Employee emp) {
		return dao.insertEmployee(emp);
	}

	public Employee getEmployeeById(int id) {
		return dao.getEmployeeById(id);
	}

	public List<Employee> getAllEmployees() {

	}
}
```

#### EmployeeDAOImpl.java
```java
public class EmployeeDAOImpl implements EmployeeDAO1 {
	@Autowired
	private SessionFactory sessionFactory;

	public EmployeeDAOImpl() {}

	public boolean insertEmployee(Employee emp) {
		try {
			Session session = sessionFactory.openSession();
			session.beginTransaction(); //transaction started
			session.save(emp); //insert
			//if it throws exception it'll not be committed
			session.getTransaction().commit();
			return true;
		} catch (Exception e) {
			throw e;
		}
	}

	public Employee getEmployeeById(int id) {
		//hibernate will use session for performing db operations
		Session session = sessionFactory.openSession();
		return session.get(Employee.class, id)l
	}

	public List<Employee> getAllEmployees() {
		Session session = sessionFactory.openSession();
		return session.createQuery("from Employee database", Employee.class).list();
	}

	public boolean updateEmployee(Employee emp) {
		try {
			Session session = sessionFactory.openSession();
			session.beginTransaction(); //transaction started
			session.save(emp); //if record not present it'll insert else it'll update
			session.getTransaction().commit();
			return true;
		} catch (Exception e) {
			throw e;
		}
	}

	public boolean deleteEmployee(int id) {
		boolean deleted = false;
		Session session = sessionFactory.openSession();
		session.beginTransaction(); //transaction started
		Employee e = session.get(Employee.class, id);

		if(e != null) {
			session.delete(e);
			session.getTransaction().commit();
			deleted = true;
		}
		return deleted;
	}
}
```

#### Main.java
```java
package com.psl.training.test;

import java.sql.Date;

public class Main {
	private static int empidSeed = Math.abs((int) System.currentTimeMillis());

	public static void main(String[] args) {
		//Configuration in XML
		//ApplicationContext context = new ClassPathXmlApplicationContext("beanConfig.xml");
		
		//Configuration in Java Class
		ApplicationContext context = new AnnotationConfigApplicationContext(BeanConfig.class);
		Employee emp = new Employee(empidSeed++, "Aishwariya", "Pune", Date.valueOf(LocalDate.now()));

		EmployeeService service = context.getBean(EmployeeServiceImpl.class);

		boolean isInserted = service.insertEmployee(emp);
		System.out.println("Data Inserted Successfully" + isInserted);

		//System.out.println(service.getEmployeeById(101));

		service.getAllEmployees().forEach(employee -> System.out.println(employee));

	}
}
```

#### Output
```
Employee [empid=992811928, empname=Robert Pirsig, city=Rome, joindate=2020-02-02 00:00:00.0]
Employee [empid=992888572, empname=Robert Pirsig, city=Rome, joindate=2020-02-02 00:00:00.0]
Employee [empid=992987579, empname=Robert Pirsig, city=Rome, joindate=2020-02-02 00:00:00.0]
Employee [empid=1008683945, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
Employee [empid=1008811486, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
Employee [empid=1008864195, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
Employee [empid=1008864195, empname=Aishwariya, city=Pune, joindate=2023-03-09 00:00:00.0]
```

#### For this application, u need to setup mysql database
- Setup mysql -u root -p
- use employees
- describe employee
- select * from employee;
- delete from employee;
