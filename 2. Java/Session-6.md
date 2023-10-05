# Session - 6 | Multithreading

## Threads
- Threads work asynchronously.
- When two threads run, they run in the same instance of JVM; they have same memory space, and they can access each other.
- A thread is a class in Java, and we can write our custom Thread by extending from Thread using the `extends` keyword.
- A thread to run in a system we'll have to override the behavior of thread
- I'll have to override the methods of thread, there are two methods; run() and start()
- The code below, cannot be called as running a thread, it is more of a running a class. It's WRONG ❌

## Types of Creating Thread
- using Runnable
- extending Thread Class
## Extending Thread Class 
#### ThreadSimpleClient.java
```java
public class ThreadSimpleClient {
	public static void main(String[] args) {
		//creating the object of thread
		MyThread mt = new MyThread();
		//calling the run method of MyThread using its object
		//however this is WRONG ❌
        mt.run();
	}
}
```

#### MyThread.java
```java
public class MyThread extends Thread {
	@Override
	public void run() {
		System.out.println("In run method");
	}
	@Override
	public synchronized void start() {
		Systerm.out.println("In start method");
        super.start();
	}
}
```

#### Output
```
In run method
```

- if we want to run the method, we should never call the `run()` method, instead we should call the `start()` method
- and `start()` method internally will call the run method
- Since, we are overriding the `start()` , the Thread's *(extended)* method (start()) will never be called, unless we write `super.start()`
- `super.start()` is responsible for running the thread
- Either we should not override the start() method, or we should override it and then write the `super.start()` to let it do its job
- Now, we are calling the start() method, but we are getting the output as *In start method*, *In run method*, run method is called becaused of the `super.start()` internally.
- mt is in a new state; `MyThread mt = new MyThread();`
- when we call this 👉 `mt.start()` then, it goes into a state known as **Runnable**
- Runnable means now it is *eligible for running*.
- When this `mt.start()` tells JVM that this thread is ready to run now.
    - now JVM, will decide by its own, and then it will tell this thread `mt`, now u run your `run()` method.
- So, a thread which is *runnable* may not be running, it only begins to run, when its actual `run()` method starts running.
    - when the `run()` method is running for that class *(MyThread.java)*
    - when it was printing the statement of run() method, it was in a running state
    - but there might occur a situation, when thread has to wait
        - for example: thread is expecting some input, now thread has to wait
        - and thread needs to allow other threads to move forward
        - so thread which has been waiting, or which has let the other thread to move forward.
        - JVM will keep that thread aside, and JVM will give other threads an opportunities to move forward/go ahead. Example: Pani Puri
- Whenever you want to run a thread, **NEVER** call its `run()` method call its `start()` method

### Modification Version - 1
#### ThreadSimpleClient.java
```java
public class ThreadSimpleClient {
	public static void main(String[] args) {
		//mt is currently in new state
		MyThread mt = new MyThread();
		// when we call the start method to it, then it goes to a state called as Runnable
        mt.start();
	}
}
```

#### MyThread.java
```java
public class MyThread extends Thread {
	@Override
	//when this is running; it is in a printing state
	public void run() {
		System.out.println("In run method");
	}
	@Override
	public synchronized void start() {
		Systerm.out.println("In start method");
        super.start();
	}
}
```

#### Output
```
In start method
In run method
```

### Modification Version - 2

- Let's suppose we want to run multiple instances of mt
- now, the thread model doesn't listen to the `mt.start()`
- the run() method we could have called anytime, BUT start() method we cannot call.
    - because we call a start method of the thread for the first time, it internally calls the run method, thread becomes running and when the job is done, thread is dead; terminates
    - and in a dead thread, you cannot call a start() method.
- we cannot call multiple times `start()` method on the same instance (mt), otherwise it'll throw an **IllegalThreadStateException**

#### ThreadSimpleClient.java
This is not a valid code.👇
```java
public class ThreadSimpleClient {
    public static void main(String[] args) {
        MyThread mt = new Mythread();
        mt.start();
        mt.start();
    }
}
```
#### Output
```
In start method
In start method
In run method
Exception in thread "main" java.lang.IllegalThreadStateException ...
```

