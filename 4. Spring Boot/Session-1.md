# Contents

## Module 1 : Introduction to Spring Boot
- What is Spring Boot?
- Why Spring Boot?
- Overview and Features of Spring Boot
- Introduction to Maven
- Development Environment Setup
- Getting Started with Spring Boot Application
- Ways to Spring Boot
- Setting the application properties

## Module 2 : Implementing Rest Endpoints and Spring JPA using Spring Boot
- Quick Overivew of Spring MVC
- Building a Sample REST API (GET, POST, PUT, DELETE)
- What is Spring Data JPA?
- Creating Spring Data JPA Repository & CRUD Operations
- Exploring JPA, CRUD Repositories

## Module 3 : Documenting Rest API using Swagger
- Introduction to Swagger
- Swagger Integration with Spring Boot
- Configuring Application with Swagger Annotatinos

## Module 4 : Devtools, Logging, Actuator, Security Overview
- Overview of Devtools
- Implementing Logging
- Implementing Actuator in Spring Application
- Implementing Security
- Testing Spring Application

----------------------------------------------------------------

## Overview of Spring Boot
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
- **Spring Boot** : An *opinionated* view of building Spring Applications and get you up running as quickly as possible.

### Why Spring Boot?
Spring Boot makes it easy to create **stand-alone**, **production-grade** Spring-based Applications that you can run. We take an opinionated view of the Spring platform and third-party libraries, so that you can get started with minimum fuss. Most Spring Boot applications need very little Spring configuration.

- A **R**apid **A**pplication **D**evelopment platform based on Spring
- An opinionated instance of a Spring application. **Convention over Configuration.**
- Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run"
- Most Spring Boot Applicatons need very little Spring configuration

### Features of Spring Boot
- Create stand-alone Spring applications
- Embedded Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
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

----------------------------------------------------------------------

Controller 👉 Service 👉 DAO 👉 Database

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
    - Inside **application.properties** file
    - Inside application.yml file
    - as command line switches
- Complete list of properties can be found at
    - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html

### Modifying Application Properties
#### applicationproperties.xml
```xml
server.port = 8090 # changing the port number
server.servlet.context-path=/MyApp # specifying the base path
```

------------------------------------------------

## Introducing REST
- REST : **RE**presentational **S**tate **T**ransfer
- REST was defined by *Roy Fielding*, a computer scientist.
    <img src = "../../../images/spring-boot-rest.png">
- When a RESTful API is called, the server will transfer to the client a *representation* of the state of the requested resource.
- In REST, everything is defined as '*representation of a resource*' and we can perform CRUD operation on resources using REST API
- REST key principles
    - Uniform Interface (Verbs)
    - Easy Resource Access (URI)
    - Multiple Represenations of data using MIME types (JSON/XML etc)

| Action | HTTP Method (Verbs) | Resource URIs (Nouns) | Comment |
| ------ | ------------------- | --------------------- | ------- |
| Create | POST | /employees | Example URI to create an employee, *pass the entire object* (JSON/XML) |
| Read | GET | /employees/1 OR /employees | Example URI to *retireve* an employee using say ID, for *retireving all employees* use /employees |
| Update | PUT | /employees | Example URI to update an employee, this method *needs passing entire object* (JSON/XML). For single field update, use PATCH method |
| Delete | DELETE | /employees/1 | Example URI to delete single employee using ID |

