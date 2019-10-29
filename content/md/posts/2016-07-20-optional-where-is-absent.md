{:title "Where is Optional.isEmpty()"
:layout :post
:tags ["practice" "craft"]
}

When `Optional` was introduced in Java 8, one of the noticeable design decision was not to include any kind of `isEmpty()` method into `Optional` API. Comparing to Scala which tried to take advantage of Java 8 not yet being released, had `Option` class with both `isDefined()` and `isEmpty()` methods.

First it might seem like a flaw in JDK API design. But, lets try to step out and think.

### So obvious?

If it was the first thing we have noticed, and thought about. And it seems like so obvious to have it, don't you think it was not that obvious for the Java 8 API designers? I don't think. I believe they made a concious choice to not add that kind of method into `Optional`, and for a good reason (depends on what you consider good, of course).

### So, what is the point?

I have not Google-d for the official explanation, I completely sure, it can be found either on Stack Overflow or any group discussion. This is not the point. The point is what I think could be their reasoning behind of this decision. And I think they are:

- to promote functional paradigm and thinking
- write better and cleaner code

Lets go through these points as how I understand them.

#### Promote functional paradigm and thinking

If you look into [`Optional` SDK API](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) you can see that it has a few interesting methods: `filter`, `flatMap`, `map`, `orElse`, `orElseGet` and `orElseThrow`. They all take into account the presence or absence of the value. The most interesting in the context of our discussion are the last 3: `orElse`, `orElseGet` and `orElseThrow`, because usually what you want to do when the value is missing? Most of the time either return (or produce aka supply) the default value, or throw some kind of exception. And this exactly what these methods are for, so by not having explicit `isEmpty()` method we are encouraged to look over the API and find the more elegant way of doing same thing.

Lets do some examples:

```
public boolean isSubscribe() {
    return getAction().map(Action::isSubscribe).orElse(Boolean.FALSE);
}
```

or how about this one:

```
record.orElseThrow(() -> new RecordNotFoundException());
```

So, most of the time you don't need `isEmpty()`, and can express same intention and logic in more concise and objectively nicer way.

#### Write better and cleaner code

I do really care about clean code and simplicity (simplicity on all the levels - starting from classes/methods and ending with overall system design and architecture, there are various techniques to achieve those).

In the context of `Optional` and clean code, one of the practice says that negation in if statements should be avoided. So, every time I want to write `if (!Optional.isPresent())` I stop myself, take a pause, and try to understand whether it is possible to achieve same result, but not using negation? Sometimes the answer is to use the methods mentioned above, but sometimes (this is where it promotes to write better and cleaner code) it requires to evaluate the current design, which usually leads in simpler and cleaner way of doing things.

### A few more words on `Optional` usage

If you compare Java 8 `Optional` with, lets say, Guava `Optional`, you might notice that Java 8 `Optional` is not `Serializable`. And this I know for sure was made for a [reason](http://mail.openjdk.java.net/pipermail/jdk8-dev/2013-September/003274.html). The reason is to promote *optional-return idiom only*. So, it means you should not use `Optional` in any other context apart from being used in the return type of a method. So, not in parameter, not in the class field, but just only in return type (although being an idiom, nothing prevents you to do otherwise, but then it is the question of professionalism).

And this is one more example of a choice which encourages for thinking and better design. As it would be much simpler to pass the obtained `Optional` from method to method, before deciding on what to do with, but, objectively, it would make the program structure less clean and easy to work with.

***

So this is it for now. Stay in touch!
