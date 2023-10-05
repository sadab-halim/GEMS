# Session - 5 (Refresher)
- [Module 1 : Introduction to Spring Boot](#module-1--introduction-to-spring-boot)
- [Module 2 : Implementing REST Endpoints and Spring Data JPA Using Spring Boot](#module-2--implementing-rest-endpoints-and-spring-data-jpa-using-spring-boot)
- [Module 3 : Documenting REST API Using Swagger](#module-3--documenting-rest-api-using-swagger)
- [Module 4 : Devtools, Logging, Actuator, Security, Testing Overview](#module-4--devtools-logging-actuator-security-testing-overview)
 
---------------------------------

# **Module 1 : Introduction to Spring Boot**

## What is Spring Boot
Likely issues with a Spring framework application
- Huge framework
- So many setup steps
- XML/Annotatinos
- Version compatability of libraries/modules

In order to reap the best of the framework, a lot has to be done <br/>
Would it not be nice to hide/abstract some of these setup steps? <br/>
That's what led to Spring Boot

### What is Spring Boot?
- **Spring** : A framework that helps you write enterprise Java applications.
- **Boot** : "Bootstrap" meaning a self-starting process that is supposed to proceed with external input.
- **Spring Boot** : An opinionated view of building Spring Applications and get you up running as quickly as possible.

### Why Spring Boot?
Spring Boot makes it easy to create **stand-alone**, **production-grade** Spring-based Applications that you can run. We take an opinionated view of the Spring platform and third-party libraries, so that you can get started with minimum fuss. Most Spring Boot applications need very little Spring configuration.

- A **R**apid **A**pplication **D**evelopment platform based on Spring
- An opinionated instance of a Spring application. **Convention over Configuration.**
- Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run"
- Most Spring Boot Applicatons need very little Spring configuration

## Overview and Features of Spring Boot
- Create stand-alone Spring applications
- Embedded **Tomcat**, **Jetty** or **Undertow** directly (no need to deploy WAR files)
- Provide opionante 'starter' dependencies to simplify your build configuration (BOM and Starters)
- Automatically configure Spring (Autoconfiguration) and 3rd party libraries whenever possible
- Provide production-ready features such as metrics, health checks, and externalized configuration
- Absolutely no code generation and no requirement for XML configuration

## Creating Spring Boot Application
### System Requirements (Spring Boot 2.7.9 RELEASE)
| Software | Version |
| -------- | ------- |
| Java | Java 8 and is compatible upto Java 19 (included) |
| Spring | Spring Framework 5.3.25 or above |
| Build Tool | Maven 3.5* or Gradile |
| IDE | STS 4.0+ / Eclipse with STS plugin / IntelliJ IDEA |

### What is Maven?
- Apache Maven is a software project management and comprehension tool.
- Based on the concept of a Project Object Model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.
- Maven aims at making the build process easy.
- Learn more: [Click here](https://maven.apache.org/)
- We can create a maven project via Eclipse
- Simplest Structore looks as follows:
    <img src = "../../../images/maven-proj-tree.png">
- Ref: [Click here](https://www.tutorialspoint.com/maven/maven_quick_guide)

### Some of the quick ways to create a Spring Boot Application:
- Using Maven Project
- **Using Spring Initializr** : https://start.spring.io/
- Spring Boot CLI (Command Line Interface)
- **Using Spring Starter Project (STS IDE)**

### Creating Spring Boot Application using Spring Initializr
- Go to https://start.spring.io/
    - Select Project Type: Maven
    - Language: Java
    - Spring Boot: 2.7.9
    - Project Metadata
        - Group:
        - Artifact:
        - Name:
        - Description:
        - Package Name:
        - Packaging:
        - Java Version:
    - Add Dependencies
        - Spring Web
        - Spring Data JPA
- Generate Project
- Import in IDE; Existing Maven Project

### Creating Spring Boot Application using STS IDE
- New --> Spring Starter Project

### Project Structure
- Create the new packages under the base package

#### pom.xml
```xml

```

#### MySpringBootAppApplication.java
```java
package com.psl;

import org.springframework.boot.SpringApplication;
//this annotation is used to mark the main class of a Spring Boot Application. 
//it encapsulates @Configuration, @EnableAutoConfiguration, 
//and @ComponentScan annotations with their default attributes
@SpringBootApplication
public class MySpringBootAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(MySpringBootAppApplication.class, args);
    }
}
```

#### HelloController.java
```java
package com.mycompany.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;
//RESTful Controller
@RestController //@RestController = @Controller + @ResponseBody
public class HelloController {
    //to hit it as a part of URI , use this annotation👇
    //@RequestMapping can also be used on the top of HelloController
    @RequestMapping(value = "/hello", method = RequestMethod.GET) //it is a GET method
    public String sayHelloWorld() { //exposing an API
        return "Hello World! Hello Spring Boot!";
    }
}
```

#### Output
<img src = "../../../images/spring-boot-hello.png" >


### Understanding Annotations
| Annotations | Description |
| ----------- | ----------- |
| @SpringBootApplication | This annotation is used to mark the main class of a Spring Boot Application. It encapsulates @Configuration, @EnableAutoConfiguration, and @ComponentScan annotations with their default attributes |
| @EnableAutoConfiguration | Enable Spring Boot's auto-configuration mechanism |
| @ComponentScan | Enable @ComponentScan on the package where the application is located | 
| @Configuration | Allow to register extra beans in the context or import additional configuration classes |

### Customizing Spring Boot Application
- To customize Spring Boot Application, properties can be specified:
    - Inside application.properties file
    - Inside application.yml file
    - as command line switches

- Complete list of properties can be found at
    - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html

If you want to write unit test cases, some convention will be followed for the test package.

---------------------------------------

# **Module 2 : Implementing REST Endpoints and Spring Data JPA Using Spring Boot**

## Introducing REST
- REST : **RE**presentational **S**tate **T**ransfer
- REST was defined by *Roy Fielding*, a computer scientist.
   <img src = "../../../images/spring-boot-rest.png">

- When a RESTful API is called, the server will transfer to the client a *representation* of the state of the requested resource.
- In REST, everything is defined as 'representation of a resource' and we can perform CRUD on resources using REST
- REST key principles
    - Uniform Interface (Verbs)
    - Easy Resource Access (URI)
    - Multiple Represenations of data using MIME types (JSON/XML etc)

| Action | HTTP Method (Verbs) | Resource URIs (Nouns) | Comment |
| ------ | ------------------- | --------------------- | ------- |
| Create | POST | /employees | Example URI to create an employee, pass the entire object (JSON/XML) |
| Read | GET | /employees/1 OR /employees | Example URI to retireve an employee using say ID, for retireving all employees use /employees |
| Update | PUT | /employees | Example URI to update an employee, this needs passing entire object (JSON/XML). For single field update, use PATCH method |
| Delete | DELETE | /employees/1 | Example URI to delete single employee using ID |

## Safety and Idempotency of HTTP Methods
- If a HTTP method does not change the representation of the resource on the server, it is **Safe.**

- If a HTTP methods does not give different outcomes, even if executed multiple times, it is **Idempotent**

| HTTP Method | Safe | Idempotent |
| ----------- | ---- | ---------- |
| GET | Yes | Yes |
| POST | No | No |
| PUT | No | Yes |
| DELETE | No | Yes |
| OPTIONS | Yes | Yes |
| HEAD | Yes | Yes |
| PATCH | No | No |

## Spring MVC & REST
- When a request come then the front-controller, the only point of contact is the **DispatcherServlet**.
- This DispatcherServlet *consults* the **handler mapping** to find out which controller is going to handle it.
- Once it has the knowledge DispatcherServlet *forwards* *it* to the *Controller*.
- It *sends the info back* to the **DispatcherServlet**, DispatchcerServlet then takes **ViewResolver**.
- DispatcherServlet *renders* the view which is visible to the browser
- **ViewResolver** takes and convert it into physical view from logical view.

    <img src = "../../../images/Spring MVC Flow.png">

## Annotations
- Spring Boot can be used for:
    - MVC Application using **@Controller** annotation
    - RESTful services using **@RestController**

- The **@Controller** annotation is used for traditional Spring controllers and has been part of the framework for a very long time.

- The **@RestController** annotation was introduced in Spring 4.0 to simplify the creation of RESTful web services. It's a convenient annotation that combines **@Controller** and **@ResponseBody** - which eliminates the need to annotate every request handling method of the controller class with the **@ResponseBody** annotation

- Other Annotations:
    - **@RequestMapping** : It is used to map the web requests. It has many optional elements like consumes, header, method, name, params, path, produces, and value. We use it with the class as well as the method.
    
    - **@GetMapping** : It maps the **HTTP GET** requests on the specific handler method. It is used to create a web service endpoint that **fetches** It is used instead of using: @RequestMapping(method = RequestMethod.GET)
    - **@PostMapping** : It maps the **HTTP POST** requests on the specific handler method. It is used to create a web service endpoint that **creates** It is used instead of using: @RequestMapping(method = RequestMethod.POST)
    - **@PutMapping** : It maps the **HTTP PUT** requests on the specific handler method. It is used to create a web service endpoint that **creates** or **updates** It is used instead of using: @RequestMapping(method = RequestMethod.PUT)
    - **@DeleteMapping** : It maps the **HTTP DELETE** requests on the specific handler method. It is used to create a web service endpoint that **deletes** a resource. It is used instead of using: @RequestMapping(method = RequestMethod.DELETE)
    - **@PatchMapping** : It maps the **HTTP PATCH** requests on the specific handler method. It is used instead of using: @RequestMapping(method = RequestMethod.PATCH)
    - **@RequestBody** : It is used to bind HTTP request with an object in a method parameter. *Internally it uses HTTP MessageConverters to convert the body of the request*. When we annotate a method parameter with @RequestBody, the Spring framework *binds the incoming HTTP request body to that parameter*
    - **@ResponseBody** :  It *binds the method return value to the response body*. It tells the Spring Boot Framework to serialize a return an object into JSON and XML format.
    - **@PathVariable** : It is *used to extract the values from the URI*. It is most suitable for the RESTful web service, where the URL contains a path variable. We can define multiple @PathVariable in a method.
    - **@RequestParam** : It is *used to extract the query parameters form the URL.* It is also known as a **query parameter**. It is most suitable for web applications. It can specify default values if the query parameter is not present in the URL.
    - **@RestController** : It can be considered as a *combination* of @Controller and @ResponseBody annotations. The @RestController annotation is itself annotated with the @ResponseBody annotation. It eliminates the need for annotating each method with @ResponseBody.

### Code with @Controller Annotation
```java
@Controller
@RequestMapping("books")
public class SimpleBookController {
    @GetMapping("/{id}", produces = "application/json")
    public @RespnoseBody Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        //...
    }
}
```

### Code with @RestController Annotation
```java
@RestController
@RequestMapping("books-rest")
public class SimpleBookController {
    @GetMapping("/{id}", produces = "application/json")
    public Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        //...
    }
}
```

### Communication between the classes in Spring Boot Application 
- RestController → Service → DAO Interface → DAO Impl → Database
- RestController → Service → Repository Interface → Database

<br/>

- From POV of Maintenance of Application, and Best Coding Practice, you should NOT communicate application directly to Database through Service Layer.
- Always follow this sequence 👇 <br/>
    Controller → Service → DAO → Database

- Each Layer has the contract:
    - **Controller** : serves the URIs, responsible for the endpoints, concern of the controller is like handling the endpoints and requests

    - **Service** : is responsible for processing the data, to filer, to do some extra work. 

    - **DAO** : is responsible for connecting to the Database and giving it back to us

- These layers helps in achieveing *Loose Coupling* in the project.
- The default implementation of JPA when it comes to Spring Boot is **Hibernate**.
- One Object will translate into one row of the relational table in the database

## Creating Project Using STS
File --> New --> Spring Starter Project
- Give Name, Choose Java Version, Give Group, Artifact Names, and Package Name
- Select Boot Version: 2.7.9
- Select Dependency --> Spring Web
- Finish
- Application is embedded at the end of the filename.
- Every other packages creates should be under the base package.

#### MySpringBootAppApplication.java
```java
package com.psl;

import org.springframework.boot.SpringApplication;

@SpringBootApplication
public class MySpringBootAppApplication {
    public static void main(String[] args) {
        SpringBootApplication.run(MySpringBootAppApplication.class, args);
    }
}
```

#### application.properties
```python
server.port=8090
server.servlet.context-path=/MyApp
```

#### Employee.java
```java
package com.psl.domain;

public class Employee {
    private Long id;
    private String name;
    private String skill;

    public Employee() {}
    public Employee(Long id, String name, String skill) {
        this.id = id;
        this.name = name;
        this.skill = skill;
    }

    public Long getId() { return id; }
    public String getName() { return name; }
    public String getSkill() { return skill; }

    public void setId(Long id) { this.id = id; }
    public void setName(String name) { this.name = name; }
    public void setSkill(String skill) { this.skill = skill; }

}
```

- to invoke the services of EmployeeService class, autowire it inside the controller class
- Controller 👉 Service 👉 DAO 👉 Database

#### EmployeeController.java
```java
package com.psl.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody; 
import org.springframework.web.bind.annotation.PostMapping;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PutMapping;

import com.psl.domain.Employee;
import com.psl.service.EmployeeService;

@RestController
public class EmployeeController {
    //this annotation is used for automatic dependency injection
    @Autowired
    private EmployeeService service;

    /* --- Create --- */
    //@RequestBody is used so that incoming requests from Postman client gets applied to the object
    //@RequestMapping(value = "/employees", method = RequestMethod.POST)
    @PostMapping("/employees") 
    public Employee addEmployee(@RequestBody Employee e) {
        return service.addEmployee(e);
    }

    /* --- Reading 1 Employee --- */
    //mapping emplooyes {id} to Long id, using the @PathVariable
    //@RequestMapping(value = "/employees/{id}", method = RequestMethod.GET)
    @GetMapping("/employees/{id}")
    //@PathVariable is used to capture values from the URL path; mapping it..
    public Employee getEmployee(@PathVariable Long id) {
        return service.getEmployee(id);
    }

    //Reading All Employees - Ideally Supplied with a Boundary Condition
    @GetMapping("/employees")
    public List<Employee> getAllEmployees() {
        return service.getAllEmployees();
    }
    /* --- Update --- */
    @PutMapping("/employees")
    //using @RequestBody annotation you will get your values mapped with the model/entity/domain class
    public Employee updateEmployee(@RequestBody Employee e) {
        return service.updateEmployee(e);
    }
    /* --- Delete --- */
    @DeleteMapping("/employees/{id}")
    public void deleteEmployee(@PathVariable Long id) {
        service.deleteEmployee(id);
    }
}
```

#### EmployeeService.java
```java
public interface EmployeeService() {
    Employee addEmployee(Employee e);
    Employee getEmployee(Long id);
    List<Employee> getAllEmployees();
    Employee updateEmployee(Employee e);
    void deleteEmployee(Long id);
}
```

#### EmployeeServiceImpl.java
```java 
package com.psl.service;

import java.util.List;
import org.springframework.stereotype.Service;
import com.mycompany.entities.Employee;

@Service
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    private EmployeeRepository repo;

    @Override
    public Employee addEmployee(Employee e) {
        return repo.save(e);
    }
        
    @Override
    public Employee getEmployee(Long id) {
        return repo.findById(id).get();
    } 

    @Override
    public List<Employee> getAllEmployees() {
        return repo.findAll();
    }

    @Override
    public Employee updateEmployee(Employee e) {
        return repo.save();
    }

    @Override
    public void deleteEmployee(Long id) {
        repo.deleteById(id);
    }
}
```

#### EmployeeRepository.java
```java
package com.mycompany.repository;
import java.util.List;

public interface EmployeeRepository extends CrudRepository<Employee, Long> {
    List<Employee> findAll();
}
```

#### EmployeeSkillDetails.java
```java
package com.mycompany.entities;

import javax.persistence.Id;
import javax.persistence.Column;

@Entity
@Table(name = "employeeskill_details") // 👈 derived name
//@Table //by default; internally 👉 @Table(name = "employee_skill_details");
public class EmployeeSkillDetails {
    @Id //Primary Key
    //indicates the persistence provider must assign primary keys
    //for the entity using a database entity column 👇
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;
    @Column(name = "detail")
    private String detail;

    public EmployeeSkillDetails(int id, String detail) {
        super();
        this.id = id;
        this.detail = detail;
    }

    public int getId() { return id; }
    public String getDetail() { return detail; }

    public int setId(int id) { this.id = id; }
    public String setDetail(String detail) { this.detail = detail; }
}
```

## Building a sample REST API (GET, POST, PUT, DELETE)
- The way inject dependencies in the classes is usinng the **@Autowired** annotation

- **@Service** specifies that it's not a normal class, it's a bean, not even a normal bean, it's a bean which has stereotype Service.

- When we write @PostMapping, we always add the body using the @RequestBody annotation.
- When we write @PutMapping, we always add the body using the @RequestBody annotation.
- When we write @GetMapping, we always add the body using the @PathVariable annotation.
- When we write @DeleteMapping, we always add the body using the @PathVariable annotation.
- In each of our operation in the Controller Class we have triggered respective services
- Since, we are using Services of Service Layer, that's the reason why we have @Autowired Service in the Controll Class
- In ORM, we rely on **@Entity** So, Java Objects are now Entities.
- The moment we introduce @Enity tag in the Enitiy class, the Spring Boot will understand that, I am trying to have a corresponding table for my Entity.
- Give a @Table (ideally recommended) 
- @Entity specifies that this class is an Entity whereas @Table it's says that it is the primary table for the annotated Entity.
- If I give @Table at the top of the Entity Object (Employee) 
- The Table by default will get created when I start the server, it'll be "employee" Table with a small e
- The frontend triggers the URI patterns.

#### application.properties
```
server.port = 8090
server.servlet.context-path = /MyApp

#MySQL Database
#spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url = jdbc:mysql://localhost:3306/empDatabase
spring.datasource.username = springuser
spring.datasource.password = ThePassword

#if it is update👇it means, once u start ur server if their is any change; it'll update id
#spring.jpa.hibernate.ddl-auto=update
#if it is update👇it means, once u start ur server, first it'll drop the table, nd then the opertions will be performed
spring.jpa.hibernate.ddl-auto=create-drop
#the statements which are visible in console, is due to this👇
spring.jpa.show-sql=true

sprindog.swagger-ui.path=/swagger-ui

management.endpoints.web.exposure.include=info,metrics,health
management.server.port=9090
management.server.base-path=/management
management.endpoints.web.base-path=/
management.endpoint.health.show-details=always

#spring.security.user.name=admin
#spring.security.user.password=Test123

logging.level.org.springframework.boot.autoconfigure.security=warn
```

## Response Status
- For **@ResponseStatus** : https://docs.io/spring-framework/docs/current/javadoc-api/springframework/web/bind/annotation/ResponseStatus.html

    ```java
    @GetMapping("/employees/{id}")
    @ResponseStatus(HttpStatus.FOUND) //👈 it'll ideally accept a response status 
    public Employee getEmployee(@PathVariable Long id) {
        return service.getEmployee(id);
    }
    ```

- For **@HTTP Status** : https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/HttpStatus.html

## Using Spring ResponseEntity to work with the HTTP Responses
- @ResponseBody puts the return value into the body of the response
- ResponseEntity allows us to add headers and status code
    - ResponseEntity represents an HTTP response, including headers, body and status.

```java
@GetMapping("/hello")
public String greeting() {
    return "Hello World";
}
```

👇

```java
@GetMapping("/hello")
public ResponseEntity<String> greetings() {
    return new ResponseEntity<String>("Hello Word", HttpStatus.OK);
}
```

For **ResponseEntity** : https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html


## What is Spring Data JPA?
- In a typical Java application, you might expect to write a class that implements EmployeeRepository.
- However, with Spring Data JPA, you need to write an implementation of the repository interface.
- Spring Data JPA creates an implementation when you run the application.

<br/>

Before:
    ```
        |-----------------|            |---------------|  
        | EmployeeDAOImpl | implements |  EmployeeDAO  |
        |     (class)     | ---------> |  (interface)  | 
        |-----------------|            |---------------|
    ```

After:
    ```
        |--------------------|            |------------------------------|  
        | EmployeeRepository |  extends   |  CRUDRepository (interface)  |
        |     (interface)    | ---------> |       OR JPARepository       | 
        |--------------------|            |------------------------------|
    ```

## Spring Boot Flow Architecture
<img src = "../../../images/Spring-Boot-Flow-Architecture.png">

## Spring Data JPA
- JPA stands for Java Persistence API
- Makes it easy to easily implement JPA based repositories
- In an application, implementing data access layer too much boilerplate code has to be written to execute simple queries.
- Spring Data JPA aims to siginficantly improve the implementation of data access layers by reducing the effort to the amount that's actually needed.
- You write your repository interfaces, and Spring will provide the implementation automatically.
- JPA is the specification.
- Hibernate is one of the implementations of JPA
- By default Hibernate is used as default ORM implementation in Spring Boot but if we want other implementations e.g. EclipseLink etc we can do that as well.

### Spring Data JPA Interfaces
- The central interface in the Spring Data repository abstraction is **Repository**
- The **CrudRepository** (I) provides sophisticated *CRUD functionality* for the entity class (Employee) that is being managed.
- On top of the CrudRepository, there is a **PagingAndSortingRepository** abstraction that adds additional *methods to ease paginated access to entities.*
- Spring also provide persistence technology-specific abstractions, such as **JPARepository** or **MongoRepository** etc.

### Settings
- With Spring Boot, add the following dependencies:
    ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
    ```

- Default Database H2
    ```xml
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
    ```

- If any other database used e.g. MySQL, use the corresponding dependency for the driver
    ```xml
        <dependency>
            <groupId>com.mysql<groupId>
            <artifactId>mysql-connetor-j</artifactId>
            <scope>runtime</scope>
        </dependency>
    ```

## Creating Spring Data JPA Repository & CRUD Operations
### Sample Repository
```java
package com.mycompany.repositories;

import java.util.List;

public interface EmployeeRepository extends CrudRepository<Employee, Long> {
    /* --- Derived Queries --- */
    List<Employee> findByNameAndSkill(String name, String skill);
}
```

### Modification Version - 1
- Adding Spring Data JPA Dependency & MySQL
    #### pom.xml
    ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
    ```

- Create database
    - `create database empDatabase;` : Creates the new database
    - `create user 'springuser'@'%' identified by 'admin';` : Creates the user
    - `grant all on empDatabase.* to 'springuser'@'%'; ` : Gives all privilges to the new user on the newly created database
- Adding properties:
    - spring.datasource.driver-class-name=com.mysql.cj.jdbc.driver
    - spring.datasource.url=jdbc:mysql://localhost:3306/empDatabase
    - spring.datasource.username = springuser
    - spring.datasource.password = admin
    - spring.data.jpa.hibernate.ddl-auto=update
    - spring.data.jpa.show-sql=true

- Annotate @Entity class as under
    ```java
    @Entity
    public class Employee {
        @Id
        @GeneratedValue(strategy=GenerationType.AUTO)
        private Long id;
        private String name;
        private String skill;
    }
    ```

### Testing using Postman
#### GET Method
<img src = "../../../images/spring-boot-emp-method-get.png">

#### POST Method
<img src = "../../../images/spring-boot-emp-method-post.png">

#### Inside MySQL Workbench
<img src = "../../../images/spring-boot-mysql-workbench.png">

#### POST Method (2nd Request)
<img src = "../../../images/spring-boot-emp-method-post2.png">

#### GET Method(ID: 1)
<img src = "../../../images/spring-boot-emp-method-get-emp1.png">

#### GET Method(ID: 2)
<img src = "../../../images/spring-boot-emp-method-get-emp2.png">

#### Console 
These are the queries being interally fired by the Spring Boot Application 👇 <br/>
<img src = "../../../images/spring-boot-quries-being-internally-fired.png">

#### PUT Method (Modification)
Postman 👇 <br/>
<img src = "../../../images/spring-boot-emp-method-put.png">

SQL Workbench 👇 <br/>
<img src = "../../../images/spring-boot-emp-method-put-sql.png">

-------------------------------------------------------

## Spring Data JPA - Other Query Methods
- Derived Query Methods
    - Spring Data JPA also lets you define other query methods by declaring their method signature.
    - E.g. findByLastName, findByNameEquals, findByNameIsNot, findByNameEndingWith, findByNameLike, findByAgeBetween, findByNameOrderByNameAsc

- Named Query : Use @NamedQuery annotation to specify a named query within an entity class and then declare that method in repository
    ```java
    @Entity
    @Table(name = "Employee")
    @NamedQuery(name = "Employee.findBySkill",
    query = "SELECT e from Employee where e.skill = ?1")
    public class Employee {
        @Id
        @GeneratedValue(strategy = GenerationType.AUTO)
        private Long id;
    }
    ```

- Custom Queries using @Query : It is denoted by @Query
    1. Simple @Query
        - Ex: @Query("SELECT e FROM Employee e") <br/>
              List<Employee> findAllEmployees()

    2. Using JPQL (Java Persistence Query Language) :
        - doesn't know the table names
        - it doesn't understand the name of the tables, which has been given explicitly
        - it only understand the Java object, in our case (*Employee*)
        - it doesn't works with SQL types of syntax
        - it works with objects
        - because of ORM (Object Relation Mapping)
        - whereas SQL doesn;t know the Java name, it know the table name.

    3. Using Native Query :
        - in native query we have to specify with a boolean flag

<img src = "../../../images/springboot-spring-data-jpa.png">
  
In the console we are getting the Hibernate query, because in the application.properties we have configured that, show-ddl = true

To See CriteriaQuery and Querydsl 👉 https://spring.io/blog/2011/04/26/advanced-spring-data-jpa-specifications-and-querydsl

## Running Spring Boot App Outside the IDE
- There are two ways to run the Spring Boot App (by navigating to command prompt at the application level)
    1. mvn spring-boot:run
    2. java -jar/targetapp.jar

### Creating Jar File
Run as --> Maven Build --> Goals: clean package 
<img src = "../../../images/spring-boot-jar-generated.png">

## Adding DevTools Dependency
Copy the dependencies from Spring Initializr

----------------------------------------------------------

## Final Code : Part - 1

#### Project Structure
<img src = "../../../images/spring-boot-folder-structure-1.png">

#### pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.12</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.psl</groupId>
	<artifactId>MySpringBootApp</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>MySpringBootApp</name>
	<description>Demo project for Spring Boot</description>
	
	<properties>
		<java.version>1.8</java.version>
	</properties>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		
		<dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-data-jpa</artifactId>
    	</dependency>

   		<dependency>
        	<groupId>com.mysql</groupId>
        	<artifactId>mysql-connector-j</artifactId>
        	<scope>runtime</scope>
    	</dependency>
    	
    	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
    	
    	<dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
        </dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

#### MySpringBootApplication.java
```java
package com.psl;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootAppApplication {
	public static void main(String[] args) {
		SpringApplication.run(MySpringBootAppApplication.class, args);
	}
}
```

#### HelloController.java
```java
package com.psl.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

//RESTful Controller
@RestController //@RestController = @Controller + @ResponseBody
public class HelloController {	
	@RequestMapping(value = "/hello", method = RequestMethod.GET)
	public String sayHelloWorld() {
		return "Hello World! Hello Spring Boot!";
	}
}
```

#### EmployeeController.java
```java
package com.psl.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody; 
import org.springframework.web.bind.annotation.PostMapping;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PutMapping;

import com.psl.domain.Employee;
import com.psl.service.EmployeeService;

@RestController
public class EmployeeController {
	
	@Autowired
	private EmployeeService service;
	//Create
//	@RequestMapping(value = "/employees", method = RequestMethod.POST)
	@PostMapping(value = "/employees")
	public Employee addEmployee(@RequestBody Employee e) {
		return service.addEmployee(e);
	}
	//Read 1 Employee
//	@RequestMapping(value = "/employees/{id}", method = RequestMethod.GET)
	@GetMapping(value = "/employees/{id}")
	public Employee  getEmployee(@PathVariable Long id) {
		return service.getEmployee(id);
	}
	//Read all employees - ideally supplied with a boundary condition
		@GetMapping("employees")
		public List<Employee>  getAllEmployees() {
			return service.getAllEmployees();
		}
	//Update
	@PutMapping(value = "/employees")
	public Employee updateEmployee(@RequestBody Employee e) {
		return service.updateEmployee(e);
	}	
	//Delete
	@DeleteMapping(value = "/employees/{id}")
	public void deleteEmployee(@PathVariable Long id) {
		
	}
}
```

#### EmployeeDAO.java
```java
package com.psl.dao;

import java.util.List;

import org.springframework.data.repository.CrudRepository;
import com.psl.domain.Employee;

public interface EmployeeDAO extends CrudRepository<Employee, Long>{	
	/* --- Derived Queries --- */
	List<Employee> findAll();
}
```

#### Employee.java
```java
package com.psl.domain;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
//@Table("employeeTable")
public class Employee {
	@Id //Primary Key
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	//@Column
	private String name;
	private String skill;
	
	public Employee() {}
	public Employee(Long id, String name, String skill) {
		this.id = id;
		this.name = name;
		this.skill = skill;
	}
	
	public Long getId() { return id; }
	public String getName() { return name; }
	public String getSkill() { return skill; }
	
	public void setId(Long id) { this.id = id; }
	public void setName(String name) { this.name = name; }
	public void setSkill(String skill) { this.skill = skill; }
	
}
```

#### EmployeeService.java
```java
package com.psl.service;

import java.util.List;
import com.psl.domain.Employee;

public interface EmployeeService {
	Employee addEmployee(Employee e);
	Employee getEmployee(Long id);
	List<Employee> getAllEmployees();
	Employee updateEmployee(Employee e);
	void deleteEmployee(Long id);
}
```

#### EmployeeServiceImpl.java
```java
package com.psl.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.psl.dao.EmployeeDAO;
import com.psl.domain.Employee;

@Service
public class EmployeeServiceImpl implements EmployeeService {

	@Autowired
	private EmployeeDAO dao;
	
	@Override
	public Employee addEmployee(Employee e) {
		return dao.save(e);
	}

	@Override
	public Employee getEmployee(Long id) {
		return dao.findById(id).get();
	}

	@Override
	public List<Employee> getAllEmployees() {
		return dao.findAll();
	}

	@Override
	public Employee updateEmployee(Employee e) {
		return dao.save(e);
	}

	@Override
	public void deleteEmployee(Long id) {
		dao.deleteById(id);
	}
}
```

#### application.properties
```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/empDatabase
spring.datasource.username=springuser
spring.datasource.password=admin

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```
---------------------------------------------------------  

## Final Code : Part - 2
### Project Structure
<img src = "../../../images/spring-boot-proj-structure-using-swagger.png">

#### pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.12</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.psl</groupId>
	<artifactId>MySpringBootApp</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>MySpringBootApp</name>
	<description>Demo project for Spring Boot</description>
	
	<properties>
		<java.version>1.8</java.version>
	</properties>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		
		<dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-data-jpa</artifactId>
    	</dependency>

   		<dependency>
        	<groupId>com.mysql</groupId>
        	<artifactId>mysql-connector-j</artifactId>
        	<scope>runtime</scope>
    	</dependency>
    	
    	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
    	
    	<dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
        </dependency>
        
        <dependency>
    		<groupId>org.springdoc</groupId>
    		<artifactId>springdoc-openapi-ui</artifactId>
    		<version>1.6.15</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

#### application.properties
```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/empDatabase
spring.datasource.username=springuser
spring.datasource.password=admin

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

#### MySpringBootApplication.java
```java
package com.psl;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootAppApplication {
	public static void main(String[] args) {
		SpringApplication.run(MySpringBootAppApplication.class, args);
	}
}

```

#### Employee.java
```java
package com.mycompany.entities;

import javax.persistence.Id;
import javax.persistence.GenerationType;

@Entity
//@Table("employeeTable")
public class Employee {
    @Id //--> Primary Key
    @GeneratedValue(Strategy = GenerationType.AUTO)
    private Long id; //--> JPA took this as field name, and made the cols
    private String name; //--> JPA took this as field name, and made the cols
    private String skill; //--> JPA took this as field name, and made the cols
    //@Column(skill = "skill") --> we can also explicity say that this is a col 👇
    //private String skill;

    public Employee() {}
    public Employee(Long id, String name, String skill) {
        this.id = id;
        this.name = name;
        this.skill = skill;
    }

    public Long getId() { return id; }
    public String getName() { return name; }
    public String getSkill() { return skill; }

    public void setId(Long id) { this.id = id; }
    public void setName(String name) { this.name = name; }
    public void setSkill(String skill) { this.skill = skill; }

}
```

#### HelloController.java
```java
package com.psl.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

//RESTful Controller
@RestController //@RestController = @Controller + @ResponseBody
@Tag(name = "Hello API")
public class HelloController {
	
	@RequestMapping(value = "/hello", method = RequestMethod.GET)
	public String sayHelloWorld() {
		return "Hello World! Hello Spring Boot!";
	}
}
```

#### EmployeeController.java
```java
package com.mycompany.controller;

import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody; 
import org.springframework.web.bind.annotation.PostMapping;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PutMapping;

import com.psl.domain.Employee;
import com.psl.service.EmployeeService;

import io.swagger.v3.oas.annotations.Hidden;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.tags.Tag;

@RestController
@Tag(name = "Employee REST API")
public class EmployeeController {

    @Autowired
    private EmployeeService service;

     /* ------ Create ------ */
    //@RequestMapping(value = "/employees", method = RequestMethod.POST)
    @PostMapping("/employees")
    @Hidden //hides it; the summary nd the description won't be visible in the browser
    @Operation(summary = "Adds an employee", description = "Takes an employee object")
    public @ApiResponse(description = "Returns an Employee object") Employee saveEmployee(@Parameter(description = "Employee object") @RequestBody Employee e) {
        System.out.println("Saving employee");
        service.saveEmployee(e);
    	return e;
    }

    /* ------  Read ------ */
    //@RequestMapping(value = "/employees/{id}", method = RequestMethod.GET)
    @GetMapping("/employees/{id}") //this type of URL pattern should be unique.
    @Operation(summary = "Retrieves an employee", description = "Takes the employee's id")
    public @ApiResponse (description = "Returns the Employee object based on the id") Employee getEmployee(@Parameter(description = "ID of the employee is ")  @PathVariable Long id) {
        System.out.println("Getting Employee");
    	return service.getEmployee(id);
    }

    /* ------  Update ------ */
    @PutMapping("/employees")
    public Employee updateEmployee(@RequestBody Employee e) {
    	System.out.println("Updating Employees");
        service.updateEmployee(e);
        return e;
    }

    /* ------  Delete ------ */
    @DeleteMapping("/employees/{id}")
    public void deleteEmployee(@PathVariable Long id) {
    	System.out.println("Deleting employee with id " + id);
        service.deleteEmployee(id);
    }

    @GetMapping("/employees")
    public List<Employee> getAllEmployees() {
        return service.getAllEmployees();
    }

    @GetMapping("/employees/name/{name}") 
    public List<Employee> getAllEmployeesWithNameAndSkill(@PathVariable String name, @RequestParam String skill) {
        return service.getAllEmployeesWithNameAndSkill(name, skill);
    }
}
```

#### EmployeeService.java
```java
package com.psl.service;

import java.util.List;
import com.psl.domain.Employee;

public interface EmployeeService {
    Employee saveEmployee(Employee e);
    Employee getEmployee(Long id);
    Employee updateEmployee(Employee e);
    
    void deleteEmployee(Long id);

    List<Employee> getAllEmployees();
    List<Employee> getAllEmployeesWithNameAndSkill(String name, String skill);
//    List<Employee> getAllEmployeeWithName(String name);
}

```

#### EmployeeServiceImpl.java
```java
package com.psl.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.psl.dao.EmployeeDAO;
import com.psl.domain.Employee;

@Service
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    private EmployeeDAO repo;
    
    @Override
	public Employee saveEmployee(Employee e) {
		repo.save(e);
		return e;
	}
    
    @Override
    public Employee getEmployee(Long id) {
        return repo.findById(id).get();
    } 
    
    @Override
    public Employee updateEmployee(Employee e) {
        repo.save(e);
        return e;
    }
    
    @Override
    public void deleteEmployee(Long id) {
        repo.deleteById(id);
    }
    
    @Override
    public List<Employee> getAllEmployees() {
    	//return (List<Employee>repo.findAll();
        return repo.findAllEmployees();
    }
    
    @Override
    public List<Employee> getAllEmployeesWithNameAndSkill(String name, String skill) {
        return repo.findByNameAndSkill(name, skill);

//        return repo.findEmployeeByNameAndSkillIndexJpql(name, skill);
//        return repo.findEmployeeByNameAndSkillNamedJpql(name, skill);

//         return repo.findEmployeeByNameAndSkillIndexNative(name, skill);
//         return repo.findEmployeeByNameAndSkillNamedNative123(name, skill);
    }
}
```

#### EmployeeRepository.java
```java
package com.mycompany.repository;

import java.util.List;

import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.CrudRepository;
import org.springframework.data.repository.query.Param;

import com.psl.domain.Employee;

public interface EmployeeRepository extends CrudRepository<Employee, Long> {
    /* --- Derived Queries --> (findBy) --- */
    Employee findByName(String name);
    List<Employee> findByNameAndSkill(String name, String skill);
    //------------------------------------------------------------
    /* --- Custom Queries --- */
    /* --- 1. Simple Query --- */
    @Query("SELECT e FROM Employee e")
    List<Employee> findAllEmployees();
    //------------------------------------------------------------
    /* --- 2. Using JPQL Query --- */
      //a. with indexed parameters
      /*
        - e.name = ?1, this ?1 represents index no 1
        - e.skill = ?2, this ?2 represents index no 2
      */
      @Query("SELECT e FROM Employee e WHERE e.name = ?1 and e.skill = ?2")
      List<Employee> findEmployeeByNameAndSkillIndexJpql(String name, String skill);
      //b. with named parameters
      /*
        - e.name = :name1, this represents index no 1 for name
        - e.skill = :skill1, this represents index no 1 for skill
      */
      @Query("SELECT e FROM Employee e WHERE e.name = :name1 and e.skill = :skill1")
      /*
        - @Param is used for named parameters; it will take the value, & will be used in the JPQL query..
        - 👇this method accepts name and skill, and it'll come from : Service --> Controller --> URI
      */
      List<Employee> findEmployeeByNameAndSkillNamedJpql(@Param("name1") String name, @Param("skill1") String skill);

    /* --- 3. Using Native Query --- */
      //a. with indexed parameters
      @Query(value = "SELECT * FROM Employee WHERE name = ?1 AND skill = ?2", nativeQuery = true)
      List<Employee> findEmployeeByNameAndSkillIndexNative(String name, String skill);

      //b. with named parameters 
      @Query(value = "SELECT * FROM Employee WHERE name = :name AND skill = :skill", nativeQuery = true)
      List<Employee> findEmployeeByNameAndSkillNamedNative123(@Param("name") String name, @Param("skill") String skill);
}
```

# **Module 3 : Documenting REST API Using Swagger**
## Overview of Swagger
### What is Swagger?
- The design and documentation platform for teams and individuals working with the OpenAPI Specificaiton.
- Swagger makes API design a breeze, with easy-to-use tools for developers, architects, and product owners.

### Why Swagger?
Nowadays, front-end and back-end components often separate a web application. Usually, we expose APIs as a back-end component for the front-end component or third-party app integrations.

In such a scenario, it is essential to have proper specifications for the back-end APIs. At the same time, the API documentation should be informative, readabale and easy to follow.

Reference documentation should simulatenously describe every change in the API. Accoumplishing this manually is a tedious exercise, so automation of the process was inevitable.

Swagger is one of the solution for this.

### Implementing Swagger with Spring Boot
- It will give us:
    - **Swapper API Docs** : It'll read our controllers and give it'll give us the docs
    - **Swagger UI** : This will help you to try or test the APIs, something similar to Postman
    
```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.6.15</version>
</dependency>
```

### Generating Documentatin Using Postman
<img src = "../../../images/spring-boot-postman-swagger-docs.png">

### Generating Documentation of our Project in the Web
The UI of complete Project has been generated by appending the `swagger-ui/index.html` to the base url

<img src = "../../../images/swagger-ui-1.png">
<img src = "../../../images/swagger-ui-2.png">

For better understanding of the UI part watch → *timestamp: 1:10:00 - Sesssion: 3* <br/>
For customization → Adding `springdoc.swagger-ui.path=/swagger-ui.html` in application.properties file 

### Annotations!
- **@OpenAPIDefinition** : used to populate OpenAPI Object fields info, tags, servers, security and externalDocs If more than one class is annotated with OpenAPIDefinition , with the same fields defined, behaviour is inconsistent.

- **@Tag** : The annotation may be applied at class or method level to define tags for the single operation (when applied at method level) or for all operations of a class (when applied at class level).
- **@Operation** : used to define a resource method as an OpenAPI Operation, and/or to define additional properties for the Operation.
- **@Parameter** : used on a method parameter to define it as a parameter for the operation, and/or to define additional properties for the Parameter.
- **@ApiResponse** :  used to describe possible success and error codes from your REST API call.
- **@Hidden** : Marks a given resource, class or bean type as hidden, skipping while reading / resolving

See 👉 https://springdoc.org/#migrating-from-springfox

### Customizing OpenAPI Definition
In order to customize OpenAPI Definition, making changes to the SpringBootApplication file

#### SpringBootApplication.java
```java
package com.psl;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import io.swagger.v3.oas.annotations.OpenAPIDefinition;
import io.swagger.v3.oas.annotations.info.Info;

@SpringBootApplication
//used for customize the OpenAPI Definition
@OpenAPIDefinition(info = @Info(title = "Employee CRUD REST API", version = "1.0", description = "Sample API"))
public class MySpringBootAppApplication {

	public static void main(String[] args) {
		SpringApplication.run(MySpringBootAppApplication.class, args);
	}
}
```

#### From **OpenAPI Definition** changed to **Employee CRUD REST API**
<img src = "../../../images/swagger-ui-3.png">

---------------------------------------------------

# **Module 4 : Devtools, Logging, Actuator, Security, Testing Overview**

## Overview of Spring Boot Devtools
- An additional set of tools that can make the application development experience a little more pleasant.
- The spring-boot-devtools module can be included in any project to provide additional development-time features.
- To include devtools support, add the module dependency to your build:
    ```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
    ```

- Developer tools are automatically disabled when running a fully packaged application (i.e. application launched from java -jar or if it is started from a special classloader)
- System property to control devtools 👉 `spring.devtools.restart.enabled`
- Applications that use spring-boot-devtools automatically restart whenever files on the classpath change.
- To exclude resources that do not necessarily need to trigger a restart when they are changed, use a property in application.properties e.g. `spring.devtools.restart.exclue=static/**,public/**`
- You can disable restart feature by using the property `spring.devtools.restart.enable` (available as system property as well as in application.propertied)
- Use a Trigger file to control restarts
    e.g. in application.properties, if we set this property `spring.devtools.restart.trigger-file=.reloadtrigger`, restarts will now only happen when the src/main/resources/.reloadtrigger is updated.

Ref → https://docs.spring.io/spring-boot/docs/2.7.9/reference/html/using.html#using.devtools

## Actuator
An **actuator** is a manufacturing term that refers to a mechanical device for *moving* or *controlling something*. <br/>
Actuators can generate a large amount of motion from a small change.

To add the actuator to a Maven-based project, add the following 'Starter' dependency:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### Endpoints
- Actuator endpoints let you monitor and interact with your application. Spring Boot includes a number of built-in endpoints and lets you add your own.

- Each individual endpoint can be enabled or disabled. By default, all endpoints except for `shutdown` are enabled.

- To configure the enablement of an endpoint, use its management.endpoint<id>.enabled property; for example to enable shutdown endpoint we can configure property as follows 👉 management.endpoint.shutdown.enabled=true

- To remotely access an endpoint, it needs to be exposed via JMX or HTTP (**Caution**: Since Endpoints may contain sensitive information, you should carefully consider when to expose them. Use Spring Security to secure the endpoints)

    By default, on HTTP, only, 'health' endpoint is exposed and is mapped to '/actuator/health'

Following properties can be used to control the exposure of endpoints
| Property | Default |
| -------- | ------- |
| management.endpoints.jmx.exposure.include | |
| management.endpoint.jmx.exposure.include | * |
| management.endpoints.web.exposue.exclude | |
| management.endpoints.web.exposure.include | health |

### Testing the Actuator using Postman
<img src = "../../../images/spring-boot-actuator-postman.png">

Actuator endpoints let you monitor and interact with your application. Spring Boot includes a number of built-in endpoints and lets you add your own. For example, the `health` endpoint provides basic application health information.

You can **enable** or **disable** each individual endpoint and *expose* them (make them remotely accessible) over HTTP or JMX. <br/>
An endpoint is considered to be available when it is both enabled and exposed. <br/>
The built-in endpoints are auto-configured only when they are available.

Most applications choose exposure over HTTP, where the ID of the endpoint and a prefix of `/actuator` is mapped to a URL. <br/>
For example, by default, the `health` endpoint is mapped to `/actuator/health`

#### Modifying application.properties
Adding `management.endpoints.web.exposure.include=info,metrics,health` in application.properties file

<img src = "../../../images/spring-boot-actuator-postman-info.png"> <br/>
<img src = "../../../images/spring-boot-actuator-postman-metrics.png">

Adding these things 👇 in the application.properties file
```
management.server.port = 9090
management.server.base-path=/management
```

#### Before
<img src = "../../../images/spring-boot-actuator-management-before.png">

#### After
<img src = "../../../images/spring-boot-actuator-management-after.png">

If we don't want the /actuator thing, then 👇 (url has been shorten) <br/>

Adding `management.endpoints.web.base-path=/` in application.properties file <br/>
<img src = "../../../images/spring-boot-actuator-management-removed-actuator.png">

After incorporating this thing 👇 in application.properties file <br/>
`management.endpoint.health.show-details=always`

It shows something diff in the json format in comparison to what it was showing previously 👇
<img src = "../../../images/spring-boot-actuator-management-health-always.png">

If we remove the endpoint, then this is the output we get 👇
<img src = "../../../images/spring-boot-actuator-management-removed-endpoints.png">

### Updating the build plugin inside the pom.xml file
#### pom.xml
```xml
.
.
.
<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<executions>
					<execution>
						<goals>
							<goal>build-info</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>
.
.
.
```

Security is must when you deal with any such endpoints which are exposed

------------------------------

## Spring Security
- Spring Security provides comprehensive support for authentication, authorization, and protection against common exploits. 
- Authentication is how we verify the identity of who is trying to access a particular resource. A common way to authenticate users is by requiring the user to enter a username and password. Once authentication is performed we know the identity and can perform authorization.

- If Spring Security is on the classpath, then web applications are secured by default.
- Security is enabled for all the MVC/REST endpoints in the application.
- The default UserDetailsService has a single user. The user name is 'user'.
- The password is random and is printed at INFO level when the application starts.
- Alternatively, you can configure users in application.properties files with the following properties 👉 spring.security.user.name and spring.security.user.password
- More commonly, you can configure a class that extends WebSecurityConfigurerAdapter and override the 'configure' method.
- See also 👉 https://spring.io/guides/gs/securing-web/

Adding Spring Dependency in pom.xml file
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

Dependency for testing security
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope></scope>
</dependency>
```

When we enter the url as `http://localhost:8080/hello` it redirects to `http://localhost:8080/login` bcoz it's secured now, it was achieved by using **Spring Security**

Login Page 👇 <br/>
<img src = "../../../images/spring-boot-login-page.png">

The default username of the login page is **user** whereas the password is given in the console 👇 <br/>
<img src = "../../../images/spring-boot-security-password.png">

After logging in with the correct credentials, it'll show the inner material, i.e., the output 👇
<img src = "../../../images/spring-boot-security-logged-in.png">

Even if we try accessing the metrics endpoint or the info endpoint, still we'll get the login page, it means that our spring boot application is completely secured, and can be accessed by only providing the correct credentials, and that is shared only with the developers in the console, and this login page is for just development purpose, not production use.

If we want to logout after seeing the contents of the web page, then all we need to do is to type logout at the end of the url, for example `http://localhost:8080/logout` (base-url/logout)

Then, it'll show a confirmation page for clicking on the log out button 👇 <br/>
<img src = "../../../images/spring-boot-security-log-out.png">

Page after logging out 👇 <br/>
<img src = "../../../images/spring-boot-security-logged-out.png">


--------------------------------

## Final Code : Part - 3
### Project Structure
<img src = "../../../images/spring-boot-proj-structure-using-swagger.png">

#### pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.12</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.psl</groupId>
	<artifactId>MySpringBootApp</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>MySpringBootApp</name>
	<description>Demo project for Spring Boot</description>
	
	<properties>
		<java.version>1.8</java.version>
	</properties>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		
		<dependency>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		
		<dependency>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
		
		<dependency>
   	 		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-test</artifactId>
   			 <scope></scope>
		</dependency>
		
		<dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-data-jpa</artifactId>
    	</dependency>

   		<dependency>
        	<groupId>com.mysql</groupId>
        	<artifactId>mysql-connector-j</artifactId>
        	<scope>runtime</scope>
    	</dependency>
    	
    	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
    	
    	<dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
        </dependency>
        
        <dependency>
    		<groupId>org.springdoc</groupId>
    		<artifactId>springdoc-openapi-ui</artifactId>
    		<version>1.6.15</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<executions>
					<execution>
						<goals>
							<goal>build-info</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>

</project>

```

#### application.properties
```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/empDatabase
spring.datasource.username=springuser
spring.datasource.password=admin

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

springdoc.swagger-ui.path=/swagger-ui.html

management.endpoints.web.exposure.include=info,metrics,health
management.server.port = 9090
management.server.base-path=/management
management.endpoints.web.base-path=/
management.endpoint.health.show-details=always
```

#### MySpringBootApplication.java
```java
package com.psl;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import io.swagger.v3.oas.annotations.OpenAPIDefinition;
import io.swagger.v3.oas.annotations.info.Info;

@SpringBootApplication
//used for customize the OpenAPI Definition
@OpenAPIDefinition(info = @Info(title = "Employee CRUD REST API", version = "1.0", description = "Sample API"))
public class MySpringBootAppApplication {

	public static void main(String[] args) {
		SpringApplication.run(MySpringBootAppApplication.class, args);
	}
}


```

#### Employee.java
```java
package com.mycompany.entities;

import javax.persistence.Id;
import javax.persistence.GenerationType;

@Entity
//@Table("employeeTable")
public class Employee {
    @Id //--> Primary Key
    @GeneratedValue(Strategy = GenerationType.AUTO)
    private Long id; //--> JPA took this as field name, and made the cols
    private String name; //--> JPA took this as field name, and made the cols
    private String skill; //--> JPA took this as field name, and made the cols
    //@Column(skill = "skill") --> we can also explicity say that this is a col 👇
    //private String skill;

    public Employee() {}
    public Employee(Long id, String name, String skill) {
        this.id = id;
        this.name = name;
        this.skill = skill;
    }

    public Long getId() { return id; }
    public String getName() { return name; }
    public String getSkill() { return skill; }

    public void setId(Long id) { this.id = id; }
    public void setName(String name) { this.name = name; }
    public void setSkill(String skill) { this.skill = skill; }

}
```

#### HelloController.java
```java
package com.psl.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import io.swagger.v3.oas.annotations.tags.Tag;

//RESTful Controller
@RestController //@RestController = @Controller + @ResponseBody
@Tag(name = "Hello API")
public class HelloController {
	
	@RequestMapping(value = "/hello", method = RequestMethod.GET)
	public String sayHelloWorld() {
		return "Hello World! Hello Spring Boot!";
	}
}

```

#### EmployeeController.java
```java
package com.mycompany.controller;

import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody; 
import org.springframework.web.bind.annotation.PostMapping;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PutMapping;

import com.psl.domain.Employee;
import com.psl.service.EmployeeService;

import io.swagger.v3.oas.annotations.Hidden;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.tags.Tag;

@RestController
@Tag(name = "Employee REST API")
public class EmployeeController {

    @Autowired
    private EmployeeService service;

     /* ------ Create ------ */
    //@RequestMapping(value = "/employees", method = RequestMethod.POST)
    @PostMapping("/employees")
    @Hidden //hides it; the summary nd the description won't be visible in the browser
    @Operation(summary = "Adds an employee", description = "Takes an employee object")
    public @ApiResponse(description = "Returns an Employee object") Employee saveEmployee(@Parameter(description = "Employee object") @RequestBody Employee e) {
        System.out.println("Saving employee");
        service.saveEmployee(e);
    	return e;
    }

    /* ------  Read ------ */
    //@RequestMapping(value = "/employees/{id}", method = RequestMethod.GET)
    @GetMapping("/employees/{id}") //this type of URL pattern should be unique.
    @Operation(summary = "Retrieves an employee", description = "Takes the employee's id")
    public @ApiResponse (description = "Returns the Employee object based on the id") Employee getEmployee(@Parameter(description = "ID of the employee is ")  @PathVariable Long id) {
        System.out.println("Getting Employee");
    	return service.getEmployee(id);
    }

    /* ------  Update ------ */
    @PutMapping("/employees")
    public Employee updateEmployee(@RequestBody Employee e) {
    	System.out.println("Updating Employees");
        service.updateEmployee(e);
        return e;
    }

    /* ------  Delete ------ */
    @DeleteMapping("/employees/{id}")
    public void deleteEmployee(@PathVariable Long id) {
    	System.out.println("Deleting employee with id " + id);
        service.deleteEmployee(id);
    }

    @GetMapping("/employees")
    public List<Employee> getAllEmployees() {
        return service.getAllEmployees();
    }

    @GetMapping("/employees/name/{name}") 
    public List<Employee> getAllEmployeesWithNameAndSkill(@PathVariable String name, @RequestParam String skill) {
        return service.getAllEmployeesWithNameAndSkill(name, skill);
    }
}
```

#### EmployeeService.java
```java
package com.psl.service;

import java.util.List;
import com.psl.domain.Employee;

public interface EmployeeService {
    Employee saveEmployee(Employee e);
    Employee getEmployee(Long id);
    Employee updateEmployee(Employee e);
    
    void deleteEmployee(Long id);

    List<Employee> getAllEmployees();
    List<Employee> getAllEmployeesWithNameAndSkill(String name, String skill);
//    List<Employee> getAllEmployeeWithName(String name);
}

```

#### EmployeeServiceImpl.java
```java
package com.psl.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.psl.dao.EmployeeDAO;
import com.psl.domain.Employee;

@Service
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    private EmployeeDAO repo;
    
    @Override
	public Employee saveEmployee(Employee e) {
		repo.save(e);
		return e;
	}
    
    @Override
    public Employee getEmployee(Long id) {
        return repo.findById(id).get();
    } 
    
    @Override
    public Employee updateEmployee(Employee e) {
        repo.save(e);
        return e;
    }
    
    @Override
    public void deleteEmployee(Long id) {
        repo.deleteById(id);
    }
    
    @Override
    public List<Employee> getAllEmployees() {
    	//return (List<Employee>repo.findAll();
        return repo.findAllEmployees();
    }
    
    @Override
    public List<Employee> getAllEmployeesWithNameAndSkill(String name, String skill) {
        return repo.findByNameAndSkill(name, skill);

//        return repo.findEmployeeByNameAndSkillIndexJpql(name, skill);
//        return repo.findEmployeeByNameAndSkillNamedJpql(name, skill);

//         return repo.findEmployeeByNameAndSkillIndexNative(name, skill);
//         return repo.findEmployeeByNameAndSkillNamedNative123(name, skill);
    }
}
```

#### EmployeeRepository.java
```java
package com.mycompany.repository;

import java.util.List;

import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.CrudRepository;
import org.springframework.data.repository.query.Param;

import com.psl.domain.Employee;

public interface EmployeeRepository extends CrudRepository<Employee, Long> {
    /* --- Derived Queries --> (findBy) --- */
    Employee findByName(String name);
    List<Employee> findByNameAndSkill(String name, String skill);
    //------------------------------------------------------------
    /* --- Custom Queries --- */
    /* --- 1. Simple Query --- */
    @Query("SELECT e FROM Employee e")
    List<Employee> findAllEmployees();
    //------------------------------------------------------------
    /* --- 2. Using JPQL Query --- */
      //a. with indexed parameters
      /*
        - e.name = ?1, this ?1 represents index no 1
        - e.skill = ?2, this ?2 represents index no 2
      */
      @Query("SELECT e FROM Employee e WHERE e.name = ?1 and e.skill = ?2")
      List<Employee> findEmployeeByNameAndSkillIndexJpql(String name, String skill);
      //b. with named parameters
      /*
        - e.name = :name1, this represents index no 1 for name
        - e.skill = :skill1, this represents index no 1 for skill
      */
      @Query("SELECT e FROM Employee e WHERE e.name = :name1 and e.skill = :skill1")
      /*
        - @Param is used for named parameters; it will take the value, & will be used in the JPQL query..
        - 👇this method accepts name and skill, and it'll come from : Service --> Controller --> URL
      */
      List<Employee> findEmployeeByNameAndSkillNamedJpql(@Param("name1") String name, @Param("skill1") String skill);

    /* --- 3. Using Native Query --- */
      //a. with indexed parameters
      @Query(value = "SELECT * FROM Employee WHERE name = ?1 AND skill = ?2", nativeQuery = true)
      List<Employee> findEmployeeByNameAndSkillIndexNative(String name, String skill);
      //b. with named parameters 
      @Query(value = "SELECT * FROM Employee WHERE name = :name AND skill = :skill", nativeQuery = true)
      List<Employee> findEmployeeByNameAndSkillNamedNative123(@Param("name") String name, @Param("skill") String skill);
}
```

---------------------------------------


## Overview of Spring Boot Logging
- Spring Boot uses Commons Logging for all internal logging but leaves the underlying log implementation open.
- Default configurations are provided for Java Util Logging, Log4J2, and Logback.
- By default, if you use the "Starters" (spring-boot-starter-web), Logback is used for logging.
- The default log configuration echoes messages to the console as they are written.
    By default, ERROR-level, WARN-level, and INFO-level messages are logged.
- Color coding is configured using the `%clr` conversion word. In its simplest form the converter will color the ouput according to the log level.

The following table describes the mapping of log levels to colors:
| Level | Color |
| ----- | ----- |
| `FATAL` | Red |
| `ERROR` | Red |
| `WARN` | Yellow |
| `INFO` | Green |
| `DEBUG` | Green |
| `TRACE` | Green |


The following colors and styles are supported:
- blue
- cyan
- faint
- green
- magenta
- red
- yellow

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@RestController
public class HelloWorldController {
    private final Logger LOGGER = LoggerFactory.getLogger(this.getClass());

    @GetMapping("/helloWorld")
    public String sayHelloWorld() {
        LOGGER.debug("This is a debug message..");
        LOGGER.info("This is an info message..");
        LOGGER.warn("This is a warn message..");
        LOGGER.error("This is an error message..");

        return "Hello World";
    }
}
```

When this code is executed, notice that default log level is info. <br/>
It logs Info, Warn, Error (for logback, Fatal maps to Error)

- The previous behaviour can be controlled:
    - Temporarily by passing log level on command line or VM settings
        - e.g. For command line 👉 mvn spring-boot:run -Dspring-boot.run.arguments=--logging.level.org.springframeowrk=TRACE
    - Permanently by passing via application.properties
        - e.g. logging.level.root = DEBUG
    - Via Logback.xml
        - When a file on the classpath has one of the following name, Spring Boot will automatically load it over the default configuration:
            - logback-spring.xml
            - logback.xml
            - logback-spring.groovy
            - logback.groovy

- Some more settings
    - application.properties
        logging file.name and logging.file.path
    - Logging patterns
        - #Logging pattern for the console: logging.pattern.console=%d{yyy-MM-dd HH:mm:ss} - %msg%n

        - #Logging patter for file: logging.patter.file=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n

Ref 👉 https://docs.spring.io/spring-boot/docs/2.7.9/reference/html/features.html#features.logging

### Sample logback.xml
If we want to disable console logging and write output only to a file, we need a custom logback-spring.xml that imports file-appender.xml byt not console-appender.xml, as shown in the following example"

The logback-spring.xml file could be added in 'resource' folder with following content.

<img src = "../../../images/jks-sample-logback.png">

You need to add logging.file.name to our application.properties or application.yaml: logging.file.name=myapplication.log

Ref 👉 https://docs.spring.io/spring-boot/docs/2.7.9/reference/htmlsingle/#howto.logging.logback


## Overview of Spring Boot Testing
The spring-boot-starter-test "Starter" (in the test scope) contains the following provided libraries:
- JUnit 5: The de-fato standard for unit testing Java applications.
- Spring Test & Spring Boot Test: Utilities and Integratino Test support for Spring Boot Applications
- AsserJ: A fluent assertion library
- Hamcrest: A library of matcher objects (also known as constrains or predicates)
- Mockito: A Java mocking framework
- JSONassert: An assertion library for JSON
- JsonPath: XPath for JSON

### Some Spring Boot Testing Annotations
- `@SpringBootTest`
    - Can be used as an alternative to the standard spring-test @ContextConfiguration annotation when you need Spring Boot features/
    - By default it will not start as a server
    - Also sets up a mock environment for testing web endpoints
    - With Spring MVC, we can query our web endpoints using MockMvc or WebTestClient

- `@DataJpaTest`
    - Used to test repository
    - Annotation for a JPA test that focuses only on JPA components.
    - Using this annotation will disable full auto-configuration and instead apply only configuration relevant to JPA tests
    - By default, tests annotated with @DataJpaTest are transactional and roll back at the end of each test.
    - 

- `@WebMvcTest`
    - Used to test controller
    - Annotation that can be used for a Spring MVC test that focuses only on Spring MVC components.
    - Using this annotation will disable full auto-configuration and instead apply only configuration relevant to MVC tests
    - By default, tests annotated with @WebMvcTest will also auto-configure Spring Security
    - Typically @WebMvcTest is used in combination with @MockBean or @Import to create any collaborators required by your @Controller beans.

- `@MockBean`
    - Used to add mocks to a Spring ApplicationContext.
    - The mock will replace any existing bean of the same type in the application context.
    - If no bean of the same type is defined, a new one will be added.
    - This annotation is useful in integration tests where a particular bean, like an external service, needs to be mocked.

----------------------------------------------------

# **From Guiding Session**
## Spring Boot Introduction
- Spring is widely used for creating scalable applications. For web applications SPring provie Spring MVC.
- Spring MVC is a widely used module of spring which is used to create scalable web applications.
- But main **disadvantages** of spring projects is that configuration is really time-consuming and can be a bit overwhelming for the new developers.
- Solution to this is Spring Boot. Spring Boot is built on the top of the spring and contains all the features of spring. And is becoming favourite of develoepr's these days because of it's a rapid production-ready environment which enables the developers to directly focus on the logic instead of struggling with the configuration and set up.
- Spring Boot is a microservice-based framework and making a production-ready application in it takes very less time.

## Spring Boot Features
- Spring Boot is built on the top of the conventional spring framewoek. So, it provides all the features of spring and is yet easier to use than spring.
- It allows us to avoid heavy configuration of XML which is present in spring.
- Unlike the Spring MVC Project, in spring boot everything is auto-configured. We just need to use proper configuration for utilizing a particular functionality. For example: If we want to use hibernate (ORM) then we can just add @Table annotation above model/entity class and add @Column annotation to map it to table and columns in the database.
- It provides easy maintenance and creation of REST end points.
    Creating a REST API is very easy in Sprin gBooot. Just the annotation @RestController and @RequestMapping(/endPoint) over the controller class does the work.
- It includes embedded Tomcat server: Unlike Spring MVC project where we hwave to manually add and install the tomcat server, Spring Boot comes with an embedded Tomcat server, so that the applications can be hosted on it.

- Deployment is very easy, war and jar file can be easily deployed in the tomcat server:
    war or jar files can be direcly deployed on the Tomcat Server and Spring Boot provides the facility to convert our project into war or jar files. Also, the instance of Tomcat can be run on the cloud as well.

- Microservice Based Architecture:
    Microservice as the name suggests is the name given to a module/service which focuses on a single type of feature, exposing an API. Let us consider an example of a hospital management system:
        - In case of monolithic systems, there will be a single code containing all the features which are very tough to maintain on a huge scale.
        - But in the microservice-based system, each feature can be divided in to smaller subsystems like service to handle patient registration, service to handle database management, service to handle billing etc.
    
    Microservice based system can be easily migrated as only some services need to be altered which also makes debugging and deployment easy. Also, each service can be integrated and can be made in different technologies suited to them.

## Spring Boot Architecture
- Layers in Spring Boot: There are four main layers in Spring Boot:
    - **Presentatin Layer** : It consists of views (i.e., frontend part)
    - **Data Access Layer** : CRUD (create, retrieve, update, delete) operations on the database comes under this category.
    - **Service Layer** : This consist of service classes and uses services provided by data access layers.
    - **Integration Layer** It consits of web different web services (any service available over the internet and uses XML messaging system)

- Then we have utility classes, validator classes and view classes
- All the services provided by the classes are implemented in their corresponding classes and are retrieved by implementing the dependency on those interfaces.

## Annotation
- @Retention
- @Target

<img src = "../../../images/spring-boot-hierarchy-of-annotation.png">