### Modification Version - 3
Can i create one more instance and then call? ➡️ Yes
However, this is a valid code.👇

```java
public class ThreadSimpleClient {
    public static void main(String[] args) {
        MyThread mt = new Mythread(); //this thread has served its purpose, and now is dead
        mt.start();
        mt = new MyThread(); //creating another of the same ref, but its reffering to diff obj
        mt.start();
    }
}
```

#### Output
```
In start method
In start method
In run method
In run method
```

### Modification Version - 4

- Every thread has got some default name
- `.currentThread()`
- `Thread.currentThread().getName()` : 
- `Thread.sleep(delay)` : Thread.sleep is a static method, `delay` is expressed in milliseconds and datatype as long
- If we want to add delay in our program, we can allow threads to sleep so that the other threads can run.
- If the thread can sleep a given milliseconds of delay, so can we make a clock app whose second hands moves one position after 1000s
    - no, beacuse of time delay
- when the thread wakes up again it becmoes eligible to runnable, to run again, but JVM has not allowed yet to run
- sleep is a method for delay
- in the code below, it prints the count and the tread name, and then it goes to sleep so that other threads will get some time
- sleep method throws an *InterruptedException*
- every thread will runniing for counter
- creating three objects of threads, every thread will be running for counter, and then go for sleep and wake up again
- and these threads are running **asynchronously**
- order can vary as we can see from the output
	- *thread uth rha hai, kaam kr rha hai, fir so rha hai, (hota rehta h ye) jab tak uska counter 0 nhi ho jata*

#### ThreadSimpleClient.java
```java
public class ThreadSimpleClient {
	public static void main(String[] args) {
		ThreadClass t0 = new ThreadClass(10,300);
		t0.start();
		ThreadClass t1 = new ThreadClass(15,200);
		t1.start();
		ThreadClass t2 = new ThreadClass(20,400);
		t2.start();
	}
}
```

#### ThreadClass.java
```java
public class ThreadClass extends Thread{
    private int counter;
    private int delay;

    public ThreadClass(int counter, int delay) {
        super();
        this.counter = counter;
        this.delay = delay;
    }

    @Override
    public void run() {
        while (counter-- > 0) {
			System.out.println(Thread.currentThread().getName() + "-" + counter);
			try {
				Thread.sleep(delay);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
    }
}
```

#### Output
```
Thread-0-9
Thread-2-19
Thread-1-14
Thread-1-13
Thread-0-8
Thread-1-12
Thread-2-18
Thread-0-7
Thread-1-11
Thread-1-10
Thread-2-17
Thread-0-6
Thread-1-9
Thread-0-5
Thread-2-16
Thread-1-8
Thread-1-7
Thread-0-4
Thread-1-6
Thread-2-15
Thread-0-3
Thread-1-5
Thread-2-14
Thread-1-4
Thread-0-2
Thread-1-3
Thread-0-1
Thread-1-2
Thread-2-13
Thread-1-1
Thread-0-0
Thread-2-12
Thread-1-0
Thread-2-11
Thread-2-10
Thread-2-9
Thread-2-8
Thread-2-7
Thread-2-6
Thread-2-5
Thread-2-4
Thread-2-3
Thread-2-2
Thread-2-1
Thread-2-0
```

------------------------------------------------

## Understanding Multithrading along with main method
- main method is itself like a thread, and it is also running a thread; parent -> child
- main thread is finished but stil thread is working
- hypothetical example:
    - *supervisor aya, tasks assign kiya workers ko supervisor nikal gaya, par workers abhi v tasks kar rhe h, jab tak complete na ho jaye*

#### FirstClient.java
```java
public class FirstClient {
	public static void main(String[] args) {
		System.out.println(Thread.currentThread().getName() + " - Started");
		ThreadClass tc = new ThreadClass(10, 500);
		tc.start();
		
		try {
			Thread.sleep(700);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println(Thread.currentThread().getName() + " - Finished");
	}
}
```

#### Output
```
main - Started
Thread-0-9
Thread-0-8
main - Finished 👈 this method is not waiting for the other methods to complete
Thread-0-7
Thread-0-6
Thread-0-5
Thread-0-4
Thread-0-3
Thread-0-2
Thread-0-1
Thread-0-0
```
---------------------------------------------

