# Session - 9 | Lambda Expressions

- One way of running our thread was to make object of a thread, create a class of interface runnable, then we override the run method.
- When this object of Thread t1 is assigned to thread
- and thread is calling the method, `t1.start()`, then this `t1.start()` is internally going to start the thread which is created by **MyRunnable**
    ```java
    //creating object of the thread
    Thread t1 = new Thread(new MyRunnable());
    //then creating a class which implements Runnable
    class MyRunnable implements Runnable {
        //overriding the run method
        @Override
        public void run() {
            System.out.println("Running in conventional way.");
        }
    }
    ```
- Another way of running our thread... is by creating **Anonymous Class**
- Here we are creating runnable but as anonymous class
    ```java
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            System.out.println("Running runnable as anonymous class.");
        }
    }
    ```

## Lambda Expression
- Lambda is a thing which is a shortcut writing for anonymous class.
- In anonymous class, we actually write the class, by implementing the given interface.
- Lambda Expression are based on interfaces ; *Functional Interface*
- **Functional Interface** can contain only one abstract method.
- If it has more than abstract methods, then it cannot be called, *Functional Interface*
- Runnable's run() does not take any parameters
- This Lambda Example does not accept any parameters 👇

### Example - 1
#### LambdaConventional.java
```java
package com.psl.lambda;

public class LambdaTestConventional {
    public static void main (String[] args) {
        //creating a class which implements runnable
        Runnable runnable1 = new MyRunnable();
        //passing the runnable to the thread
        Thread t1 = new Thread(runnable1);
        // first way
        //this is going to internally start the thread which is created MyRunnable
        t1.start();

        //another way using anonymous class
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("Running runnable as anonymous class");
            }
        };

        Thread t2 = new Thread(runnable);
        t2.start();
        //this is internally going to call the run() method
        Runnable lamRunnable = () -> System.out.println("Running runnable as Lambda");
        //Lambda Expression 👇
        Thread t3 = new Thread(
            () -> {System.out.println("Runnable runnable as Lambda");}
        );

        t3.start();

        //For Single Statement {} NOT REQUIRED
        Thread t4 = new Thread(() -> System.out.println("Running runnable as Lambda short expression"));
        t4.start();
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Running runnable in conventional way.");
    }
}
```

------------------------------------

- TreeSet gives us the SortedOrder
- But Sorted Order, cannot be done without any comparison.
- It can be done using the *Comparator*
- There were two ways of overriding the method
    1. Implement a Comparable Interface with our Entity Class
    2. Use a Comparator; which works at runtime
### Example - 2 (Anonymouc Class for Comparator)
#### LambdaComparator.java
```java
public class LambdaComparator {
    public static void main(String[] args) {
        /* ----- ANONYMOUS CLASS FOR COMPARATOR ----- */
        //this inner class👇 is going to provide instance of a comparator
        Set<Name> names = new TreeSet<>(new Comparator() {
            @Override
            public int compare(Name one, Name two) {
                //lastNames are equal👇
                if(one.lastName.compareTo(two.lastName) == 0) {
                    //if last names are equal; making comparision with firstName
                    return one.firstName.compareTo(two.firstName);
                } else {
                    return one.lastName.comparetTo(two.lastName);
                }
            }
        });

        Set<Name> names = new TreeSet<>(compLambda);

        names.add(new Name("Kavita", "Rathi"));
        names.add(new Name("Amar", "Rane"));
        names.add(new Name("Tushar", "Singh"));
        names.add(new Name("Tapi", "Mira"));
        names.add(new Name("Anthony", "Gonsolvis"));
        names.add(new Name("Akbar", "Ilahabadi"));

        System.out.println(names);
    }
}

class MyComparator implements Comparator<Name> {
    @Override
    public int compare(Name one, Name two) {
        if(one.lastName.compareTo(two.firstName) == 0) {
            return one.firstName.compareTo(two.firstName);
        } else {
            return one.lastName.compareTo(two.lastName);
        }
    }
}

class Name {
    String firstName;
    String lastName;

    public Name(String firstName, String lastName) {
        super();
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override
    public String toString() {
        StringBuilder builder = new StringBuilder();
        builder.append("FirstName: ");
        builder.append(firstName);
        builder.append(", ");
        builder.append(lastName);

        return builder.toString();
    }
}
```

#### Output
```
[Gonsolvis Anthony, Ilahabadi Akbar, Mira Tapi, Rane Amar, Rathi Kavita, Singh Tushar]
```

- The output is sorted till the last name

### Modification Version - 1 (Using Lambda for Comparator)
```java
public class LambdaComparator {
    public static void main(String[] args) {
        /* ----- LAMBDA FOR COMPARATOR ----- */
        //now the name has been tripped of from the ()
        // as it's already present inside <>
        Comparator<Name> compLambda = (one, two) -> {
            if(one.lastName.compareTo(two.lastName) == 0) {
                return one.firstName.copmareTo(two.firstName);
            } else {
                return one.lastName.compareTo(two.lastName);
            }
        };
        //lambdas can be passed as parameters to the constructor
        Set<Name> names = new TreeSet<>(compLambda);

        names.add(new Name("Kavita", "Rathi"));
        names.add(new Name("Amar", "Rane"));
        names.add(new Name("Tushar", "Singh"));
        names.add(new Name("Tapi", "Mira"));
        names.add(new Name("Anthony", "Gonsolvis"));
        names.add(new Name("Akbar", "Ilahabadi"));

        System.out.println(names);
    }
}

class MyComparator implements Comparator<Name> {
    @Override
    public int compare(Name one, Name two) {
        if(one.lastName.compareTo(two.firstName) == 0) {
            return one.firstName.compareTo(two.firstName);
        } else {
            return one.lastName.compareTo(two.lastName);
        }
    }
}

class Name {
    String firstName;
    String lastName;

    public Name(String firstName, String lastName) {
        super();
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override
    public String toString() {
        StringBuilder builder = new StringBuilder();
        builder.append("FirstName: ");
        builder.append(firstName);
        builder.append(", ");
        builder.append(lastName);

        return builder.toString();
    }
}
```

