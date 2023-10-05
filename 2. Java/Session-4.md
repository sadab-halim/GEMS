# Session 4 | Collections

#### InterfaceExample.java
```java
package com.psl.iface;

public class InterfaceExample {
	public static void main(String[] args) {}
}
```

#### Vehicle.java
```java
package com.psl.iface;
//Vehicle is an abstract class we cannot create its instance
public abstract class Vehicle {
	//abstract method means we don't need to give any imp to it
	public abstract void start();
	// same as ☝️ void start();
}
```

#### Car.java
```java
package com.psl.iface;
//Car is inheriting two properties: a. extending Vehicle, b. implements Commodity, 
//Commodity is an interface
//once a class is interface, all the methods are public as well as abstract

//Car has two fulfill two conditions; 
	//a. it must implement start() which comes from Vehicle.
	//b. it must implement sell() which is coming from Commodity
public class Car extends Vehicle implements Commodity {
	
	@Override
	public void start() {
		System.out.println("Car Started.");
	}

	@Override
	public void sell() {
		System.out.println("Car Sold.");
	}
}
```

#### Commodity.java
```java
package com.psl.iface;
//Commodity is an interface and we cannot create its instace
public interface Commodity {
	public void sell();
}
```

#### Driver.java
```java
package com.psl.iface;

public class Driver {
	//private Car car = new Car();
	// ----------------------------
	//on left hand side, it checks whether it can be called or not
	//only Car's instance can be created;
	//since, Commodity is an interface and we cannot create instance of an interface
	//Vehicle's ref is pointing to Car object
	//first of all it'll check on left hand side, it is vehicle's ref
	private Vehicle vehicle = new Car();
	public void drive() {
		//we don't want car to sell; car.sell();
		//------------------------------------
		//so i cannot call that method which is not their in the vehicle, even though it is in the Car
		//vehicle.sell();   //error; Driver class will not be able to call this method
		vehicle.start();
	}
}
```

#### Dealer.java
```java
package com.psl.iface;

public class Dealer {
	//interface binds a contract with the client
	//and contract with Dealer is, "you can only sell(), you can't start()"
	private Commodity commodity = new Car();
	public void doBusiness() {
		//similarly, like the prevs example, we cannot call any other method other than sell()
		commodity.sell();
		//commodity.start();  error; it has no meaning
	}
}
```

- If we make any method *static*, then we'll not be able to use the functionality of **Polymorphism**

---------------------

# Collections
- Collection is a type; includes *classes, interface, enum, everything*
- Collection is basically one interface, one interface means u cannot make instance of it.
- You need instance of some class which implements these interface and we can extend from another class but we implement from an interface.
- Class can implement multiple interfaces but a class can extend only from one class.
- In Java, multiple inheritance is not possible.
- Map is a key-value pair.
- Collection is a data structure holding multiple objects together.
- **Set** holds only *unique elements*
	- **SortedSet** sorts the elements in it.
- **List** *allows duplicates* BUT *maintains the sequence*.
- **Map** is a *key-value* pair.
	- When we want to store something as in *dictionary* format, then we use Map
	- **SortedMap** sorts the entries with the sorted keys.
	- Multiple keys can have same values.
	- BUT, a key cannot be duplicate.
- We can iterate over the collections in different ways; shown below 👇

## List Example
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Iterator;

