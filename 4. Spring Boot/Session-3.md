# Session - 3

## Spring Data JPA - Other Query Methods
- Derived Query Methods
    - Spring Data JPA also lets you define other query methods by declaring their method signature.
    - E.g. findByLastName, findByNameEquals, findByNameIsNot, findByNameEndingWith, findByNameLike, findByAgeBetween, findByNameOrderByNameAsc

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

### Settings
#### With Spring Boot, add the following dependency
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

#### Default Database H2
```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

#### If any other database used e.g., MySQL, use the corresponding dependency for the driver
```xml
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <scope>runtime</scope>
</dependency>
```

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

-----------------------------------

### Performing Tests using Postman







-------------------------------------

# Documenting REST API using Swagger
## Overview of Swagger
### What is Swagger?
- The design and documentation platform for teams and individuals working with the OpenAPI Specificaiton.
- Swagger makes API design a breeze, with easy-to-use tools for developers, architects, and product owners.

### Why Swagger?
Nowadays, front-end and back-end components often separate a web application. Usually, we expose APIs as a back-end component for the fron-end component or third-party app integrations.

In such a scenario, it is essential to have proper specifications for the back-end APIs. At the same time, the API documentation should be informative, readabale and easy to follow.

Reference documentation should simulatenously describe every change in the API. Accoumplishing this manually is a tedious exercise, so automation of the process was inevitable.

Swagger is one of the solution for this.

### Implementing Swagger with Spring Boot
- It will give us:
    - Swapper API Docs : It'll read our controllers and give it'll give us the docs
    - Swagger UI : This will help you to try or test the APIs, something similar to Postman
    
```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.6.15</version>
</dependency>
```

### Generating Documentatin Using Postman
<img src = "../../../images/spring-boot-postman-swagger-docs.png">

```json
{
    "openapi": "3.0.1",
    "info": {
        "title": "OpenAPI definition",
        "version": "v0"
    },
    "servers": [
        {
            "url": "http://localhost:8080",
            "description": "Generated server url"
        }
    ],
    "paths": {
        "/employees": {
            "get": {
                "tags": [
                    "Employee REST API"
                ],
                "operationId": "getAllEmployees",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/Employee"
                                    }
                                }
                            }
                        }
                    }
                }
            },
            "put": {
                "tags": [
                    "Employee REST API"
                ],
                "operationId": "updateEmployee",
                "requestBody": {
                    "content": {
                        "application/json": {
                            "schema": {
                                "$ref": "#/components/schemas/Employee"
                            }
                        }
                    },
                    "required": true
                },
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/Employee"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/hello": {
            "get": {
                "tags": [
                    "hello-controller"
                ],
                "operationId": "sayHelloWorld",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/employees/{id}": {
            "get": {
                "tags": [
                    "Employee REST API"
                ],
                "summary": "Retrieves an employee",
                "description": "Takes the employee's id",
                "operationId": "getEmployee",
                "parameters": [
                    {
                        "name": "id",
                        "in": "path",
                        "description": "ID of the employee is ",
                        "required": true,
                        "schema": {
                            "type": "integer",
                            "format": "int64"
                        }
                    }
                ],
                "responses": {
                    "default": {
                        "description": "Returns the Employee object based on the id",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/Employee"
                                }
                            }
                        }
                    }
                }
            },
            "delete": {
                "tags": [
                    "Employee REST API"
                ],
                "operationId": "deleteEmployee",
                "parameters": [
                    {
                        "name": "id",
                        "in": "path",
                        "required": true,
                        "schema": {
                            "type": "integer",
                            "format": "int64"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "OK"
                    }
                }
            }
        },
        "/employees/name/{name}": {
            "get": {
                "tags": [
                    "Employee REST API"
                ],
                "operationId": "getAllEmployeesWithNameAndSkill",
                "parameters": [
                    {
                        "name": "name",
                        "in": "path",
                        "required": true,
                        "schema": {
                            "type": "string"
                        }
                    },
                    {
                        "name": "skill",
                        "in": "query",
                        "required": true,
                        "schema": {
                            "type": "string"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/Employee"
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    },
    "components": {
        "schemas": {
            "Employee": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "integer",
                        "format": "int64"
                    },
                    "name": {
                        "type": "string"
                    },
                    "skill": {
                        "type": "string"
                    }
                }
            }
        }
    }
}
```

### Generating Documentation of our Project in the Web
The UI of complete Project has been generated by appending the `swagger-ui/index.html` to the base url

<img src = "../../../images/swagger-ui-1.png">
<img src = "../../../images/swagger-ui-2.png">

For better understanding of the UI part watch 👉 *timestamp: 1:10:00 - Sesssion: 3* <br/>
For customization 👉 Adding `springdoc.swagger-ui.path=/swagger-ui.html` in application.properties file 

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

---------------------------------------------------------------------

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

------------------------------------------------

## Spring Security
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


----------------------------------------------------

## Final Code

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