### Implementing Threads using Runnnable
- we create a runnable class
- instead of extending from thread we implement **Runnable**
- Runnable interface is also exactly like thread and thread also implements runnable interface
- as it is interface we can also extend it from another class and run the thread
- creating a thread, and passing the runnable reference to it.
- Output show, they are running in parallel, asynchronous
- we create object of the thread, and we pass the instance of the runnable inside the newly object of thread.	

#### RunnableSimpleClient.java
```java
public class RunnableSimpleClient {
	public static void main(String[] args) {
		RunnableClass r1 = new RunnableClass();
		//r1.start(); this is wrong
		//Thread has got a constructor which accepts another Runnable instance
		Thread t1 = new Thread(r1); //this thread will run internal runnable
		t1.start();	//internally, it'll call the run()
		
		RunnableClass r2 = new RunnableClass();
		Thread t2 = new Thread(r2);
		t2.start();
		
		RunnableClass r3 = new RunnableClass();
		Thread t3 = new Thread(r3);
		t3.start();
	}
}
```

This RunnableClass is exactly same, the only difference is that instead of extending it from the Thread class, we are implementing the Runnable Interface.
#### RunnableClass.java
```java
public class RunnableClass implements Runnable {
	private int counter = 10;
	@Override
	public void run() {
		while(counter-- > 0) {
			System.out.println("Runnable " + Thread.currentThread().getName() + "-" + counter);
		}
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}
```

#### Output
```
Runnable Thread-0-9
Runnable Thread-0-8
Runnable Thread-0-7
Runnable Thread-0-6
Runnable Thread-0-5
Runnable Thread-0-4
Runnable Thread-0-3
Runnable Thread-1-9
Runnable Thread-1-8
Runnable Thread-1-7
Runnable Thread-1-6
Runnable Thread-2-9
Runnable Thread-1-5
Runnable Thread-1-4
Runnable Thread-1-3
Runnable Thread-1-2
Runnable Thread-1-1
Runnable Thread-1-0
Runnable Thread-0-2
Runnable Thread-0-1
Runnable Thread-0-0
Runnable Thread-2-8
Runnable Thread-2-7
Runnable Thread-2-6
Runnable Thread-2-5
Runnable Thread-2-4
Runnable Thread-2-3
Runnable Thread-2-2
Runnable Thread-2-1
Runnable Thread-2-0
```

----------------------------------------

## Synchronized Thread
- The output shows messed up results
    - Because 3 independent threads (*t1.start(), t2.start(), t3.start()*) are working simultaneously on a shared part of the code 
- In the code written below, no thread has claimed a **LOCK** in the print method of `MultiString.java`
- Hypothetical Example:
    - *using public toilet, one cannot use unless and until the person who was using comes out, then only the other person standing outside the door, can go inside*
    - **NOTE:** *washroom does not has a lock, so it's possible that anybody can go inside, but this shouldn't be the case*

#### ThreadSynchClient.java
```java
public class ThreadSynchClient {
	public static void main(String[] args) {
		Thread t1 = new PrinterThread("Hello ", "Mr ", "Mogambo");
		Thread t2 = new PrinterThread("Any ", "news from ", "Miss Hawahawai?");
		Thread t3 = new PrinterThread("Big ", "Brother is ", "watching you");
		
		t1.start();
		t2.start();
		t3.start();
	}
}

```

#### PrinterThread.java
```java
public class PrinterThread extends Thread {
	private String part1;
	private String part2;
	private String part3;
	
	public PrinterThread(String part1, String part2, String part3) {
		super();
		this.part1 = part1;
		this.part2 = part2;
		this.part3 = part3;
	}
	
	@Override
	public void run() {
		MultiString.print(part1, part2, part3); //calling the method in a static way
        // new MultiString().otherMethod(); //calling on the instance of it..
	}
}
```