public class CollectionTut {
	public static void main(String[] args) {
		//this will not work👇
		//List names = new List(); //bcoz list is an interface and we cannot make instances of an interface
		//List names = new ArrayList();	//this one is a raw (generic) type, NOT recommended
		
		List<String> names = new ArrayList<>();	//left hand side and right hand side data type should be same
		//this ArrayList as a class☝️; is maintaing an array in the backend
		//now, it has become a dynamically growing array.  
		names.add("Suresh");
		names.add("Ramesh");
		names.add("Haresh");
		names.add("Seeta");
		names.add("Geeta");
		
		names.remove(0); //removes the value from the given index
		names.remove("Geeta"); //removes the value which has the name as "Geeta"
		
		System.out.println(names.size()); //returns the size of list
		System.out.println(names);	//internally; names.toString()
		System.out.println(names.contains("Seeta"));	//chcek and compares with every element

		//Iterator gives a guarantee that 
		//each element will be definitely addressed in its iteration
		Iterator<String> itr = names.iterator();
		//hasNext() just tells whether their is a next element present or not
		//BUT hasNext() is NOT going to shift the pointer to the next element
		while(itr.hasNext()) { //as long as next is their, we can put it inside a loop
			System.out.println(itr.next().concat(" FROM ITERATOR"));
		}
	}
}
```

#### Output
```
3
[Ramesh, Haresh, Seeta]
true
Ramesh FROM ITERATOR
Haresh FROM ITERATOR
Seeta FROM ITERATOR
```

-----------------------------------------------

- If there are three "Geeta" then which one will be removed first using names.remove("Geeta")?
	- It'll remove the first occurence of "Geeta"
## Set Example
```java
import java.util.Set;
import java.util.HashSet;
import java.util.Iterator;

public class CollectionTut {
	public static void main(String[] args) {
		//HashSet is something which is a class which implements Set
		//Set doesn't guarantees a set of sequence
		Set<String> names = new HashSet<>(); 
		names.add("Suresh");
		names.add("Ramesh");
		names.add("Haresh");
		names.add("Seeta");
		names.add("Geeta");
		
		names.remove(0);
		names.remove("Geeta");
		
		System.out.println(names.size());
		System.out.println(names);
		System.out.println(names.contains("Seeta"));

		Iterator<String> itr = names.iterator();
		while(itr.hasNext()) {
			System.out.println(itr.next().concat(" FROM ITERATOR"));
		}
	}
}
```

#### Output
```
3
[Seeta, Suresh, Haresh, Ramesh]
true
Seeta FROM ITERATOR
Suresh FROM ITERATOR
Haresh FROM ITERATOR
Ramesh FROM ITERATOR
```

--------------------------------

## Other Ways of Iteration in Collections Framework (List Example)
```java
public class CollectionTut {
	public static void main(String[] args) {
		List<String> names = new ArrayList<>();
		names.add("Suresh");
		names.add("Ramesh");
		names.add("Haresh");
		names.add("Seeta");
		names.add("Geeta");
		
		names.remove(0);
		names.remove("Geeta");
		
		System.out.println(names.size());
		System.out.println(names);
		System.out.println(names.contains("Seeta"));
		
		System.out.println("--------------------");
		/* ------- WAY-1 ------- */
		Iterator<String> itr = names.iterator();
		while(itr.hasNext()) {
			System.out.println(itr.next().concat(" FROM ITERATOR"));
		}
		
		System.out.println("--------------------");
		/* ------- WAY-2 ------- */
		//for each loop; take one name at a time, and execute whatever is written here (names)
		//names will iterate all the names as collection; bcoz, in the backend their is an array
		for(String s: names) {
			System.out.println(s.concat(" FROM FOR EACH LOOP"));
		}
		
		System.out.println("--------------------");
		/* ------- WAY-3 ------- */
		//printing form stream
		//for each element in the stream, take each name 👇
		names.stream().forEach(name->System.out.println(name.concat(" FROM STREAM")));
		
		System.out.println("--------------------");
		/* ------- WAY-4 ------- */
		//collection; conventional loop
        //avoid using this loop 
        //bcoz this loop will differentiate behavior between List and Set
		//this .get() 👇 will not work when using Set
		//bcoz for getting element as an index, their is no sequence is maintained
		for(int i = 0; i < names.size(); i++) {
			System.out.println(names.get(i).concat(" FROM CONV LOOP"));
		}
	}
}
```

#### Output
```
3
[Ramesh, Haresh, Seeta]
true
--------------------
Ramesh FROM ITERATOR
Haresh FROM ITERATOR
Seeta FROM ITERATOR
--------------------
Ramesh FROM FOR EACH LOOP
Haresh FROM FOR EACH LOOP
Seeta FROM FOR EACH LOOP
--------------------
Ramesh FROM STREAM
Haresh FROM STREAM
Seeta FROM STREAM
--------------------
Ramesh FROM CONV LOOP
Haresh FROM CONV LOOP
Seeta FROM CONV LOOP
```

-----------------------------------

## Card Example
Creating two enumerations (*Suit, Rank*), bcos these data remains the same throughout the program.
#### Suit.java
```java
public enum Suit {
	CLUB, DIAMOND, HEART, SPADE;
}
```

#### Rank.java
```java
public enum Rank {
	JACK, QUEEN, KING, ACE;
}
```

#### Card.java
```java
public class Card {
	private Suit suit; //one instance of Suit
	private Rank rank; //one instance of Rank
	//constructor
	public Card(Suit suit, Rank rank) {
		super();
		this.suit = suit;
		this.rank = rank;
	}
	//why we use toString()?
	//👉 to avoid contcatenation; performance issue
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}
}
```

### CollectionTut.java
```java
public class CollectionTut {
    public static void main(String[] args) {
		//instance of Suit and Rank using enum name with . operator
		Card c = new Card(Suit.DIAMOND, Rank.JACK);
		System.out.println(c);
	}
}
```

#### Output
```
(DIAMOND, JACK)
```

### Modification Version - 1
#### CollectionTut.java
```java
public class CollectionTut {
public static void main(String[] args) {
		//creating a list of cards
		//LinkedList >>> ArrayList; 
		//bcoz it's easy to maintain the pos and has the ref of the next elem
		//whenever we expects lots of insertion, deletion we should use LinkedList, 
		//otherwise we should use ArrayList
		List<Card> cards = new LinkedList<>();
		//iterating all the suit and rank using values method
		for(Suit suit: Suit.values()) { //returns a suit arrays
			for(Rank rank: Rank.values()) { //returns a rank arrays
				Card card = new Card(suit, rank); //getting one combination of card
				cards.add(card);
			}
		}
		System.out.println(cards);
	}
}
```
#### Output
*prints all the cards..*

### Modification Version - 2

#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		List<Card> cards = new LinkedList<>();
		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}
		//default implementation of toString() takes place.. 👇
		System.out.println(cards);	

		Card testCard = new Card(Suit.CLUB, Rank.JACK);
		//checking whether the card is present or not
		boolean present = cards.contains(testCard); // element == testCard; physical equality
		//element.equals(testCard); logical equality
		System.out.println(present); // false

	}
}
```

