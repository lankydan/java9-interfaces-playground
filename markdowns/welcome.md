# Welcome!

In this tutorial we will look at default and private methods within interfaces. Default methods were added in Java 8 allowing methods to be added to an interface that comes with a default implementation that could be used, overridden or ignored without causing issues to the existing classes that have implemented an interface. Private methods were something that was missing when this feature was added as code could not be split out into smaller methods within an interface. This is something that was a bit off putting to me as if you had a default method that became a bit long there was no way to tidy it up. So now that both default and private methods can exist within an interface we can write methods like we are used to, although if you haven't used default methods yet then you will first need to get past the fact that there is now actual code living in an interface.

In terms of adding default and private methods to an interface, its really simple. To add a default method just add the keyword `default` to the method definition and I'm not even going to tell you how to add a private method as I don't wish to insult you!

Being it's so simple lets just jump straight into some code examples.

```java
public interface MyInterface {

  default void defaultMethod() {
    privateMethod("Hello from the default method!");
  }

  private void privateMethod(final String string) {
    System.out.println(string);
  }

  void normalMethod();
}
```

```java
public class MyClassWithDefaultMethod implements MyInterface {

  @Override
  public void normalMethod() {
    System.out.println("Hello from the implemented method!");
  }
}
```
As you can see from the basic snippets above a default method is defined on the interface which in turn calls a private method. As the name suggests it is a default method and therefore due to `MyClassWithDefaultMethod` not providing an implementation for defaultMethod it will carry on and use what is defined on the interface. So if `defaultMethod` and `normalMethod` were called they would produce the following output.

```
Hello from the default method!
Hello from the implemented method!
```
There is really not much to it. If you wanted to provide your own implementation of the default method within the class then as you would with a normal interface method, just add a method with the same name and preferably pop the `@Override` annotation on top of it.

```java
public class MyClassOverrideDefaultMethod implements MyInterface {

  @Override
  public void normalMethod() {
    System.out.println("Hello from the implemented method!");
  }

  @Override
  public void defaultMethod() {
    System.out.println("I have overridden the default method!!");
  }
}
```
Nothing really to say about this code as now it looks like a normal class that has implemented an interface. When ran it will produce the following output rather than what was show previously.

```
I have overridden the default method!!
Hello from the implemented method!
```
Thats the basics of adding default and private methods to interfaces. Below we will look a bit more in depth into default methods as well as some reasons for using them in the first place.

So a class can have multiple interfaces and now an interface can define it's own default methods. What happens if a class implements multiple interfaces which each have the same default method defined on their interfaces? Well not much happens really as it will fail to compile and produce the following error.

```
java: class MyClassWithTwoInterfaces inherits unrelated defaults for defaultMethod() from types MyInterface and MyOtherInterface
```
As you can probably figure out, it cannot determine which default method it should actually use so it blows up. To get around this situation we need need to provide an implementation that will override the versions that the interfaces are giving the class which will be used instead and thus removing the ambiguity which resulted in the error.

Moving onto the next point. A class can override a default interface method and call the original method by using `super`, keeping it nicely in line with calling a super method from an extended class. But there is one catch, you need to put the name of the interface before calling `super` this is necessary even if only one interface is added. This makes sense as `super` normally refers to the extended class and as multiple interfaces are allowed restricting it to always being called like this keeps it consistent and without any ambiguity. Below is a code snippet of what this would look like.

```java
public class MyClassWithTwoInterfaces implements MyInterface, MyOtherInterface {

  @Override
  public void normalMethod() {
    // some implementation
  }

  @Override
  public void defaultMethod() {
    MyInterface.super.defaultMethod();
    MyOtherInterface.super.defaultMethod();
  }
}
```
Being able to call `super` on a default interface method also resolves the error the same default method from multiple interfaces. You will still need to override the method itself but you dont need to write much more than that, if one of the original versions satisfies your needs you can call `super` on the implementation you desire and be done with it. 

So why use default methods in the first place? After doing some googling, it seems that they were added to Java 8 as a way of adding methods in preparation of Lambda expressions without breaking code that have implemented existing interfaces. So if we had a class that used an interface that was changed, for example code within a 3rd party library, we wouldn't need to worry about any new methods being added to the interface as our existing code will still work and the new method can be ignored until a later date. By using this example I believe it is more important for API/library developers to consider using default methods than it is for those that are writing code within their own codebase where they are in control of everything and can change any classes that have been broken by adding a new interface method.

To make the point I raised above about keeping the updated interface compatible with existing implementations, I have added a code example below to help make this clearer.

```java
public class MyClass implements MyOldInterface {

	@Override
	public void doStuff() {
		// does stuff
	}
}
```
```java
public interface MyOldInterface {

	void doStuff();
}
```
So here we have a class that has sitting around nice and happy as it has implemented the method defined on the interface and requires no other work to be done. And then we come along without a care in the world and add a new method to the interface.

```java
public interface MyNewInterface {

	void doStuff();

  void doSomeMore();
}
```
When we try to compile the code it will fail as we have not provided an implementation for `doSomeMore`. Obviously this makes us cry as we need to write some extra code, even if it is some half arsed code just so we can get it to work again. Whereas if the code before was written instead...

```java
public interface MyNewInterface {

	void doStuff();

  default void doSomeMore() {
    // do some more stuff
  };
}
```
We would remain happy as our existing class will require no extra work and will still compile. When we eventually decide that we need to implement the new method added to the interface then we can do so as normal.

default methods can still be overridden as normal
provide a default implementation if not replaced (as the name suggests)
add context to why default methods are useful (added to jdk8 to help in adding features to the jdk), a class can inherit from multiple interfaces and therefore the default methods can help provide code that can easily be added to classes via implementing interfaces
private methods are used like normal private methods, simply let you reuse and tidy up code within the interface. Which was something that was missing from default methods in java8 which could lead to long default methods.
what happens if a private or package private method is added to the class who's interface has a default method of the same name
try and find a decent use case for default + private methods, but start with the basic example
mention static methods, but I dont really see much point of them