#### MultiString.java
```java
public class MultiString {
	public static void print(String part1, String part2, String part3) {
		System.out.print(part1); //printing part1

		try { //part1 : sleeping for some time, or doing other work
			Thread.sleep(500); //when it is sleeps, other might get a chance to run
		} catch (InterruptedException ie) {
			//eating exception
		}

		System.out.print(part2); //printing part2

		try { //part2 : sleeping for some time, or doing other work
			Thread.sleep(500);
		} catch (InterruptedException ie) {
			//eating exception
		}

		System.out.println(part3); //printing part3
	}
}
```

#### Output
```
Hello 
Any 
Big 
news from
Mr
Brother is
Miss Hawahawai?
Mogambo
watching you
```

### Modification Version - 1
- A mechanism is acquiring a LOCK and that is what a thread does.
- As we can see in the previous example, no threads has claimed a LOCK in the print() method
	- and that is why every thread is able to enter inside the print() method
		- because other threads does not know that someone else has already LOCKed it.
		- that's why we are getting such messed up output
- A simple mechanism for acquiring a **LOCK** in java is using `synchronized` keyword.
- When we write the keyword `synchronized` it means that the print() method will allow only one thread at a time. 
	- and if it is *sleeping* the other threads will not enter unless first thread has come out. 
- here `synchronized` keyword acts as a **LOCK**, refer to the above example, now this `synchronized` can be understood by a slipper kept outside the bathroom *working as a LOCK*
- Sequence is not maintained *(see the output)* but when the thread enters it will complete it's task will come out, and then only other tasks can get it, one by one
- When thread t1 will go in, thread t2, t3..tn will not be allowed to enter inside the print() method
- let's suppose there was other method present in the class `public synchronized static void otherMethod(String part1, String part2, String part3)` from the same class

- Although t1 is in the `print()`, thread is in sleeping state, still **JVM will not allow any other method to enter** the `otherMethod()`, because it is **synchronized**
- However if the method is not marked as `synchronized` for example, `public static void anotherMethod(String part1, String part2, String part3)`, then the other t2 and t3 can enter into this `anotherMethod()`, even if the thread t1 is locked in the `print()`

- If the thread is inside one **synchronized** method, no other thread can enter in any other **synchronized** method, however, the other threads can enter in *non-synchronized* methods.

- Threads Drawback: Performance Issue; blocking all other synchronized methods
	- and such a synchronized methods are shared objects; shared resource

#### MultiString.java
```java
public class MultiString {

	public synchronized static void otherMethod(String part1, String part2, String part3) {}

	public static void anotherMethod(String part1, String part2, String part3) {}

	public synchronized static void print(String part1, String part2, String part3) {
		System.out.print(part1); //printing part1
		try { //part1 : sleeping for some time, or doing other work
			Thread.sleep(500); //when it is sleeps, other will not get chance to run
		} catch (InterruptedException ie) {
			//eating exception
		}

		System.out.print(part2); //printing part2
		try { //part2 : sleeping for some time, or doing other work
			Thread.sleep(500);
		} catch (InterruptedException ie) {
			//eating exception
		}
		System.out.println(part3); //printing part3
	}
}
```

#### Output
```
Any news from Miss Hawahawai?
Big Brother is watching you
Hello Mr Mogambo
```

### Modification Version - 2

- Static methods are *attached to classes* and NOT to instances
    - So when t1 enters the `print()` t1 has acquired a **LOCK** on the `class MultiString`
    - There are two locks with every instance, one is *Class Level Lock*, and the other is *Instance Level Lock*
    - So whenever a thread enters a **static** method, it acquires a **Class Level** lock

- When the method is synchronized but non static (an **Instance Method**) 👉 `public syncrhonized void otherMethod()` then, t2 or other threads can enter this method.
    - Hence, t2 acquires **Object Level Lock**
- If a Static Lock is acquired by one thread, then no other thread can enter into any synchronized static method of that class
	- But they **can enter** into any *non static syncrhonized method* or *non static method*


```java
public class MultiString {
    public synchronized void otherMethod(String part1, String part2, String part3) {}
    public static void anotherMethod(String part1, String part2, String part3) {}
    public synchronized static void print(String part1, String part2, String part3) {}
    ...
}
```

### Modification Version - 3