- Why does it returns false when (*CLUB, JACK*) is already present?👇
	- `.contains()` is *physcial equality* whereas `.equals()` is a *logical equality*
- There are two types of equalities:
	- **Physical Equality** : the == sign in the physical equality (*.contains()*). In Java, the implementation of this == for any refs is exactly if they are going to *point the same memory*, then it says **true** //internally, it calls the .equals() method and compares.
	- **Logical Equality** : the *.equals()* 
- If I want the equals() method to work in my own way, then I'll have to override the equals() method.
- and whenever we override the equals() we also override the hashCode() method.

### Modification Version - 3 
#### Card.java
```java
public class Card {
	private Suit suit;
	private Rank rank;

	public Card(Suit suit, Rank rank) {
		super();
		this.suit = suit;
		this.rank = rank;
	}
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}

	@Override
	//if i change the parameters from Object to Card, then the code which was NOT recognized before will be recognized now
	//BUT, it'll violate the overriding; bcoz the main sign of object class is object NOT Card
	public boolean equals(Object obj) {
		//this code will not be recognized 👇
		//if(this.suit.equals(obj.suit) && this.rank.equals(obj.rank))
		//-----------------------------------------------------
		//converting obj ref into card (class) ref; downcasting
		Card other = (Card) object;
		return (this.suit.equals(other.suit) && this.rank.equals(other.rank)); //logical equality
	}	
}
```

- it works now, as we have provided / overrided the equals() method
- For collections to work properly, if there are entities class like this, then we will have to override the equals().
- But, this solution is not the robust soln, it's a fragile soln.

#### Output
*prints all cards..* <br/>
true

