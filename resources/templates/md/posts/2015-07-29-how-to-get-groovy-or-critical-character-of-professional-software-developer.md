{:title "How to get Groovy or critical character of professional software developer"
:layout :post
:tags ["software craftsmanship"]
}

The title of this post might sound a little bit confusing, let me explain.

Lets start from the second part of it "Critical character of professional software developer".

> Professional software developer should be able to find the answer *himself* for any problem or task in hand he is dealing with. 

By "finding the answer" I don't mean go to Stack Overflow or Google (which eventually or occasionally necessary to do), but instead be able to pause for a minute, and think about the problem. Analyze the context, and apply analytical and research skills trying to solve it yourself first. That way you are building the habit of thinking and generating solutions (and maybe even looking into the problem from the different angle or rephrasing the problem, so you might come with a better and elegant solution, and always cleaner and simple design of the system in overall) versus being a consumer by just Google-ing and reading Stack Overflow (Stack Overflow might be after all the blessing and the curse at the same time of the software development world).

Although, after all the attempts were made to solve the problem yourself with no success so far, Google for it - is the acceptable way to go (after all the problem must be solved). But doing the search after you put some efforts 
trying to solve the problem yourself first brings a few benefits:
- you are better aware of the problem
- and already tried a few options, so you can ask a better question
- and can more easily understand the solution found
- and more importantly, you build a new knowledge, as if you just get an information from internet, without being personally involved in the problem, you might immediately forget the solution, but being involved hand-first experience with the problem - chances are that finding the solution you will improve your expertise a little bit on that particular area (having an [aha! moment](https://en.wikipedia.org/wiki/Eureka_effect))

Also, it is important to understand the found solution for you problem. If you don't understand the solution completely, how you can be sure that it does what you need. Go to API and explore deeper the parts which are unclear to you. Don't just copy and paste, and trust it. 

Personally, if some part of the posted solution (on Stack Overflow, for example) is not clear to me, or analyzed the context and read through API I don't see a reason to use it fully (it is not necessary, that the person who posted a solution thought about it throughly), I just use the bare minimum of it (don't join a [Cargo cult](https://en.wikipedia.org/wiki/Cargo_cult)). And go for [Fail fast](https://en.wikipedia.org/wiki/Fail-fast) approach. It is better to fail fast, but get the valuable feedback immediately, and that way understand the reason for the excluded part of the solution actually being required, or opposite, to prove that those extra parts were redundant due to not enough thoughts/false assumptions of the person who posted the answer. After all you have to make the solution yours, by analyzing and understanding it, even if found somewhere else.

And the last point, every time you find the solution, try to understand and find the missing path to the knowledge which has helped somebody else to provide an answer you were looking for. You should immediately ask the following questions about the source of the information was used by somebody else to give you an answer:
- is this information presented in the official documentation to the library/language/framework you are using? And why you were not able to find it? If yes, it is better to spend time, and find that fact mentioned yourself
- if it is not part of the official documentation, is reading source code the way to go to get the answer?

After all if someone knows the answer, and you don't, he found it somewhere, so you should always do a "home work" - trying to find the original source of the information, so in the future next time you get a similar problem you should go and reference the original source of truth (which you should be aware right now), and trying again finding solution yourself, relying on your thinking and problem solving skills.

To summarize:
- try to solve the problem yourself first
- think critical for any acceptable answer you find out there
- explore deeper the unknown parts
- find the original source of truth to reference back every time you need to solve a similar problem or in similar area

So, what is about Groovy part, you might ask ;)? Well. I found that reading and learning Groovy is a nice example of the above mentioned principles. You might see many snippets of the code in the internet trying to solve particular problems, but in order to do it efficiently yourself and understand the language, and be able to solve new problems, it is much better just read [Groovy API](http://groovy-lang.org/api.html) and more importantly [Groovy JDK API](http://groovy-lang.org/gdk.html), which enhances JDK classes with helper methods and new behaviour, so you will be able by just reading the original source of truth to solve almost any, if not all, tasks you have in hands (and do not depend on Stack Overflow, or Google that much).

And if you actually wanted to get/install Groovy instead, the best way to do it is to use [GVM](http://gvmtool.net/):

    gvm install groovy
