# Session - 8 | JDBC

- JDBC stands for Java Database Connectivity.
- It is a bridge between Java and Database.
- Java commands are going to be converted in SQL Commands
- Database are independent.
- In a typical web application, in general there are three layers:
    - **Presentation Layer** : Components of UI, Any Browser Specific code lies here, talks about how the data should be represented to the users.
    - **Business Layer** : Domain Model, Business Application Classes, Java code lies here
    - **Persistence Layer** : Some of the objects we may require even if our server goes down, we want them permanently. It's job is to make it persistent. Database lies here. <br/>
    ```
    |--------------|     |--------------|     |--------------|
    |              |     |              |     |              |
    | Presentation | --> | Business Mod | --> |  Persistence |
    |              |     |              |     |              |
    |--------------|     |--------------|     |--------------|
    ```
- Their needs to be an interface between Business Layer and Persistence Layer, as the *Business Layer is written in Java*, and *Persistence Layer is written in SQL* (Database Language).
    - So, the Java code gets converted into SQL commands in the Persistence Layer.   
    - Because, database only understands, Database Language.
    - And because they are independent of java (programming language), it can be used with any other language.
    - But their need to be some *converter*, and that work is being done by **JDBC Driver**
- JDBC class or the JDBC code is written in Java by the vendor who is providing database.
- RDBMS takes care of relationship between entities.
- Whatever classes we get in the domain model, many times those becomes tables in the RDBMS.

--------------------------------------------------
- Now, for getting a database utility, *to connect the database*, we get something called as **driverClass**.
- This **driverClass** is basically a specific class which talks about a specific vendor's database `com.mysql.cj.jdbc.Driver`.
- `com.mysql.cj.jdbc.Driver` is a fully qualified name.
- **Driver** is the class of that interface which is provided by that vendor.
- When the class gets loaded, it gets register to the database server.
- The class is registered, and now it's available to use.
- a mysql-connector-j jar file is required to connect the database and run it.
- Add mysql-connector-j JAR File in your libraries.
- Inside our main method we have a driverClass, which we are loading → `String driverClass = "com.mysql.cj.jdbc.Driver";`
- `Class.forName()` throws an Exception → ClassNotFoundException which is a checked exception
- It is not a good idea to throw ClassNotFoundException, because what will happen is, in the browser itself it will throw Class Not Found and it will give that class
- So we catch it separately with the try block, and then we throw a new exception by embedding a message.
    ```java
    package com.persistent.jdbc;

    public class JDBCTest {
        public static void main(String[] args) throws ApplicationException {
            String driverClass = "com.mysql.cj.jdbc.Driver";
            try {
                //Step 1: telling the JVM to load the class
                Class.forName(driverClass); 
                System.out.println("Class Loaded");
            } catch (ClassNotFoundException c) { //exception occured while loading the class
                throw new ApplicationException("Cannot load Driver class : " + driverClass);
            }
        }
        .
        .
        .
    }
    ```
- Handling the exception through the try block and throwing it in our custom exception
- If I want to write any custom Checked Excpetion, then I have to extend it from Exception
- If I want write an exception from Unchecked Exception, then what we'll do ? 
    - extend it from RuntimeException

- Throwable means another exception which I can embedd inside it.
    ```java
    public class ApplicationException extends Exception {
        public ApplicationException(String messsage, Throwable embeddedException) {
            super(message, embeddedException);
        }
        public ApplicationException(String message) {
            super(message);
        }
    }
    ```

#### Getting/Obtaining a Connection
- How do we obtain a connection? → call the driver class
- Remeber such a primitve code (hardcode) is never written in production level (user and password)
- `getConnection()` is responsible for getting the instance of the connection
- this `getConnection()` method has multiple overloaded methods.

- this DriverManager class, is from `java.sql.DriverManager`, this is not a interface it's a class and it has got `getConnection()` method.

