# Session - 5 | Spring MVC

- **Presentation Layer** : 
    - takes care of how the data needs to be presented for the client.
    - Typical clients are machines, wherever human beings are going to use it.
    - Human being is going to interact with the browser
    - and on the browser, when the human being clicks a certai link
    - it is going to be redirected to the presentation layer.
    - now, the presentation layer will make a sense out of it.
    - and it'll call the business layer.
    - Typically, whenever presentation layer sends it response to the client/browser, it contains a data, with html tags.
    - The code which gets executed in the browsers comes in two forms:
        - HTML
        - JavaScript
    - Rendering is done in the browser

- **Business Layer** : 
    - Presentation Layer and Business layer will make a communicatio of request and response in between.
    - From browser to the business layer, there is one more layer in between which is going to come,
    - the components which run in that layer are server, servlets, jsp pages
    - JSP/HTML pages will work in the server side.
    - 

Database --> Persistence Layer --> Business Layer --> Service --> DAO --> Hibernate (ORM) --> Database

----------------------------

**MVC** stands for **M**odel, **V**iew, **C**ontroller
- **View** is going to be the interface b/w users and controllers.
- Controller will pass on the message to different services.
- Services will take help of AccountDAO and other things.
- Controller classes do not belong to business domain.

## All Annotations (Spring)
- **@Configuration** it says this is my configuration class, it indicates that the class has @Bean definition methods. So Spring container can process the class, and generate Spring Beans to be used in the app 
- **@Bean** it defines this method is kind of factory method, it is a method-level anntotation. it is applied on a method to specify that it returns a bean to be managed by Spring context /helps to execute the queries, if the method name is different than what we have given we need to describe it as a bean
- **@Autowired** :  is used for automatic dependency injection. Spring framework is built on dependency injection and we inject the class dependencies through spring bean configuration file.
- **@Qualifier** : is used to differentiate a bean among the same type of bean objects. If we have more than one bean of the same type and want to wire only one of them then use the @Qualifier annotation along with @Autowired to specify which exact bean will be wired.
- **@Repository** : used to indicate that the class provides the mechanism for storage, retrieval, search, update and delete operation on objects.
- **@Service** : is used to mark the class as a service provider. So overall @Service annotation is used with classes that provide some business functionalities. Spring context will autodetect these classes when annotation-based configuration and classpath scanning is used.
- **@ComponentScan** : it specifies that from basePackages it goes to every package of data acess, and scans every annotations which are given for the entities, service, dao
- **@PropertySource** : it is used to provide properties file to Spring Environment.this annotation is used with @Configuration classes
- **@Value** :  is used to assign default values to variables and method arguments
it reads from the properties file, and set it here

- **@Entity** : specifies that the class is an entity and is mapped to a database table
- **@Table** :  mapping table with Java Object , (name = "employee") --> optional, if u want to give another name to ur table other than the class name.by default, it takes the class name, in the camelCase format. 
specifies the name of the database table to be used for mapping
- **@Id** : indices the member field below is a primary key of the current entity

- **@EnableAspectJAutoProxy** :  enables support for handling components marked with AspectJ's @Aspect, Spring AOP Proxy Support

- **@Component** : when we declare this as component, then spring goes to this class and automatically derives its attributes and convert it into a bean  it allows Spring to detect our custom beans automatically

- **@Aspect** : declares the class as an aspect