### Modification Version - 4
#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		List<Card> cards = new LinkedList<>();
		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}

		System.out.println(cards);

		Card testCard = new Card(Suit.CLUB, Rank.JACK);
		String name = "Suresh";
		//checking whether the card is present or not
		boolean present = cards.contains(name); // element == testCard; physical equality
		// element.equals(testCard); logical equality
		System.out.println(present); // false

	}
}
```
#### Output
*prints all the cards..* <br/>
false

### Modification Version - 5
#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		List<Card> cards = new LinkedList<>();
		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}

		System.out.println(cards);

		Card testCard = new Card(Suit.CLUB, Rank.JACK);
		String name = "Suresh";
		//when we r sending the name in the .equals() method;
		//it is trying to typecast it in the Card class 👉 Card other = (Card) obj;
		//it's definitely not a good idea, 
		//bcoz the obj which came inside in the 👉 public boolean equals(Object obj) {..}
		//is NOT of type Card
		System.out.println(testCard.contains.equals(name));
		//checking whether the card is present or not
		boolean present = cards.contains(name); // element == testCard; physical equality
		// element.equals(testCard); logical equality
		System.out.println(present); // false

	}
}
```

#### Output
*prints all the cards..*
```
Exception in thread "main" java.lang.ClassCastException..
```

- We get a ClassCastException
- What is happening is, when we are sending name in the equals method, it is trying to typecast with the Card
- Which is not at all a good idea, and it's definitely going to fail
	- Bcoz the object which has come inside is not of the class card
- So, we'll have to check it using the if statements..
- getClass() gives the class of *this* object
- if conditions in `equals()`:
	1. If testing with itself ➡️ true
	2. If testing with null ➡️ false
	3. If both objects belong to different classes ➡️ false
	4. If a.equals(b) ➡️ true, b.equals(a) ➡️ true ; **Symmetric**
	5. If a.equals(b) ➡️ true, b.equals(c) ➡️ true, a.equals(c) ➡️ true ; **Transitive**
	6. If a.equals(b) ➡️ true, *it should be always true, if ones true*; **Consistent**

	<br/>

	- It is **Reflexive**; for any non-null ref value a, a.equals(a) returns true;
	- It is **Symmetric**; for any non-null ref values a and b, a.equals(b) should return true if and only if b.equals(a) returns true
	- It is **Transitive**; for any non-null ref values a, b and c, if a.equals(b) returns true and b.equals(c) returns true, then a.equals(c) should return true;
	- It is **Consistent**; for any non-null ref values a and b, multiple invocations of a.equals(b) consistently return true or consistently return false, provided no information used in equals comparisons on the object is modified.
- I don't want to run equals() in every conditions, instead i want to keep them all by distributing in pocket and then search from it; *key-value* pair

```
 |________| --> |________|
 |________| --> |________|
 |________| --> |________|
 |   S    | --> | Sadab  |
 |________| --> |________|
 |________| --> |________|
```

- hashCode() is an integer value, which any object returns an integer value

### Modification Version - 6
#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		List<Card> cards = new LinkedList<>();
		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
				cards.add(card);
			}
		}
		System.out.println(cards.size());
	}
}
```

#### Output
```
32
```

### Modification Version - 7

#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		Set<Card> cards = new HashSet<>();
		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				//whenever we want to add it
				//it'll check internally, do we contain these cards
				//how will it check it?
				//from the given card, it'll calculate the hashcode
				//hashcode() belongs to Object class
				//the hashcode() returns an int value 
				//the int value of hashcode is like a 🪣
				//it returns hashcode for individual identity for every object
				//for this 👇 hashcode itself is saying hashcode doesn't match
				//for two diff refs; hashcode is diff
				//if i want to treat this equal; i need to override the hashcode as well
				cards.add(card);
			}
		}

		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}

		System.out.println(cards.size());
	}
}
```

#### Output
```
32
```

- Why .contains() does not throw any exceptions?
- When we are running the application instead of implementing the overriden equals() it is bypassing it. (*for both List and Set*)
- If I add the cards 2x times, as shown in the example above, then it is not allowing duplicates. //Output: 16
- If I run the loop for the second time, then it allows the duplicates. //Output: 32
- We are not checking Physical Equality, we are checking from the Logical Equality
	- When we are using List, it doesn't matter; bcoz List allows duplicate values; hence no need to check
	- BUT when we are using Set, it was supposed to print only unique values and avoid duplicates; but it bypassed the equals() method, Why?