#### Output
*Same as previous*

### Modification Version - 2 (Using Conventional Insantiation for Comparator)
```java
public class LambdaComparator {
    public static void main(String[] args) {
        /* ----- CONVENTIONAL INSTANTIATION OF COMPARATOR ----- */
        Set<Name> names = new TreeSet<>(new MyComparator());

        names.add(new Name("Kavita", "Rathi"));
        names.add(new Name("Amar", "Rane"));
        names.add(new Name("Tushar", "Singh"));
        names.add(new Name("Tapi", "Mira"));
        names.add(new Name("Anthony", "Gonsolvis"));
        names.add(new Name("Akbar", "Ilahabadi"));

        System.out.println(names);
    }
}

class MyComparator implements Comparator<Name> {
    @Override
    public int compare(Name one, Name two) {
        if(one.lastName.compareTo(two.firstName) == 0) {
            return one.firstName.compareTo(two.firstName);
        } else {
            return one.lastName.compareTo(two.lastName);
        }
    }
}

class Name {
    String firstName;
    String lastName;

    public Name(String firstName, String lastName) {
        super();
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override
    public String toString() {
        StringBuilder builder = new StringBuilder();
        builder.append("FirstName: ");
        builder.append(firstName);
        builder.append(", ");
        builder.append(lastName);

        return builder.toString();
    }
}
```

#### Output
*Same as previous*

----------------------------------------------------
# Functional Interface
- Functional Interface of Comparator has two abstract methods:
    - `compare(T o1, T o2);` return type: int 👈 *compares its two arguments for order*
    - `equals(Object obj); ` 👈 *indicates whether some other object is "equal to" this comparator*

If the implementation comes from the Object class, then it is not taken into consideration..
## Categories of Functional Interface Using Lambda
- [Supplier Interface](#supplier-interface)
- [Consumer Interface](#consumer-interface)
- [Predicate Interface](#predicate-interface)
- [Function Interface](#function-interface)

## Supplier Interface
- Supplier Interface is another type of Functinoal Interface
- It has only one abstract method; `get()`
- It represents a supplier of results

#### SupplierLambda.java
```java
package com.psl.lambda;

import java.util.Map;

public class SupplierLambda {
    public static void main (String[] args) {
        Supplier<String> supplier = () -> {
            //env is key-value pair
            //lambda says it's going to check for env variables👇
            Map<String, String> env = System.getenv();
            return env.get("USERDOMAIN_ROAMINGPROFILE");
        };
        //using get() of Supplier Interface 👇
        String user = supplier.get(); 
        System.out.println(user); //PERSISTENT

        /* ------ INT SUPPLIER ------ */
        Random random = new Random();
        IntSupplier intSupplier = () -> {
           return random.nextInt();
        };
        //using getAsInt() of IntSupplier Interface 👇
        int ranInt = intSupplier.getAsInt(); 
        System.out.println("ranInt"); //1979031809
    }
}
```

## Consumer Interface
- Consumer Interface is another type of Functional Interface
- It has only one abstract method; `accept()`
- It represents an operation that accepts a single input argument and returns no result.
- Unlike most other functional interfaces, Consumer is expected to operate via side-effects.

#### ConsumerLambda.java
```java
package com.psl.lambda;

import java.util.Map;

public class ConsumerLambda {
    public static void main (String[] args) {
        //it is an acceptor 👇
        Consumer<String> consumer = (s) -> {
            System.out.println(s);
        };
        //using accept() of Consumer Interface
        consumer.accept("My Message");
    }
}
```

## Predicate Interface
- Consumer Interface is another type of Functional Interface
- It has only one abstract method; `test(T t)`
- It represents a predicate (boolean-valued) function of one argument.
- It returns a boolean type

#### PredicateLambda.java
```java
package com.psl.lambda;

import java.util.Map;

public class PredicateLambda {
    public static void main (String[] args) {
        Predicate<String> predicate = (s) -> {
            //Suresh and Harsh are qualifying for this condition👇
            return s.length() > 3; //boolean condition
        };
        //using test() of Predicate Interface
        boolean result1 = predicate.test("India");
        boolean result2 = predicate.test("JIO");
        System.out.println(result1); //true
        System.out.println(result2); //false
    }
}
```

## Function Interface
- Function Interface is another type of Functional Interface
- It has only one abstract method; `apply(T t)`
- It represents a function that accepts one argument and produces a result

#### FunctionLambda.java
```java
package com.psl.lambda;

import java.util.Map;

public class FunctionLambda {
    public static void main (String[] args) {
        //providing input as string and expecting output as integer
        //if u change the type to double, it'll mismatch
        //hence, type should be a integer 
        Function<String, Integer> function = (String s) -> {
            return s.length();
        };
        //using apply() of Function Interface
        Integer len = functino.apply("EUROPE");
        System.out.println(len); //6

        List<String> names = new ArrayList<>();
        //stream is not a collection
        //stream is like a running statment; like ResultSet
        names.add("Suresh");
        names.add("Li");
        names.add("A");
        names.add("Harsh");
        names.stream().filter(predicate).forEach(consumer); //Suresh and Harsh
    }
}
```

--------------------------------------------