- `commit` is a mechanism, it says, whatever changes have been taken place, commit it to the database.

- `getConnection()` attempts to establish a connection of the given database URL. 
- The **DriverManager** attempts to select an appropriate driver from the set of registered devices

- **Connection** is an *interface* and on that reference, we can only call those methods which is defined in the interface, **setAutoCommit()** is one of those methods.
- It is a good to practice to create multiple methods and achieve readability.
- The Connection method, needs to be **private**, because it needs to be used privately.

    ```java
    //Obtaining a Connection
    private static Connection getConnection() throws ConnectionException {
        //protocol:subprotocol(db_vendor)://localhost/db_name
        String DB_URL = "jdbc:mysql://localhost/students";
        final String USER = "root";
        final String PASSWORD = "admin";
            
        try {
            Connection connection = DriverManager.getConnection(DB_URL, USER, PASSWORD);
            //checking if it's good or not; working as a rollback
            connection.setAutoCommit(false);
            return connection;
        } catch (SQLException sqle) {
            throw new ConnectionException("Connection could not be obtained.");
        }
    }
    ```

- You can create database using MySQL
- `getConnection()` is called inside a try block
- Whenever we say `getConnection()` we are supposed to close the connection also.
- the resources which are created inside the try brackets, are automatically released, when the code gets out of the scope; hence connection closed.
- Once we get the connection, we should create a statement (Step 3), the statement which needs to be get executed.
- Statement is an interface
- Once we get statement, we call executeUpdate() on the sql.

- Why executeUpdate()?
    - Because this statement is going to be executed, and it will not return anything, it'll just create the database.

- What will happen, if database is not created due to some reason?
    - It does not have any other way other than throwing an exception, because it's not returning any value
- If the code fails, it is going to throw a SQLException and this SQLExcpetion is a checked exeption

- Earlier what we had to do is in the finally block, we used to close the connection
- Now, we are doing that using the try with resources, inside the try block itself.

    ```java
    public static void createDatabase() throws ApplicationException, ConnectionException {
        //describing what sql query we want to fire
        String sql = "CREATE DATABASE IF NOT EXISTS EMPLOYEE";
        //try with resources example 👇
        try (Connection connection = getConnection();
             //Step 2
             Statement statement = connection.createStatement();) {
             //getting a statement by calling the createStatement()
             //it is going to create a database for us
             statement.executeUpdate(sql);
             System.out.println("Database created successfully..");
        } catch (SQLException sqle) {   //and if it's not able to create, it'll throw exception
            throw new ApplicationException("Create database failed", sqle);
        }
    }
    ```

- Once, we have created database. Now, we'll create Table

    ```java
    public static void createTable() throws ApplicationException, ConnectionException {
        String tableName = "REGISTRATION";
        //creating a sql query by appending it using StringBuilder
        String builder = new StringBuilder("CREATE TABLE IF NOT EXISTS");
        builder.append(tableName);
        builder.append("(id INTEGER not NULL, ");
        builder.append(" first VARCHAR(255), ");
        builder.append(" last VARCHAR(255), ");
        builder.append(" age INTEGER, ");
        builder.append("PRIMARY KEY (id))");
        String sql = builder.toString();

        //gets a connection
        try (Connection connection = getConnection();
            //creates a statement
            Statement statement = connection.createStatement();) {
            //executeUpdate() hands over this sql query to the database
            //and database makes sense out of that query
            statement.executeUpdate(sql);
            System.out.println("Table created successfully..");
        } catch (SQLException sqle) {   //if it catches sqle, we create our own exception
            throw new ApplicationException("Could not create the table " + tableName, sqle); //embedding the exception which was thrown; more details
        }
    }
    ```