### Modification Version - 8
- Implementing hashCode() 
- For hashCode() and equals() you must select the identity of the object
- After implementing hashCode() and equals() we should get the output as 16
	- Bcoz, now we have told the code, what is the 🪣 and what is the equals() method
#### Card.java
```java
public class Card {
	private Suit suit;
	private Rank rank;

	public Card(Suit suit, Rank rank) {
		super();
		this.suit = suit;
		this.rank = rank;
	}
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((rank == null) ? 0 : rank.hashCode());
		result = prime * result + ((suit == nnull) ? 0 : suit.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if(this == obj) return true;
		if(obj == null) return false;
		if(getClass() != obj.getClass()) return false;
		//converting obj ref into card ref; downcasting
		Card other = (Card) object; 
		if(rank != other.rank) return false;
		if(suit != other.suit) return false;

		return true;
	}	
}
```

#### Output
```
16
```

--------------------------------------------------

### Modification Version - 9
- Can a primitive data type inherit a class? ➡️ It cannot
- Autoboxing: Even though I have given a primitive data type, it has converted into object of data type
#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		List<Integer> l = new ArrayList<>();
		//this is known as autoboxing
		//it has the converted the primtive one into object of the wrapper type, 
		//so that it can be used with a list
		l.add(2); //internally; l.add(new Integer(2)); 👈 autoboxing
		l.contains(2); //it is referring to equals(); internally
		 
	}
}
```

--------------------------------------------------

### Modification Version - 10
- Implementing using TreeSet
- TreeSet has a characterisitic that it sorts it out
- to make it at runtime, we can use comparator

#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		Set<String> names = new TreeSet<>();
		names.add("Suresh");
		names.add("Ramesh");
		names.add("Geeta");
		names.add("Haresh");
		names.add("Seeta");
		System.out.println(names);
	}
}
```

#### Output
Names are now sorted in alphabetical order
```
[Geeta, Haresh, Ramesh, Seeta, Suresh]
```

### Modification Version - 11

#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		Set<Card> cards = new TreeSet<>();
		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}

		for(Suit suit: Suit.values()) {
			for(Rank rank: Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}

		System.out.println(cards);
	}
}
```

- When I implement Comparable it means that my Card is now eligbile to be compared with the other instances of Card
	- will have to Override the compareTo() which is from Comparable
- compareTo() method is used by TreeSet

### Modification Version - 12

#### Card.java
```java
public class Card implements Comparable<Card> {
	private Suit suit;
	private Rank rank;

	public Card(Suit suit, Rank rank) {
		super();
		this.suit = suit;
		this.rank = rank;
	}
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((rank == null) ? 0 : rank.hashCode());
		result = prime * result + ((suit == null) ? 0 : suit.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		System.out.println("In equals()");
		if (this == obj) return true;
		if (obj == null) return false;
		if (getClass() != obj.getClass()) return false;
		
		Card other = (Card) obj;
		if (rank != other.rank) return false;
		if (suit != other.suit) return false;
		
		return true;
	}	

	@Override
	public int compareTo(Card other) {
		//doing the comparision only on the base of suit
		//it returns -ve value if this is less than the other one
		//it returns 0 if this is equal to other one
		//it returns +ve if this is greater than the other one
		System.out.println("In compareTo()");
		return this.suit.compareTo(other.suit);
	}
}
```

#### Output
```
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
[(CLUB, JACK), (DIAMOND, JACK), (HEART, JACK), (SPADE, JACK)]
```

- I'm trying to add 32 elements why the size has come as four?
	- the first for loop has gone well
	- but the next for loop would have been stop, since, it's a set; it doesn't allow duplicate
	- Now, in the Card class, there are two equality criterias
		- equals() crtieria is based on rank and suit
		- whereas compareTo() criteria is to compare only suit
		- it is not checking the equals() criteria
- As we can see from the output it is ignoring equals(), it is not even checking it.
- It is always checking the compareTo()
	- because, it's just need one equality criteria to check
- **Rule**: if their is equals() and compareTo() both present; then it'll ignore equals()
	- if compareTo() is not their, then only it'll use equals()
- Both the equality criterias should be in sync with each other
- When we say implements comparable we are providing natural ordering of cards

### Modification Version - 13
- For which one is large and which one is small
	- String: we go with alphabetical order
	- double, boolean, int, any other data type: the order is common convention
- But for about Card or for our own entities:
	- we use something known as Comparable Interface
	- When we use Comparable Interface, we need to tell with that type of **Object**
		- .. implements Comparable<Object>
		- Example 👇 
			```java
			public class Card implements Comparable<Card> {..}
			```
- If their is a `compareTo()` present, now we know which Card is large and which Card is small.
- TreeSet implements interface known as SortedSet
- Comparable is done in compile-time
- Comparable means, it is *comparable* to something else
#### Card.java
```java
public class Card implements Comparable<Card> {
	private Suit suit;
	private Rank rank;

