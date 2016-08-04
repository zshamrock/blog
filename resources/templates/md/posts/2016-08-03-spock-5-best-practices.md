{:title "Spock 5 best practices"
:layout :post
:tags ["practice"]
}

In one of my previous post I wrote about [the power and elegance of Spock](/posts/2016-08-01-testing-with-spock.html) testing framework. Here I will share 5 best practices we are in the team came with.

### Practice #1: Use explicit name arguments for the data tables

As I've mentioned before data tables - is one of my favorite feature of Spock. To remind you it looks like this (also some other features of Spock are demonstrated in the example below):

```
@Unroll
def "finding max value between #a and #b"() {
    expect:
    Math.max(a, b) == c

    where:
    a  | b  || c 
    10 | 5  || 10
    -5 | -1 || -1
    0  | 0  || 0
    -5 | 7  || 7
}
```

The above example, is perfectly correct one. Although when you use the data table, it is much more better to declare data tables elements as arguments of the method. It gives the reader an immediate sign that data table is being used, and set the expectations of the data.

So, the example above better will be rewritten like the following:

```
@Unroll
def "finding max value between #a and #b"(int a, int b, int c) {
    expect:
    Math.max(a, b) == c

    where:
    a  | b  || c 
    10 | 5  || 10
    -5 | -1 || -1
    0  | 0  || 0
    -5 | 7  || 7
}
```

### Practice #2: Use `||` to separate input and output elements in the data table

Again `||` is just a syntactical sugar, if you wish, although it is a good way of documenting what are the input and what are the output arguments of the data.

So, use:

```
    where:
    a  | b  || c 
    10 | 5  || 10
    -5 | -1 || -1
    0  | 0  || 0
    -5 | 7  || 7
```

instead of 


```
    where:
    a  | b  | c 
    10 | 5  | 10
    -5 | -1 | -1
    0  | 0  | 0
    -5 | 7  | 7
```

where the former is more readable.

### Practice #3: Decide on what use `setup:` or `given:` and use it consistently

Spock allows you to use both `setup:` and `given:` labels to describe the setup block of the test (aka Arrange part). The `given:` label is an alias for `setup:`. Although as it says in the documentation using `given:` *sometimes* could lead to more readable feature method description, I found that agreed on one exact label for all the cases saves the time, makes all the tests to look consistently, and also allows to focus on the tests logic at hand. So, choose one and stay with it. We use `given:`.

### Practice #4: Prefer `item | _` over `item << [values]`

For the data tables, another alternative to provide the values is to use `item << [values]` syntax. Also even if it is only an one element in the data table, we still prefer using data tables pipes instead of `<<`. As again consistency is more important, and we found it just more readable and helps the reader to not focus on the new syntax construct, but the data instead.

The only exception probably, where the data values are calculated dynamically, like using all possible enum values, for example.

### Practice #5: Do not mock classes

It is not strictly related to the Spock framework, but in general to any testing frameworks, or testing strategies. Yes, Spock allows you to mock classes by having `cglib` in the classpath, but better to not do so.

There are couple of reasons for this:

- first of all (it is very personal although, as I am in more classical TDD approach of not using mocks) it will force to use mock less often
- second, it might give you a sign that the current design might be wrong or classes unnecessarily coupled together or similar
- third you might consider of introducing an interface instead (although do it only, if you consider your design right, see previous point, as introducing new system element to *just* satisfy the test, is not the right approach. Try to rethink the design instead)

***

Are you using Spock in your organization? If so, do you have other best practices not listed here? What are they?


All the best!
