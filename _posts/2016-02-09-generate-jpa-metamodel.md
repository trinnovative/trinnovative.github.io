---
layout: post
title: Automatic JPA 2 Metamodel Generation Using Hibernate 5 and Gradle
author: jschenkl
category: Java
tags: [Java, Gradle, JPA, Hibernate, Spring Data]
---

The *JPA 2 Metamodel* provides a way to create JPA Criteria Queries in a type-safe manner. For Maven, there's an [integration provided directly by Hibernate](https://docs.jboss.org/hibernate/orm/5.0/topical/html/metamodelgen/MetamodelGenerator.html). In this post, we'll have a look at automatically generating the necessary JPA Metamodel classes with the [Gradle](http://gradle.org/) build automation tool. Moreover, we'll see how to use them in [Spring Data JPA](http://projects.spring.io/spring-data-jpa/) Specifications.

-----

### Preliminary
Suppose we have modeled simple domain entity `Person`:

{% highlight java %}
@Entity
public class Person {

    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private long id;
    private String firstName;
    private String lastName;

    public Person(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
{% endhighlight %}

For that entity, we want to generate a metamodel class like the following:

{% highlight java %}
@Generated(value = "org.hibernate.jpamodelgen.JPAMetaModelEntityProcessor")
@StaticMetamodel(Person.class)
public abstract class Person_ {

	public static volatile SingularAttribute<Person, String> firstName;
	public static volatile SingularAttribute<Person, String> lastName;
	public static volatile SingularAttribute<Person, Long> id;

}
{% endhighlight %}


### Setting up the Gradle build file
To automate the generation process with Gradle, we use the [jpamodelgen-plugin](https://github.com/iboyko/gradle-plugins/tree/master/jpamodelgen-plugin). That plugin supports JPA Metamodel generation with Hibernate as the persistence provider out of the box.

With a little gradle scripting, the build.gradle file looks as follows:

{% highlight groovy %}
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.2.RELEASE")
    }
}

plugins {
  id "at.comm_unity.gradle.plugins.jpamodelgen" version "1.1.1"
}

apply plugin: 'spring-boot'
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'at.comm_unity.gradle.plugins.jpamodelgen'

repositories {
    jcenter()
    maven { url "https://repository.jboss.org/nexus/content/repositories/releases" }
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile("org.springframework.boot:spring-boot-starter-data-jpa")
    compile("com.h2database:h2")
    testCompile("junit:junit")
}

sourceSets {
   generated {
        java.srcDir "${buildDir}/generated/src/java/"
    }
}

ext['hibernate.version'] = '5.0.7.Final'

jpaModelgen {
  library = "org.hibernate:hibernate-jpamodelgen:5.0.7.Final"
  jpaModelgenSourcesDir = "src/generated/java"
}

compileJava.options.compilerArgs += ["-proc:none"]
{% endhighlight %}

The plugin already provides an integration in the Gradle build lifecycle. The generation happens before the compileJava task. Moreover, the generated files are deleted when executing the clean task.

Please note, that I've switched Hibernate version 5.0.7.Final. Moreover, I've added a separated sourceSet for the generated sources, so that they're available on the classpath.


### Usage with Spring Data
After all that setup, it is easy to use the Metamodel classes with Spring Data Specifications:

{% highlight groovy %}
Specification<Person> byLastName(String lastName) {
    return (Root<Person> root, CriteriaQuery<?> query, CriteriaBuilder cb) -> {

        return cb.equal(root.get(Person_.lastName), lastName);
    };
}
{% endhighlight %}

### Conclusion
As we've seen, Metamodel generation can be easily integrated into the Gradle build process. The plugin mechanism and scripting features of Gradle come in pretty handy here.
If you are interested in writing more fluid queries than possible with the Criteria API, you should definitely have a look at the [QueryDSL](http://www.querydsl.com/) project.

### References
- [Gradle Homepage](http://gradle.org/)
- [jpamodelgen-plugin](https://github.com/iboyko/gradle-plugins/tree/master/jpamodelgen-plugin)
- [Hibernate JPA Metamodel Generation](https://docs.jboss.org/hibernate/orm/5.0/topical/html/metamodelgen/MetamodelGenerator.html) (with Maven)
- [Spring Data JPA Homepage](http://projects.spring.io/spring-data-jpa/)
