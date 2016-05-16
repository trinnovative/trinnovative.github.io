---
layout: post
title: Testing Java Applications with Spock
author: jschenkl
category: Java
tags: [Spock, Testing, BDD, Groovy, Java]
comments: true
---
Spock is a testing framework building on the concepts of Behavior Driven Development (BDD). It uses the Groovy programming language to allow writing tests in a concise manner. It offers an alternative to commonly used testing frameworks like JUnit or TestNG.

Spock was created by Peter Niederwieser, a software engineer at Gradleware. Spock's current version is 1.0.

-----

# Introduction
  
<figure style="float:right;">
   <img src='/public/img/leonard_nimoy_spock_1967.jpg' alt='Leonard Nimoy as Mr. Spock'/>
   <figcaption>Mr. Spock (source <a alt="Wikipedia" href="https://commons.wikimedia.org/wiki/File:Leonard_Nimoy_Spock_1967.jpg">Wikipedia</a>)</figcaption>
</figure>

## Why Spock?
Spock provides an interesting alternative to testing frameworks like JUnit or TestNG. In contrast to those tools, it uses the Groovy programming language. Groovy is a dynamic language that supports static-typing and static compilation. The main benefit of using Groovy is that it's more concise and expressive than plain Java.

## Pros:
Some of the benefits of Spock are:

* Concise and expressive language (Groovy)
* Detailed test output
* Mocking is easy
* Extensible

## Cons:
The benefits of using Spock don't come without a price. It requires at least partially learning a new language: Groovy.

# Getting Started
Lets get started with a simple (maybe a bit contrived) example. Suppose we want to develop a simple calculator that supports arithmetic operations.

## A Simple Example
A possible implementation for the ``plus`` operation could look like this:

{%highlight Java%}
package de.trinnovative.java.spock;

public class Calculator {

    public double plus(double lhs, double rhs) {
        return lhs + rhs;
    }
    
}
{% endhighlight %}

## Project Setup

For using Spock with gradle, setup is as easy as this:

{% highlight groovy %}
apply plugin: "groovy"

version = "1.0"
description = "A Spock example project"

sourceCompatibility = 1.8

repositories {
  jcenter()
}

dependencies {
  compile "org.codehaus.groovy:groovy-all:2.4.1"
  testCompile "org.spockframework:spock-core:1.0-groovy-2.4"
}
{% endhighlight %}

By this we're enabling Groovy support for our project and include the Spock framework dependencies.

## A First Test Specification
For our calculator example we can easily specify test cases with Spock. We setup our test object as an instance field and define a first feature method. Spock creates a new instance of our test object for each feature method.
 
{%highlight Groovy%}
 package de.trinnovative.java.spock
 
 import spock.lang.Specification
 
 class CalculatorSpecification extends Specification {
 
     def Calculator calc = new Calculator()
 
     def "1 plus 2 should be equal to 3"() {
 
         given:
         Calculator calc = new Calculator()
 
         when:
         def result = calc.plus(1, 2)
 
         then:
         result == 3
     }
 }
{% endhighlight %}
Each feature method is divided in three blocks:

* _given:_ is used to setup the feature's test fixtures. This is the fixed state that has to be prepared before testing.
* _when:_ is used to define the interaction with the system under test.
* _then:_ is used to describe the expected response of the system when the interaction is applied. In the `then` block asserts can be specified with `==`.
 
 
If a test fails in Eclipse, expected and actual value as well as the used comparison operator are reported.

![Failed Spock test in Eclipse](/public/img/spock_failed_test_eclipse.png)


# Exception Testing

Exception testing is also pretty straightforward as shown in the next example.

{%highlight Groovy%}

class CalculatorSpecification extends Specification {

    def Calculator calc = new Calculator()

    def "division by zero should not be allowed"() {

        when:
        calc.div(1, 0)

        then:
        def ex = thrown(IllegalArgumentException)
    }
}
{% endhighlight %}
Compared to JUnit's `@Test(expected=IllegalArgumentException.class)` or `ExpectedException` rule that's really a nice way to do exception testing. As `thrown` returns the thrown exception we can also easily refine our expectations.

# Data Driven Testing
If we want to test multiple data combinations in one test, we can resort to Data Driven Testing. Spock supports this out of the box by using so called Data Tables. Due to Spock's expressiveness, the next example is quite self-explanatory.

{%highlight Groovy%}
package de.trinnovative.java.spock

import spock.lang.Specification

class CalculatorSpecification extends Specification {

    def Calculator calc = new Calculator()

    def "a plus b should be equal to c"() {

        expect:
        calc.plus(a, b) == c

        where:
        a  |  b    ||  c
        1  |  2    ||  3
        2  | -2    ||  0
        -1  | -2    || -3
    }
}
{% endhighlight %}
A data driven test consists of two blocks:

*  _expect:_ defines the code that shall be tested and defines input (a,b) and output (c) variables
*  _where:_ provides the different examples or data values to be used for constructing the test cases

For each example in the table an isolated test is executed, the same way as if we would have defined separate test features.

# Conclusion
In this post we've had a glance at the Spock testing framework. It provides a simple and clean API to write tests for Java classes in Groovy. We've seen that exception and data driven testing can be easily achieved with Spock.

More advanced topics like mocking or integration tests will be covered in one of the upcoming posts.