	public Card(Suit suit, Rank rank) {
		super();
		this.suit = suit;
		this.rank = rank;
	}
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((rank == null) ? 0 : rank.hashCode());
		result = prime * result + ((suit == null) ? 0 : suit.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		System.out.println("In equals()");
		if (this == obj) return true;
		if (obj == null) return false;
		if (getClass() != obj.getClass()) return false;
		
		Card other = (Card) obj;
		// if (rank != other.rank) return false;
		//syncying both the equality criterias; by keeping it same
		//now both are comparing/checking same thing
		if (suit != other.suit) return false;
		
		return true;
	}	

	@Override
	public int compareTo(Card other) {
		//doing the comparision only on the base of suit
		//it returns -ve value if this is less than the other one
		//it returns 0 if this is equal to other one
		//it returns +ve if this is greater than the other one
		System.out.println("In compareTo()");
		return this.suit.compareTo(other.suit);
	}
}
```

-------------------------------------

### Modification Version - 14
- Comparator is used in run-time
- TreeSet extends from AbstractSet and implements SortedSet
- Constructors of TreeSet:
	- TreeSet()
	- TreeSet(Collection<? extends E> c)
	- TreeSet(Comparator<? super E> comparator)
	- TreeSet(SortedSet<E> s)
#### RankComparator.java
```java
import java.util.Comparator;

public class RankComparator implements Comparator<> {
	@Override
	public int compare(Card card1, Card card2) {
		System.out.println("FROM COMPARATOR");
		//we want to compare ranks b/w 2 cards and return the comparison
		//the card1.rank is not accessible here 👇
		// return (card1.rank.compareTo(card2.rank);

		//whereas in Comparable it was accessible
		//bcoz the compareTo method is inside the "Card" class itself 
		//whereas RankComparator is an external comparator, which compares two cards
		
		//adding getter methods in the "Card" class to make it accessible
		//and instead of using card1.rank, now we'll use
		return (card1.getRank()).compareTo(card2.getRank());
	}
}

```

#### CollectionTut.java
```java
import java.util.Comparator;