- Suppose, t1 has come inside otherMethod() and then gone to the print() method which is a static one.
- So, t1 has acquired LOCK on both the levels;
	- i.e., Object Level Lock on otherMethod and Static Level Lock on print()
- Now, no other method can enter into any synchronized method of this class
	- Because t1 acquired both the locks
	- and a thread can acquire locks at both level

```java
public class MultiString {
	public synchronized void otherMethod(String part1, String part2, String part3) {
		//t1 has lock
		try {
			Thread.sleep(500);
			//t1 is entering the print()
			MultiString.print("A", "C", "V");
		} catch (InterruptedException i) {
			//eating exception
		}
	}

	public synchronized static void print(String part1, String part2, String part3) {
		//now, t1 has this lock also
		System.out.print(part1); //printing part1
		try { //part1 : sleeping for some time, or doing other work
			Thread.sleep(500); //when it is sleeps, other will not get chance to run
		} catch (InterruptedException ie) {
			//eating exception
		}

		System.out.print(part2); //printing part2
		try { //part2 : sleeping for some time, or doing other work
			Thread.sleep(500);
		} catch (InterruptedException ie) {
			//eating exception
		}
		System.out.println(part3); //printing part3
	}
	}
}
```

*Complex Situation discussed at 1:52:00*

-------------------------------------------------

## Interrupting Thread
- A thread which is sleeping can be interrupted
- Sleeping is not creating performance issue, rather it is allowing other threads to run
- When the thread is sleeping is not blocking othres, unless it is sleeping inside a synchronized method
- If the thread sleeps inside the synchronized method, then it becomes a problem.

- SoneWalaAdmi jisko 5000 milli second tak sona tha, usko utha 1000 milli second me hi utha diya `Chal Uth Jaa Ab..` hogaya uska, abb woh aur 5000 milli second complete krne k liye nhi soyega; Hence, Sleep **Interrupted**

#### ThreadInterrupter.java
```java
public class ThreadInterrupter {
	public static void main(String[] args) {
		//anonymous class👇
		Thread t1 = new SoneWalaAadmi() {
			@Override
			public void run() {
				System.out.println("Thread Started");
				try {
					Thread.sleep(5000);
				} catch (InterruptedException e) {
					System.out.println("Thread was Interrupted.");
				}
				System.out.println("Thread Finished - Interrupt Kar K");
			}
		};
	}
}

class SoneWalaAadmi extends Thread {
	@Override
	public void run() {
		System.out.println("SoneWalaAadma Sone Chala Gaya..");
		try {
			Thread.sleep(5000);
		} catch (InterruptedException e) {
			System.out.println("Chal Uth Jaa Abb.. (Neend Torr Diya)");
		} 
		System.out.println("SoneWalaAadmi Apna Neend Pura Kar K Uth Gaya..");
	}
}
```  

The above code can be written in this way also 👇 *if not using Anonymous Class*

#### ThreadInterrupter.java
```java
public class ThreadInterrupter {
	public static void main(String[] args) {
		Thread t1 = new SoneWalaAadmi();
		System.out.println("Main Thread Started");
		t1.start();
		
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println("Main Thread Interrupted SoneWaleAdmi (Neend Torr Raha Hai Uska)");
		t1.interrupt(); //it interrupts the speeling thread
		System.out.println("Main Thread Finished - Interrupt Kar K");
	}
}

class SoneWalaAadmi extends Thread {
	@Override
	public void run() {
		System.out.println("SoneWalaAadmi Sone Chala Gaya..");
		try {
			Thread.sleep(5000);
		} catch (InterruptedException e) {
			System.out.println("Chal Uth Jaa Abb.. (Neend Torr Diya)");
		} 
		System.out.println("SoneWalaAadmi Apna Neend Pura Kar K Uth Gaya..");
	}
}
```

#### Output
```
Main Thread Started
SoneWalaAadmi Sone Chala Gaya..
Main Thread Interrupted SoneWaleAdmi (Neend Torr Raha Hai Uska)
Main Thread Finished - Interrupt Kar K
Chal Uth Jaa Abb.. (Neend Torr Diya)
SoneWalaAadmi Apna Neend Pura Kar K Uth Gaya..
```

