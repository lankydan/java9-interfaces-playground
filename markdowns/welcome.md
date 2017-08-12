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
As you can see from the basic snippets above a default method is defined on the interface which in turn calls a private method. As the name suggests it is a default method and therefore due to `MyClassWithDefaultMethod` not providing an implementation for defaultMethod it will carry on and use what is defined on the interface. So if defaultMethod and normalMethod were called they would produce the following output.

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
Thats the basics of adding default and private methods to interfaces. Below we will look a bit more into adding methods with different access modifiers and some reasons for using default methods in the first place.


default methods can still be overridden as normal
provide a default implementation if not replaced (as the name suggests)
add context to why default methods are useful (added to jdk8 to help in adding features to the jdk), a class can inherit from multiple interfaces and therefore the default methods can help provide code that can easily be added to classes via implementing interfaces
private methods are used like normal private methods, simply let you reuse and tidy up code within the interface. Which was something that was missing from default methods in java8 which could lead to long default methods.
what happens if a private or package private method is added to the class who's interface has a default method of the same name
try and find a decent use case for default + private methods, but start with the basic example
mention static methods, but I dont really see much point of them

This Java template lets you get started quickly with a simple working example using Maven and JUnit. If it is your first contribution then you should have a look at the [Getting Started](https://tech.io/doc/getting-started-create-playground) document.


The source code is on [GitHub](https://github.com/TechDotIO/java-template), please feel free to come up with proposals to improve it.

# Hands-on Demo

@[Luke, how many stars are there in these galaxies?]({"stubs": ["src/main/java/com/yourself/Universe.java"], "command": "com.yourself.UniverseTest#test"})

Check out the markdown file [`welcome.md`](https://github.com/TechDotIO/java-template/blob/master/markdowns/welcome.md) to see how this exercise is injected into the template.

# Template Resources

[`markdowns/welcome.md`](https://github.com/TechDotIO/java-template/blob/master/markdowns/welcome.md)
What you are reading here is generated by this file. Tech.io uses the [Markdown syntax](https://tech.io/doc/reference-markdowns) to render text, media and to inject programming exercises.


[`java-project`](https://github.com/TechDotIO/java-template/tree/master/java-project)
A simple Java + Maven + JUnit project dedicated to run the programming exercise above. A project relies on a Docker image to run. You can find images on the [Docker Hub](https://hub.docker.com/explore/) or you can even [build your own](https://tech.io/doc/reference-runner).


[`techio.yml`](https://github.com/TechDotIO/java-template/blob/master/techio.yml)
This *mandatory* file describes both the table of content and the programming project(s). The file path should not be changed.


# Visual and Interactive Content

Tech.io provides all the tools to embed visual and interactive content like a Web app or a Unix terminal within your contribution. Please refer to the [documentation](https://tech.io/doc) to learn more about the viewer integrations.