- this method 👇, insert no values, while making a preparedStatement
- PreparedStatement is a child interface of a statemnent
- the ? are the placeholders for the parameters
- these ? are in the sequence
- these indexes start from 1 and NOT 0
- PreparedStatement is a more concrete interface
- We use preparedStatement instead of Statement because they get compiled and they are **reused** by SQL Server.
- setInt, setString these methods, gives type to it.
- `executeUpdate()` returnes either (1) the row count for SQL DML statements or (2) 0 for SQL statements that return nothing.
- `connection.commit()` says to the database, whatever we have committed till now, commit it to the database; since, we have switched off setAutoCommit() 

    ```java
    public static void insertUsingPreparedStatement() throws ApplicationException, ConnectionException {
        //typical sql commmand
        String query = "Insert into registration values (?,?,?,?)";
        try (Connection connection  = getConnection();
            PreparedStatement pStatement = connection.prepareStatement(query);) {  
                //assigning values using preparedstatement      
                pStatement.setInt(1,104);
                pStatement.setString(2, "Geeta");
                pStatement.setString(3, "Senthil");
                pStatement.setInt(4,24);
                //executeUpdate returns a int value
                int i = pStatement.executeUpdate();
                System.out.println(i + "records inserted");
                connection.commit(); 
        } catch (SQLException sqle) {
            //concatenating the messaging and sending the actual exception that occurred
            throw new ApplicatonException("Could not insert during prepared statement - " + sqle.getMessage(), sqle);   
        }
    }
    ```

- ResultSet is a kind of data structure itself, and it has its own iterator
- rs is pointing to one data

    ```java
    public static void fetchDetails() throws ApplicationException, ConnectionException {
        String sql;
        sql = "SELECT id, first, last, age FROM REGISTRATION";
        try (Connection connection = getConnection();
             Statement statement = connection.createStatement();
             //executeQuery() fetches the data and returns something..
             ResultSet rs = statement.executeQuery(sql);) {
             //iterating over the resultset data structure (rs) --> collection of all the fetched data
            while(rs.next()) {
                //retrieving by column name
                int id = rs.getInt("id");
                String firstName = rs.getString("first");
                String lastName = rs.getString("last");
                int age = rs.getInt("age");
                //displays value
                System.out.println(id + firstName + " - " + lastName + " - " + age);
            }
        } catch(SQLException sqle) {
            throw new ApplicatoinException("Select statement faild", sqle);
        }
    }
    ```
- our custom exception; **ConnectionException.java**
- we can even inherit our own exceptions from user defined exceptions

    ```java
    package com.persistent.jdbc;

    public class ConnectionException extends Exception {
        public ConnectionException(String arg0) {
            super(arg0);
        }
    }
    ```

- storedProcedure are not generally created by JDBC in general.

    ```java
    public static void createStoredProcedure() throws ApplicationException, ConnectionException {
        StringBuilder builder = new StringBuilder();
        //appending sql queries
        builder.append("CREATE PROCEDURE MYPROC(IN id INT, IN first VARCHAR(20), IN last VARCHAR(20), IN age INT)");
        builder.append("\n\tBEGIN");
        builder.append("\n\tINSERT INTO REGISTRATION VALUES(id, first, last, age);");
        builder.append("\n\tEND");

        String query = builder.toString();
        System.out.println(query);

        try (Connection connection = getConnection();
            Statement statement = connection.createStatement();) {
            System.out.println("BEFORE STORED PROCEDURE");
            statement.executeUpdate(query);
            System.out.println("Stored Procedure Created..");
        } catch(SQLException sqle) {
            throw new ApplicationException("Could not created procedure", sqle);
        }
    }
    ```