### Modification Version - 1 
Snooze Wala Case 👇
#### ThreadInterrupter.java
```java
public class ThreadInterrupter {
	public static void main(String[] args) {
		Thread t1 = new SoneWalaAadmi();
		System.out.println("Main Thread Started");
		t1.start();
		
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println("Main Thread Interrupted SoneWaleAdmi (Neend Torr Raha Hai Uska)");
		t1.interrupt(); //it interrupts the speeling thread
		System.out.println("Main Thread Finished - Interrupt Kar K");
	}
}

class SoneWalaAadmi extends Thread {
	@Override
	public void run() {
		System.out.println("SoneWalaAadmi Sone Chala Gaya..");
		try {
			Thread.sleep(5000);
		} catch (InterruptedException e) {
			//alarm lagaya tha, jaagne k baad firse so gaya
			// Thread.sleep(5000); //this is theoretical, NOT practical
			System.out.println("Chal Uth Jaa Abb.. (Neend Torr Diya)");
		} 
		System.out.println("SoneWalaAadmi Apna Neend Pura Kar K Uth Gaya..");
	}
}
```

----------------------------------------------------------------------

## Example of Multithreading
- Runtime Exceptions are unchecked
- In the run() we cannot have throws clause, we cannot add it
- If a base class writes throws Exception, child classes can always avoid to write to any specfic Exception
- 
#### Calculator.java
```java
public class Calculator extends Thread {
	private Boolean done = false;
	@Override
	public void run() {
		try {
			//Executing Complex Algorithm
			Thread.sleep(5000);
		} catch (InterruptedException e) {
			e.printStackTrace();
			throw new RunTimeException("Unkown Interrupt");
		}
		//Solution Found
		done = true;
		System.out.println("Solution found and updated");
	}

	public boolean isDone() {
		return done;	
	}
}
```

#### ImpatientClient.java
```java
//ImpatientClient keeps on irritating dependent thread for completion status; see the out[ut]
public class ImpatientClient {
	public static void main(String[] args) throws InterruptedException {
		Calculator calculator = new Calculator();
		//this thread has started the calculation task
		//as if a manager has come nd it has assigned the task to new joinee
		//"you do this calculation, make a report of this"
		//"and let me know, once you are done"
		calculator.start();
		//"manager team member ko baar baar puch rha hai"👇
		//"are you done?"
		//"are you done?"
		//"are you done?"
		while(!calculator.isDone()) {
			System.out.println("Solution Not Found");
			Thread.sleep(500);
		}
		System.out.println("Solution Found");
	}
}
```

#### Output
```
Solution Not Found
Solution Not Found
Solution Not Found
Solution Not Found
Solution Not Found
Solution Not Found
Solution Not Found
Solution Not Found
Solution Not Found
Solution Found and Updated
Solution Found
```

----------------------------------------------

#### Calculator.java
```java
public class Calculator extends Thread {
	private Boolean done = false;
	@Override
	public void run() {
		try {
			//Executing Complex Algorithm
			Thread.sleep(5000);
		} catch (InterruptedException e) {
			e.printStackTrace();
			throw new RunTimeException("Unkown Interrupt");
		}
		//Solution Found
		done = true;
		System.out.println("Solution found and updated");
	}

	public boolean isDone() {
		return done;	
	}
}
```

- We can call the `join()` on another thread, so the current thread will wait for another thread to complete its task.

#### PatientClient.java
```java
//PatientClient waits for dependent thread to finish
public class PatientClient {
	public static void main(String[] args) throws InterruptedException {
		Calculator calculator = new Calculator();
		calculator.start();
		System.out.println("WAIT!.. System Busy in Complex Algorithm");
		//the thread is going to be stopped, till calculator has finished its task
		//jaise hi calculator uska kaam finish kr deta h, it come and "joins" to the main thread nd go ahead
		calculator.join();
		System.out.println("Thread Joined Again..");
		if(calculator.isDone()) {
			System.out.println("Solution Found");
		}
		//timeWaster.start();
		//above statement will result in exception,
		// we cannot start thread again by calling start()
	}
}
```

#### Output
```
WAIT!.. System Busy in Complex Algorithm
Solution Found and Updated
Thread Joined Again
Solution Found
```