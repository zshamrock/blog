{:title "Fears of Clojure"
:layout :post
:tags ["craft" "clojure"]
}

Today's post was triggered by the email from [https://clojuregazette.com/](https://clojuregazette.com/) titled "Where does Clojure shine?". I have nothing to disagree with Eric, although the following point *"Clojure is all about simplicity. Real-world problems are inherently complex, so you don't want to add to that complexity with a complicated programming language. Your language should let you untangle the concepts and put them together in an orderly way."* (credit to Eric Normand) made me feel sad. It is not that because I disagree, no I completely agree, and simplicity is one of my believe and value as a software engineer. What made me sad, is that apart from my interest in Clojure, and that it looks like it was built with simplicity in mind, I still tend put Clojure aside.

I've started to think why? I like Clojure, I am very interested in the language. But still don't use on a daily basis.

So, the answer I got is fear (or fears).

Don't get me wrong. I don't fear of functional programming. Personally, I found it very natural, and was doing it (in a degree in which it was possible) in JavaScript - either native and using lodash library. I was writing Scala for a while (in production), was doing Clojure (mostly personal projects), and studied Clojure in general - conferences, books, articles, meetups, as well as writing.

I don't fear of testing or TDD, as I practice them already.

I don't fear of core.async, as I studied the concurrency and Go lang.

I don't fear of vim or Emacs to work with Clojure (actually [https://github.com/zshamrock/startup-prepublic](https://github.com/zshamrock/startup-prepublic) was completely written in vim). Although nowadays I would try Cursive.

I don't fear of touching new languages, as I already tried so many of them.

I think I've already passed the learning curve after which I don't fear Clojure anymore (although still far from being an expert), I know how to build the program, how to use libraries, how to test, where to get help, and documentation, building my own workflow. So, in a nutshell I can be productive with Clojure.

But, apparently, there are still things which stop me committing fully into the language.

### Fear #1: No types

As working as Java developer for all my career, and I still enjoy working with Java on a daily basis, although my approach to design the code (focusing on simplicity, elegance, and testing) might not be the typical in the industry, and this is why I still like Java, because this is not problem of a tool, but how you use it.

So, I really appreciate having types. Although I am using quite successfully Groovy for testing. I used JavaScript a lot in the past. And more or less Clojure from time to time.

What I afraid is that lack of types make the program less readable in the longer term. Obviously, Clojure has records. And might use them and keywords a lot.

But, probably good names, small methods, and tests, plus REPL, might overcome lack of types, or to user alternative 

### Fear #2: Clojure is for small team

Probably up to 2-3 developers. I was involved in the existing projects where it was very difficult to understand the code with the team not being much bigger. But still types helped there. Maybe in case of Clojure it won't that big issues.

### Fear #3: Not able to understand your code later

I looked into 
(https://github.com/zshamrock/startup-prepublic)[https://github.com/zshamrock/startup-prepublic], obviously it is not the biggest one, and I didn't use any exotic features, still I was able to understand it due to good naming (not only mine, but from Clojure API and libraries), and smaller functions. Also REPL is there, which will help to explore if something is not clear.

### Fear #4: Refactoring is no longer possible

### Fear #5: Not enough big market

It is relatively easy to find Java job, so far I have not seen a lot traction outside of the big names. Yes, if you are planning the rellocation, it actually might be the advantage, as you have the competetive advantage. Also I found knowing Clojure makes you a better software engineer no matter which language you use on a daily basis.

### Fear #6: Vulnerability
Feeling vulnerable if switching from the area where the person have expertise to the unknown. Although it is not very much the case for Clojure for Java developers, as they stay on JVM, and any skills or expertise got before can be applicable in smaller or bigger degree in Clojure too. 

And in general, we live in so fast changing world, that walking outside of the comfort zone from time to time in professional are is probably even advised to do so.

### So, fake fears?

Those fears are, of course, not only specific to Clojure, but to any dynamic, or any new language even, and there are successful big systems written lets say in Python, or JavaScript, so it proves, it can work. Although it might require discipline and style to do so.

Back to the Eric's quote from the beginning. I think it is absolutely possible to write complex code in any languages available nowadays. It looks like, due to various reasons, in some languages it is easier than in others. Opposite is true too, in some languages writing simple code becomes easy or easier, and complex code more difficult. But also I believe that no matter which language you use, it is possible to write simple code in any language as well. So, this might be why I still using Java on a daily basis, because I know how to write simple code in Java. And I agree, that in general Java community (I think it becomes better right now, but legacy and "broken" mind-set is huge), it was favored to write complex code, that you almost become a hero if manage to write the code nobody else could understand.

So, it boils to the personal principles, techniques, philosophy and values, which has nothing, ok, almost, nothing to do with the language. The critical component of the language is then becomes the community. And this is where the difference happens. Community either have simplicity as a core value of it, or doesn't. And be honest, Clojure, in my opinion is the winner here.

So, it looks all my fears are not really fears. Probably then it is just a matter of decision - to jump into unknown for a long enough, and see how it goes. Which probably I will do eventually.