- insertUsingStoredProcedure()
- we are using execute() because it's a **CallableStatement**
- In general, it is a good practice is to never write a business code in the stored procedure
    - Because the business layer which is responsible for capturing all the business needs 
    - Only if a small part is implemented in database, the stored procedure will go along the database
    - To avoid this situation, generally business rules are never written in stored procedures.
    - Business rules should be in the business layer

    ```java
    public static void insertUsingStoredProcedure() throws ApplicationException, ConnectionException {
        String query = "CALL MYPROC(?,?,?,?)";
        try (Connection connection = getConnection();
            CallableStatement cStatement = connection.prepareCall(query);) {
                //setting the parameters
                cStatement.setInt(1,203);
                cStatement.setString(2, "Shivnath");
                cStatement.setString(3, "Tuteja");
                cStatement.setInt(4, 45);
                cStatement.execute();
                System.out.println(" records inserted through stored procedure");
        } catch (SQLException sqle) {
            throw new ApplicationException("Insert using stored procedure failed", sqle);
        }
    }
    ```

- `updateUsingPreparedStatement()` is almost similar to `insertUsingStoredProcedure()`
- the only diff in update.. we are using `executeUpdate()` as it's a PreparedStatement
- but in insert.. we have used `execute()` since it was a CallableStatement
- only for SELECT QUERY, executeQuery() is used
    ```java
    public static void updateUsingPreparedStatement() throws ApplicationException, ConnectionException {
        String query = "update registration set first = ? where id = ?";
        try (Connection connection = getConnection();
            PreparedStatement pStatement = connection.prepareStatement(query);) {
                pStatement.setString(1, "Ramesh");
                pStatement.setInt(2, 101);

                int i = pStatement.executeUpdate();
                System.out.println(i + " records updated");
        } catch (SQLException sqle) {
            throw new ApplicationException("updateUsingPreparedStatment failed", sqle);
        }
    }
    ```

- similar to createTable() their is a dropTable

    ```java
    public static void dropTable() throws ApplicationException, ConnectionException {
        String tableName = "REGISTRATION";
        String sql = "DROP TABLE" + tableName;

        try (Connection connection = getConnection();
            Statement statement = connection.createStatement();) {
            statement.executeUpdate(sql);
            System.out.println("Table dropped successfully..");
        } catch(SQLException sqle) {
            throw new ApplicationException("Could not drop the table " + tableName, sqle);
        }
    }
    ```

- dropProcedure()

    ```java
    public static void dropProcedure() throws ApplicationException, ConnectionException {
        String sql = "DROP PROCEDURE MYPROC";
        try (Conneciton connection = getConnection();
            Statement statement = connection.createStatemet();) {
            statement.executeUpdate(sql);
            System.out.println("Procedure dropped successfully..");
        } catch (SQLException sqle) {
            throw new ApplicationException("Drop a procedure failed ", sqle);
        }
    }
    ```

- dropDatabase()

    ```java
    public static void dropDatabase() throws ApplicationException, ConnectionException {
        String sql = "DROP DATABASE EMPLOYEES";
        try (Connection connection = getConnection();
            Statement statement = connection.createStatement();) {
            statement.executeUpdate(sql);
            System.out.println("Database dropped successfully..");
        } catch (SQLException sqle) {
            throw new ApplicationException("Drop database failed", sqle);
        }
    }
    ```
