{:title "Embrace final"
:layout :post
:tags ["craft" "practice"]
}

Having immutability as the language feature by default is a very nice thing. That you know that everything you declare is immutable, and instead, you have to apply some "ceremony" to actually change the value of the thing, so it becomes very explicit to the reader that the value is changing.

Unfortunately Java doesn't have this by default. But Java has the `final` keyword (which has multiple semantics, to be precise, depends on the context), which can be used to embrace immutability into your codebase.

### Not very popular idea

I am very aware that the idea of declaring everything (well, almost everything, see the exceptions below) `final` is not very popular amongst Java developers. And the main reasons against they usually provide: 

- it makes code verbose
- if you write smaller methods you don't need `final` as it obvious what you assign to what

As you expect, I completely disagree with those statements. 

Verbosity is a matter of time or habit, the first time you start to put `final` everywhere it might look like a lot, but eventually you stop noticing it (like parenthesis in Clojure, that many people, I guess afraid :)

As I completely agree that methods should be small and concise, eventually over time they might become bigger, or refactored. Although still the average method will be from 5 to 10 lines of code (usually more) with some logic in it. So, those methods will benefit from using `final`. And being consistent - is a very important feature of the codebase. So, either same style/technique should be used everywhere or not used at all (probably the exception is the transition period, when the team is switching from one style/technique, or absence of style, into another style, and it is not feasible to change all the codebase at once, so they start write the new code using the new standards, and changing the old code every time they touch it).

Luckily I see more and more developers are in for using `final` everywhere and embracing immutability. Although still not a very common practice to see around. Which I would like to see happen more.

### The value of `final`

As I see, there are multiple values the team and product will benefit of using `final`:

1. the larger the codebase becomes the more the value of `final` becomes evident
1. it is all about helping to reason about the code, and focus on the logic, and not looking for the potential buggy assignment statement or potential defect
1. it delivers safety, that you know if something got a value, no matter what follows next the value will never change, so the reader can more easily move forward by reading the code, and jumping back and forth, and should not need to keep track of what and when, on what condition the variable was assigned/reassigned the value, but simply relying on the fact the it is immutable as soon as the value was set
1. less error prone concurrency
1. using `final` forces the writer to think about better design (even on the method level), which in turns improves the overall structure and design of the class (which is not obvious, but read for more further)

### What should be `final`

Almost everything:

- class fields
- method parameters
- local variables
- even declared exceptions in the catch block
- literally anything which can be declared final, except classes and methods

### Except classes and methods

This is about the intention. `final` on classes and methods has a clear intention that the writer wanted them NOT to be extendible for a reason. And when putting `final` on all the classes and methods the intention is lost, and it is no longer possible to distinguish between whether it was put for a reason, or just because of the rule.

This is the same as with the comment. They should be rare. But, if they are there, they should be gold. Same here. Something which is not typical around codebase should tell you about something, and should be valued.

So, this why I skip putting `final` by default on class and method levels. To preserve the value of intention and semantics of `final` on classes and methods.

### Better design

As I've mentioned above, that using `final` might help to design a better code. How? By giving the developer more time to think about what he is doing. If something is marked as `final` and it becomes very difficult to implement the method logic without breaking this rule (which, well I completely accept sometimes happens), but the first attempt should be not to remove `final`, but instead try to restructure the code differently. How specifically? This is all on the use case basis. But giving time to pause and think - is already a valuable by itself.

### Avoid using `null`

Usage of null should be avoided. And `final` helps to reduce the number of use cases where you have to use `null`.

Although I am not that extreme and say that `null` should not be never used. Not at all. For me, I have some rules and believes (which is using `final` everywhere, for example), and when it is possible to achieve those I am happy. If there are clear reasons, or will be beneficial to break those, it might not be nice, but I am for balance between the pure theory and pragmatic results (but I first will make efforts to find another way of doing/designing things).

### Do I ever break the rule?

Not very often. I would say 99% percent of a time it is possible to declare everything `final`. But there is still 1% where this becomes difficult to achieve. But, then, if it is an agreed practice to use `final` everywhere, this 1% doesn't become hidden, but very noticeable by everybody, so:

1. everybody knows something special is happening here
1. can always give it a try, and come with idea how to achieve the same without breaking the rule

### Don't trust humans

Don't trust humans to remember of putting the `final` everywhere. Use static code analysis tools which will do that for them. Checkstyle, for example. Automate the process (very important), and be strict on it - the build should fail every time the rule is violated, until explicitly suppressed (which should be avoided, but allowed).

These are Checkstyle checks related to the usage of `final` I use:

```
<module name="FinalLocalVariable">
    <property name="validateEnhancedForLoopVariable" value="true"/>
</module>
<module name="FinalParameters">
    <property name="tokens" value="CTOR_DEF, METHOD_DEF, LITERAL_CATCH"/>
    <property name="ignorePrimitiveTypes" value="false"/>
</module>
<module name="FinalClass"/>
```

The full [checkstyle.xml](https://gist.github.com/zshamrock/5ce5715f163ff296b583b1ac25b4351b) can be taken from Gist (for all the available checks consult [official doc](http://checkstyle.sourceforge.net/checks.html)).

And the corresponding Maven setup:

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>2.17</version>
    <dependencies>
        <dependency>
            <groupId>com.puppycrawl.tools</groupId>
            <artifactId>checkstyle</artifactId>
            <version>${checkstyle.version}</version>
        </dependency>
    </dependencies>
    <executions>
        <execution>
            <id>checkstyle-validation</id>
            <phase>validate</phase>
            <configuration>
                <configLocation>config/checkstyle/checkstyle.xml</configLocation>
                <encoding>UTF-8</encoding>
                <consoleOutput>true</consoleOutput>
                <failsOnError>true</failsOnError>
                <includeTestSourceDirectory>true</includeTestSourceDirectory>
                <skip>${checkstyle.skip}</skip>
            </configuration>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

### A few typical examples

Examples below - just a few typical use cases where it might be tempting to skip `final` and use `null`, but look how it could be structured differently.

#### if/else

```
final String s;
if (n % 2 == 0) {
    s = "even";
} else {
    s = "odd";
}
System.out.println(s);
```

or even better 

```
final String s = n % 2 ? "even" : "odd";
```

versus 

```
String s;
if (n % 2 == 0) {
    s = "even";
} else {
    s = "odd";
}
System.out.println(s);
```

Same logic is applicable to the `switch` statement.

#### assignment in try/catch

```
private void method1(final int n) {
    final Optional<String> s = method2();
    s.ifPresent(System.out::println);
}

private Optional<String> method2() {
    try {
        return Optional.of(canThrow());
    } catch (final IOException ex) {
        ex.printStackTrace();
        return Optional.empty();
    }
}

private String canThrow() throws IOException {
    return "Safe";
}
```

versus 

```
private void method1(final int n) {
    String s = null;
    try {
        s = canThrow();
    } catch (final IOException ex) {
        ex.printStackTrace();
    }
    System.out.println(s);
}
```

Obviously, it is not the complete examples set, but just a few came into my mind at the moment.

***

Over time you probably start having the opposite feeling, that if something is not declared `final`, it might start feeling like something incomplete, something "naked". And don't give up in the beginning, it will pay off in the future, where you will be glad you declared things `final`. Be creative, when there is a temptation to break the rule, it might be difficult in the beginning, messy in the middle, and gorgeous in the end :)

Have a nice, productive week!
