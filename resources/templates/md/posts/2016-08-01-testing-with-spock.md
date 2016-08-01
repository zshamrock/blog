{:title "Testing with Spock"
:layout :post
:tags ["practice"]
}

Lets start with a profound statement that one day would be nice to see Spock Framework to become the de-facto testing framework for JVM languages, at least for Java.

For many years, I, as many other Java developers, was using JUnit, Mockito, Hamcrest to power unit and integration testing. The day I found Spock - changed everything :) (at least in the area of testing).

Nowadays already used Spock for my personal projects and as well in production, I can't imagine going back to JUnit and Hamcrest for writing unit tests. JUnit5 or AssertJ don't change anything. So, why you are not still using Spock for unit testing (which I really would like to see happening more and more), keep reading and I hope after this post you at least will give it a try.

### Why Spock

Spock is not only elegant, powerful and simple, but it also very descriptive, and it is a pleasure to write tests in it (how often you have this same feeling writing tests in JUnit?). 

Moreover tests written in Spock are:

- maintainable
- easy to write
- easy to read
- easy to modify
- descriptive
- save time

The last point probably needs a little bit of an explanation. Writing tests in Spock is fast due to the fact that you actually have to type much less, so you save lots of time on typing. I can seriously imagine writing same quality of tests using JUnit easily can double the total time, and probably triple the code written (and remember less code is always better).

### You can learn it literally in 5-10 minutes

You might be exciting about Spock, and really want give it a try. But you might think: "I am so busy at work, and I don't have time to learn yet another testing framework".