----------------------------------------------------------------
## Full Code of JDBC
#### JDBCTest.java
```java
import java.sql.CallableStatment;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCTest {
    public static void main(String[] args) throws ApplicationException {
        String driverClass = "com.mysql.cj.jdbc.Driver";
        try {
            Class.forName(driverClass);
            System.out.println("Class Loaded");
        } catch (ClassNotFoundException e1) {
            throw new ApplicationExceotion("Cannot load driver class - " + driverClass);
        }

        try {
            // drop();
            create();
        } catch (ApplicationException appEx) {
            System.out.print(appEx.getMessage());
            Throwable cause = appEx.getCause();
            if(cause != null) {
                throw new ApplicationException("Cannot create database." + "\tDETAILS: " + cause.getMessage());
            }
        }
    }

    //all the methods related to creating database, and perform operations related to it
    public static void create() throws ApplicationException, ConnectionException {
        createDatabase();
        createTable();
        fetchDetails();

        insertUsingPreparedStatement();
        updateUsingPreparedStatement();
        fetchDetails();
        
        createStoredProcedure();
        insertUsingStoredProcedure();
        fetchDetails();
    }

    //all the methods related to dropping database, and perform operations related to it
    public static void drop() throws ApplicationException, ConnectionException {
        dropTable();
        dropDatabase();
        dropProcedure();
    }

    //Obtaining a Connection
    private static Connection getConnection() throws ConnectionException {
        String DB_URL = "jdbc:mysql://localhost/students";
        final String USER = "root";
        final String PASSWORD = "admin";

        try {
            Connection connection = DriverManager.getConnection(DB_URL, USER, PASSWORD);
            connection.setAutoCommit(false);
            return connection;
        } catch (SQLException sqle) {
            throw new ConnectionException("Connection could not be obtained");
        }
    }

    //Creating Database
    public static void createDatabase() throws ApplicationException, ConnectionException {
        String sql = "CREATE DATABASE IF NOT EXISTS EMPLOYEES ";
        try (Connection connection  = getConnection();
             Statement statement = connection.createStatement();) {
            
            statement.executeUpdate(sql);
            System.out.println("Database created successfully..");
        } catch (SQLException sqle) {
            throw new ApplicationException("Create database failed ", sqle);
        }
    }

    //Updating Database
    public static void updateUsingPreparedStatement() throws ApplicationException, ConnectionException {
        String query = "update registration set first = ? where id = ?";
        try (Connection connection = getConnection();
             PreparedStatement pStatement = connection.prepareStatement(query);) {
                pStatement.setString(1, "Ramesh");
                pStatement.setInt(2, 104);

                int i = pStatement.executeUpdate();
                connection.commit();
                System.out.println(i + " records updated");
        } catch (SQLException sqle) {
            throw new ApplicationException("updaeUsingPreparedStatement failed ", sqle);
        }
    }

    //Dropping Table
    public static void dropTable() throws ApplicationException, ConnectionException {
        String tableName = "REGISTRATION";
        String sql = "DROP TABLE " + tableName;

        try (Connection connection = getConnection();
             Statement statement = connection.createStatement();) {
             
             statement.executeUpdate(sql);
             System.out.println("Table dropped successfully..");
        } catch (SQLException sqle) {
            throw new ApplicationException("Could not drop the table " + tableName, sqle);
        }
    }

    //Dropping Procedure
    public static void dropProcedure() throws ApplicationException, ConnectionException {
        String sql = "DROP PROCEDURE MYPROC";
        try (Connection connection = getConnection();
             Statement statement = connection.createStatemenet();) {
             statement.executeUpdate(sql);
        } catch (SQLException sqle) {
            throw new ApplicationException("Drop procedure failed ", sqle);
        }
    }
    
    //Dropping Database
    public static void dropDatabase() throws ApplicationException, ConnectionException {
        String sql = "DROP DATABASE EMPLOYEES";
        try (Connection connection = getConnection();
             Statement statement = connection.createStatement();) {
             
             statement.executeUpdate(sql);
             System.out.println("Database dropped successfully..");
        } catch (SQLException sqle) {
            throw new ApplicationException("Drop database failed", sqle);
        }
    }
}
```

#### Output [drop()]
```
Class Loaded
Table dropped successfully..
Database dropped successfully..
Procedure dropped successfully..
```

#### Output [create()]
```
Class Loaded
Database created successfully..
Table created successfully..
1 records inserted
0 records inserted
104Geeta - Senthil - 24
CREATE PROCEDURE MYPROC(IN id INT, IN first VARCHAR(20), IN last VARCHAR(20), IN age INT)
    BEGIN
    INSERT INTO REGISTRATION VALUES(id, first, last, age);
    END
BEFORE STORED PROC
Stored procesdure created..
 records inserted through stored procedure
104Geeta - Senthil - 24
```