public class CollectionTut {
	public static void main (String[] args) {
		RankComparator comp = new RankComparator();
		//passing the comp at the time of constructing the TreeSet
		//now, whenever it'll add the elements it'll use this ☝️ comp
		Set<Card> cards = new TreeSet<>(comp);
		for(Suit suit : Suit.values()) {
			for(Rank rank : Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}
		for(Suit suit : Suit.values()) {
			for(Rank rank : Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}
	}
}
```

#### Output
```
FROM COMPARATOR
FROM COMPARATOR
FROM COMPARATOR
FROM COMPARATOR
FROM COMPARATOR
FROM COMPARATOR
FROM COMPARATOR
FROM COMPARATOR
[(CLUB, JACK), (CLUB, QUEEN), (CLUB, KING), (CLUB, ACE)]
```

- Now, it allows, *CLUB, CLUB, CLUB..*
- But, it will accept ONLY one *JACK*, one *QUEEN*, one *KING*, and one *ACE*
- Bcoz, the equality criteria for *JACK* matches with *CLUB* so, it'll not allow to enter inside.
- So, now the comparison is based on the Rank.
- Now, the TreeSet has got 3 equality criterias present:
	1. compare() method of RankComparator class
	2. compareTo() method of Comparable
	3. equals() method in Card class
- If their is an external Comparator, it'll always use the compare() method criteria of Comparator
	- It'll ignore the other two.
- If the Comparator is absent, then it'll use Comparable 👉 compareTo() method
- If the Comparable 👉 compareTo() is also absent
	- Then, it'll use the equals() method of a TreeSet
	- Bcoz, TreeSet requires the Sorting Order.

-------------------------------------
### Modification Version - 15
- If we remove the comp, if we don't pass the comp
- Now, it'll use compareTo() method

#### CollectionTut.java
```java
import java.util.Comparator;

public class CollectionTut {
	public static void main (String[] args) {
		RankComparator comp = new RankComparator();
		//comp is removed from TreeSet👇 
		Set<Card> cards = new TreeSet<>();
		for(Suit suit : Suit.values()) {
			for(Rank rank : Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}
		for(Suit suit : Suit.values()) {
			for(Rank rank : Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}
	}
}
```

#### Output
```
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
In compareTo()
[(CLUB, JACK), (DIAMOND, JACK), (HEART, JACK), (SPADE, JACK)]
```

### Modification Version - 16
- And now if we don't implement the Comparable, then the TreeSet will not work.
- It is mandatory to implement Comparable in order to use TreeSet

#### Card.java
```java
public class Card {
	private Suit suit;
	private Rank rank;

	public Card(Suit suit, Rank rank) {
		super();
		this.suit = suit;
		this.rank = rank;
	}
	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((rank == null) ? 0 : rank.hashCode());
		result = prime * result + ((suit == null) ? 0 : suit.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		System.out.println("In equals()");
		if (this == obj) return true;
		if (obj == null) return false;
		if (getClass() != obj.getClass()) return false;
		
		Card other = (Card) obj;
		// if (rank != other.rank) return false;
		//syncying both the equality criterias; by keeping it same
		//now both are comparing/checking same thing
		if (suit != other.suit) return false;
		
		return true;
	}	
}
```

#### Output
```
Exception in thread "main" java.lang.ClassCastException..
```

### Modification Version - 17
- But instead of using TreeSet, if I use HashSet, then it does not require a comparison, bcoz now it does not requires to sort the elements

#### CollectionTut.java
```java
import java.util.Comparator;

public class CollectionTut {
	public static void main (String[] args) {
		RankComparator comp = new RankComparator();
		Set<Card> cards = new HashSet<>();
		for(Suit suit : Suit.values()) {
			for(Rank rank : Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}
		for(Suit suit : Suit.values()) {
			for(Rank rank : Rank.values()) {
				Card card = new Card(suit, rank);
				cards.add(card);
			}
		}
	}
}
```

#### Output
```
In Equals
In Equals
In Equals
In Equals
In Equals
In Equals
In Equals
In Equals
```
*prints all the cards..*

-----------------------------------

- If we use HashSet and Comparator, then which one will be printed?
	- 👉 Still In Equals, get printed..
	- HashSet does not require any comparisons
- HashSet does not implements SortingSet and it does not have constructor with a Comparator
- HashSet does not gives guarantee that it will be sorted.

--------------------------------------------
## Checklist 📝
✅ Always have classes in your *named-package*, NEVER use *default-package* <br/>
✅ Every attribute must be private <br/>
✅ Constructor depends on what kind of object creation you need. <br/>
✅ While creating getter-setter methods, preferred choice should be to keep them as private, if we don't want to share with clients. If you want to share, make sure, setter methods are private; and If you want to make setter methods as public at least do not make setter methods of primary identity of that object as public <br/>
✅ toString() method should be Overriden <br/>
✅ equals() method must be implemented <br/>
✅ hashCode() method must be implemented <br/>
✅ If you @Override equals() you must @Override hashCode() <br/>
✅ Always prefer using Comparable, if you need natural ordering, and need to do comparison in your application <br/>
✅ Comparator is optional, you can add it afterwards.

--------------------------------------------

### Modification Version - 18
- Static Block gets called first, because it is attached to the class
- Then Instance Block is called; when the instance of this card is being generated and from instance block afterwards; Constructor is called

#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		Card c = new Card(Suit.HEART, Rank.QUEEN);
	}
}
```

#### Card.java
```java
public class Card implements Comparable<Card>{
	private Suit suit;
	private Rank rank;

	static {
		System.out.println("Static Block");
	}
	
	{
		System.out.println("Instance Block");
	}
	
	public Card(Suit suit, Rank rank) {
		super();
		System.out.println("Constructor");
		this.suit = suit;
		this.rank = rank;
	}
	
	public Suit getSuit() { return suit; }

	public Rank getRank() { return rank; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((rank == null) ? 0 : rank.hashCode());
		result = prime * result + ((suit == null) ? 0 : suit.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		System.out.println("IN EQUALS");
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Card other = (Card) obj;
		if (rank != other.rank)
			return false;
		if (suit != other.suit)
			return false;
		return true;
	}
	@Override
	public int compareTo(Card other) {
		System.out.println("IN COMPARETO method");
		return this.suit.compareTo(other.suit);
	}	
}
```

#### Output
```
Static Block
Instance Block
Constructor
```

### Modification Version - 19
#### CollectionTut.java
```java
public class CollectionTut {
	public static void main(String[] args) {
		Card c1 = new Card(Suit.HEART, Rank.QUEEN);
        Card c2 = new Card(Suit.HEART, Rank.QUEEN);
        Card c3 = new Card(Suit.HEART, Rank.QUEEN);
	}
}
```

#### Output
```
Static Block
Instance Block
Constructor
Instance Block
Constructor
Instance Block
Constructor
```

- adding few more instances of the class card
- Static Block gets called first; whenever we will access the class Card, the class loader loads the class in a memory; *Sandbox Model*
- **Sandbox Model** in Java means Java programs run in a completely closed box, unless you allow them to access outside resources they cannot.
- When it loads the class, Static Block gets called; now the class is loaded into the memory
- The next step is it is creating an instance of it, and then Instance Block got called and finally Constructor got Called.
- for the Second Instance, the static block is not called anymore, but the instance block and constructor gets called every time
    - Because class is already loaded, it doesn't need to load the class again.
    - Instance Block runs everytime when the class is loaded, but Static Block runs only one time.
- If there are multiple static block or instance block, the one which is at the top will be executed first; sequence
- Even if we shift the Constructor completely below or the Instance Block completely below, the Instance Block will get first executed and then the Constructor

### Modification Version - 20

#### Card.java
```java
public class Card implements Comparable<Card>{
	private Suit suit;
	private Rank rank;

	static {
		System.out.println("Static Block 2");
	}

	static {
		System.out.println("Static Block 1");
	}
	
	{
		System.out.println("Instance Block 2");
	}

	{
		System.out.println("Instance Block 1");
	}
	
	public Card(Suit suit, Rank rank) {
		super();
		System.out.println("Constructor");
		this.suit = suit;
		this.rank = rank;
	}
	
	public Suit getSuit() { return suit; }

	public Rank getRank() { return rank; }

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("(");
		builder.append(suit);
		builder.append(", ");
		builder.append(rank);
		builder.append(")");
		return builder.toString();
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((rank == null) ? 0 : rank.hashCode());
		result = prime * result + ((suit == null) ? 0 : suit.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		System.out.println("IN EQUALS");
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Card other = (Card) obj;
		if (rank != other.rank)
			return false;
		if (suit != other.suit)
			return false;
		return true;
	}
	@Override
	public int compareTo(Card other) {
		System.out.println("IN COMPARETO method");
		return this.suit.compareTo(other.suit);
	}	
}
```

#### Output
```
Static Block 2
Static Block 1
Instance Block 2
Instance Block 1
Constructor
Instance Block 2
Instance Block 1
Constructor
Instance Block 2
Instance Block 1
Constructor
```

----------------------------------

Always follow this sequence:
- Always put attributes first
- Then, Static Block
- Then, Instance Block
- Then, Constructor
- After Constructor, put the business methods
- Then, Getter Methods
- Then, Setter Methods
- and at last put the toString() method