First of all, see above, Spock can easily pays off the time taken to learn it. And second, to learn Spock takes for about 5-10 minutes (just the time to read 4 pages of the documentation [Spock Framework Reference Documentation](http://spockframework.github.io/spock/docs/1.1-rc-1/index.html)). I told you, it is very simple in nature, but very powerful (every time I see new testing framework coming on the horizon - none of them have the same level of quality, simplicity and power Spock has).

### Arrange Act Assert

Before using Spock or in the projects where due to already big heritage of tests written in JUnit it was not possible to introduce Spock I was using and introduced into JUnit the concept of Arrange Act Assert, where your JUnit test looks like:

```
// Arrange
final TimeMachine machine = new TimeMachine();

// Act
machine.run(); // only one action per test

// Assert
assertEquals(...);
```

It makes JUnit tests easier to maintain and read, specially during the code review.

Even though not everybody in the team liked this. Some people told that you can use blank line to designate end of one section and mark the beginning of the next section. Other people just simply didn't like to use comments for the code organization and structure. Although I generally agree with those both points of view in the context of production code, in case of Arrange Act Assert the value it brings outweighs the "drawbacks" people complained.

Luckily there were people who liked the idea, and helped to spread this technique further over organization. Better than nothing, if it is not possible to use Spock.

### Setup/Given When Then

So, how Assert Act Arrange corresponds to Spock. Actually Spock has `setup:/given:`, `when:/expect:`, `then:` blocks, and they are not comments, but the language/framework constructs. Moreover you can attach the description to each of the block and could have multiple of them to better express the intention and group the logic, i.e.

```
setup: "optional description of the 'setup' block"
...

and: "you can use 'and' just simply group related logic and make 'setup' or 'then' or any other section is even more readable"

when: "optional description of the 'when' block"
...

then: "optional description of the 'then' block"

```

### Assertions

In Spock 99% of the time you don't need to type `assert` or `assertEquals` or any kind of assert function or statement in order to do the assertion, just simply write:

```
expected == actual
```

This is it. Much simpler, logical and cleaner. Assertion can be any expression in `then:` block which results in boolean value.

Yet, there are use cases when you do need type the `assert` explicitly, in any nested blocks (like `if` or closures) or methods, but again compare `assert expected == actual` vs `assertEquals(expected, actual)` in JUnit. The former looks much nicer to me (and easier to type).

#### Assertion errors

Also, Spock has very helpful assertion errors, if some of the assertion fails, i.e.

```
Condition not satisfied:

session.allPlayers == ["a", "b", "c", "x", "y", "w"] as Set
|       |          |
|       |          false
|       [a, b, c, x, y, z]
Session(type=DEMO, id=f47ba33f-71a6-46bc-be29-84960d3b6e1d, startTimestamp=1469659683885, endTimestamp=1469659683886, ... 
```

See, how beautiful and helpful it is (it actually prints the whole content of the Session, this is me truncated it for the purpose of this blog post).

So, no more guessing or debugging the exact values of the objects used in the assertion. You have everything you need just there in the failure report.

### Data driven testing

This is one of my favorite feature of the Spock - data tables.

Here is the example:

```
expect:
Math.max(a, b) == c

where:
a  | b  || c
10 | 20 || 20
0  | 0  || 0
-1 | -5 || -1
```

Of course, example is very simple, but the data table can be very rich in data. You can go much beyond of this simple example above. And moreover it is very readable and easy to maintain. Imagine doing this with JUnit (yes, JUnit has the concept of parametrized tests, but first you should create a separate test class for each of the data table, and second is far less readable and powerful comparing to Spock).

### Mocking

Mocking with Spock is also excellent. Although I do not use Mocks a lot in my tests, the ease and elegance of mocking in Spock is far more from what other frameworks or libraries could offer. Really, after using Spock mocking features for a while coming back to Mockito feels like coming back for 100 years ago. So primitive and boilerplate they look like.

### How to setup Spock?

If you are using Maven add the following into pom.xml (taken from here [pom.xml](https://github.com/spockframework/spock-example/blob/master/pom.xml), also check this [spock-example](https://github.com/spockframework/spock-example) for Gradle setup and other Maven options you might be interested):

```

<properties>
    <groovy.major.version>2.4</groovy.major.version>
    <groovy.patch.version>6</groovy.patch.version>
    <groovy.version>${groovy.major.version}.${groovy.patch.version}</groovy.version>
    <spockframework.version>1.0-groovy-${groovy.major.version}</spockframework.version>
</properties>
...
<dependencies>
    <dependency>
        <groupId>org.spockframework</groupId>
        <artifactId>spock-core</artifactId>
        <version>${spockframework.version}</version>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.codehaus.groovy</groupId>
        <artifactId>groovy-all</artifactId>
        <version>${groovy.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>
...
<build>
    <plugins>
        <!-- Spock -->
        <!-- Mandatory plugins for using Spock -->
        <plugin>
            <!-- The gmavenplus plugin is used to compile Groovy code. To learn more about this plugin,
            visit https://github.com/groovy/GMavenPlus/wiki -->
            <groupId>org.codehaus.gmavenplus</groupId>
            <artifactId>gmavenplus-plugin</artifactId>
            <version>1.5</version>
            <dependencies>
                <dependency>
                    <groupId>org.codehaus.groovy</groupId>
                    <artifactId>groovy-all</artifactId>
                    <version>${groovy.version}</version>
                    <scope>runtime</scope>
                </dependency>
            </dependencies>
            <executions>
                <execution>
                    <goals>
                        <goal>compile</goal>
                        <goal>testCompile</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        
        <!-- Optional plugins for using Spock -->
        <!-- Only required if names of spec classes don't match default Surefire patterns (`*Test` etc.) -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.6</version>
            <configuration>                
                <systemPropertyVariables>
                    <java.awt.headless>true</java.awt.headless> <!-- Not related to Spock, but good to have -->
                </systemPropertyVariables>
                <includes>
                    <include>**/*Spec.java</include>
                </includes>
                <excludes>
                    <exclude>**/*ITSpec.java</exclude>
                </excludes>
            </configuration>
        </plugin>

        <!--
            The Failsafe Plugin is designed to run integration tests while the Surefire Plugin is designed
            to run unit tests. The name (failsafe) was chosen both because it is a synonym of surefire and
            because it implies that when it fails, it does so in a safe way.
         -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>2.19.1</version>
            <executions>
                <execution>
                    <id>integration-test</id>
                    <goals>
                        <goal>integration-test</goal>
                        <goal>verify</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>                
                <systemPropertyVariables>
                    <java.awt.headless>true</java.awt.headless>
                </systemPropertyVariables>
                <includes>
                    <include>**/*ITSpec.java</include>
                </includes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### Integration with Spring

Works pretty well. Testing with Spock our Spring Boot application. Works like a charm. Simply include:

```
<dependency>
    <groupId>org.spockframework</groupId>
    <artifactId>spock-spring</artifactId>
    <version>${spockframework.version}</version>
    <scope>test</scope>
</dependency>
```

into your Maven dependencies. And add 
```
@SpringApplicationConfiguration
@IntegrationTest
```

on top of your test specification (it actually depends on what kind Spring integration testing you are doing).

### Groovy?

I guess one of the reason (or just another reason) why some people might avoid Spock - is the necessity to learn Groovy. Or they might think, that they don't have enough time to learn yet another language. The good news is that all Groovy you need to know is nicely summarized in one page: [Style Guide](http://groovy-lang.org/style-guide.html). And just a side note no matter which language you use, even though the language gives you powerful constructs it doesn't necessary mean you have to use it. And this is even more critical for the tests. To keep them simple and clean. So, this Groovy background will be enough to write clean and simple Spock tests that everyone in the team can understand. And this is the point.

### What about Kotlin Spek? Or other JVM lang testing frameworks?

Yes, there are other JVM languages, which you might target or which might look more attractive to you. Although none of them is that simple to learn and use as Groovy (both Kotlin and Scala, for example, have a higher learning curve). But more importantly the existing testing frameworks are far from simplicity, power and beauty of Spock. Although Spek if putting more efforts one day could result in something hopefully close to Spock, so far Spock is the clear winner in this space.


***
So join the efforts and make Spock the de-facto testing framework for JVM languages :) What are you waiting for? :) But, seriously give it a try. I promise you won't want come back to JUnit after.

