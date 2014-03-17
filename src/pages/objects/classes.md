---
layout: page
title: Classes
---

A class is a template for creating objects. When we define a class we can create objects that all have similar methods and instance variables and, importantly, all have the same type. This means we can pass different objects with the same class to a method expecting that class. If you did the exercises in the previous section you will have seen this issue.

## Defining a Class

Here is a simple class definition:

~~~ scala
scala> class Person {
     |   val firstName = "Noel"
     |   val lastName = "Welsh"
     |   def name = firstName + " " + lastName
     | }
defined class Person
~~~

Let's look at what we have. The class is called `Person`. We can create a new `Person` object using the `new` operator as follows:

~~~ scala
scala> val noel = new Person
noel: Person = Person@3235186a
~~~

We can access the instance variables and members of this class in the usual way:

~~~ scala
scala> noel.firstName
res26: String = Noel
~~~

Notice the type of the object `noel` is `Person`

~~~ scala
scala> :type noel
Person
~~~

This means we can write methods that take type `Person` and pass in objects to it.

~~~ scala
scala> def sayHello(person: Person) =
     |   "Hello " + person.firstName
sayHello: (person: Person)String

scala> sayHello(noel)
res27: String = Hello Noel
~~~

## Constructors

As it stands our `Person` class is rather useless. The `firstName` and `lastName` can't be changed when we create the object. What if we want to model a person with a different name?

The solution is to introduce a constructor, which allows us to pass in to the class objects that will be used to customise the object that is created. That's a long sentence for saying we can set the names when we create the class.

Here's how we write it:

~~~ scala
scala> class Person(val firstName: String, val lastName: String) {
     |   def name = firstName + " " + lastName
     | }
defined class Person

scala> val dave = new Person("Dave", "Gurnell")
dave: Person = Person@3ed12df7

scala> dave.name
res29: String = Dave Gurnell
~~~

## Exercises

We now have enough machinery to have some fun playing with classes.

#### A Simple Counter

Implement a `Counter` class. The constructor should take an `Int`. The methods `inc` and `dec` should increment and decrement the counter respectively, returning a new `Counter`.

Here's an example of usage:

~~~ scala
scala> new Counter(10).inc.dec.inc.inc.count
res42: Int = 12
~~~

<div class="solution">
~~~ scala
class Counter(val count: Int) {
  def dec = new Counter(count - 1)
  def inc = new Counter(count + 1)
}
~~~

There are two points to this exercise. The first goal is to get some practice creating a simple class definition. The second point is think about why this class returns a new class, rather than updating the counter directly. We haven't seen any syntax for setting a variable yet.
</div>


#### Fun with Constructors

What happens if we remove the `val`s from the constructor? Something no longer works, but what exactly? Hint: try accessing constructor arguments from inside and outside the object.

<div class="solution">
The constructor arguments are not visible outside the class.
</div>


#### More Constructors

We can split a `String` into an `Array` of components as follows:

~~~ scala
scala> val parts = "John Doe".split(" ")
parts: Array[String] = Array(John, Doe)

scala> parts(0)
res36: String = John
~~~

Create a class, called a `PersonFactory` with a single method with signature `create(name: String): Person`. When `create` is called with a `String` it should split the `String` at the space character and construct a `Person`. Here's an example of use:

~~~ scala
scala> val factory = new PersonFactory()
factory: PersonFactory = PersonFactory@4bce79b8

scala> val john = factory.create("John Doe")
john: Person = Person@2fe6a820

scala> john.name
res38: String = John Doe
~~~

<div class="solution">
~~~ scala
class PersonFactory {
  def create(name: String): Person = {
    val parts = name.split(" ")
    new Person(parts(0), parts(1))
  }
}
~~~

This construction is a bit awkward. We have to create a new class just so we can create another class. It would be nice to make this simpler.
</div>

#### Extending our Counter

Here's a simple class

~~~ scala
class Adder(amount: Int) {
  def apply(in: Int) = in + amount
}
~~~

Extend your `Counter` class to have a method called `map`. This method should accept an `Adder` and return a new `Counter` with the result of applying the `Adder` to the `count`.

<div class="solution">
~~~ scala
class Counter(val count: Int) {
  def dec = new Counter(count - 1)
  def inc = new Counter(count + 1)
  def map(adder: Adder) =
    new Counter(adder(count))
}
~~~

Notice I called the `apply` method without naming it. In Scala, a method `apply` on an object `foo` can be called just by writing `foo()`. This allows some objects to look just like functions! Remember from the last chapter that methods are not values. But objects are. So we can pass around objects that act like functions! Interesting.

It would be nice to generalise `map` to take any function that takes an `Int` and produces an `Int`. We'll see how to do that later.
</div>