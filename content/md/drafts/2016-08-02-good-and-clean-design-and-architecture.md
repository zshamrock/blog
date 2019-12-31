{
:title "Good clean design and architecture"
:layout :post
:tags ["craft"]
}

*Disclaimer*: this post is very personal, and it just an attempt to express my feelings, and share my own pain of the current state of the industry.

Through all my career, since my first Hello World and till nowadays, I was always looking into understanding the nature of software development and design. How to design a software? What does it mean to be a professional? What does it mean to write a good code? What other people in industry think about writing good code and craftsmanship?

It is a difficult journey. Mostly due to the fact that 80% of the developers I met just simply don't care about all that things. And I have put lots of efforts, personal time, to share the knowledge, teach the skills, and trying to explain that there is a different way of doing things, and different way of thinking. It is not always rewarding, sometimes I just want to give up, and let them do whatever they are doing, sometimes it is even impossible to change the company culture or attitude, so better leaving such companies.

On the bright side - I see more and more people starting to emphasis and accept various techniques to help to design and write the code, but still many of them lacks the simplicity.

Also, I believe anyone can be a good developer and designing a good simple systems, even if they have not read any books. It all comes to the common sense.

I am really tired see the people constantly making the illogical choices, and as a result the code slowly starts to rot. Yes, they managed to implement the task at hand, it even works. But it all in the cost of the future maintenance and extendibility, and other people's efforts and time to understand the code, make sense of it (which most of the time, there is not), and make it simpler.

I struggle to explain my own way of thinking or designing things. As it comes so natural to me, like breathing. You don't think about how to breathe, same, in my case, goes for the software design and the code I write. It is all the results of reading books, blogs, watching videos, attending conferences, learning and practicing dozen of different languages, exploring things outside of my day to day language, frameworks, and libraries. And I am really surprised, when the senior people encounter the new way of doing things, which is already obvious to me for so many years. 

The best way to explain my own way of thinking and designing things I come to is common sense, or things sound logical. It is still difficult to explain, but every time I do write something, and it doesn't feel right (this inner feeling of beauty and good design, which I believe was grown in me but the plethora of resources, and many time spending thinking and analysing things), I know that there is something wrong there, and it could be modeled and done differently. It might require multiple iteration over the implementation cycle, before the things start to make sense, but I always keep this in mind. And I don't understand how other people can't see it. And can happily produce illogical code and design, mixing different concepts together. I do accept, when people say, that they know the current design has flaws, but they could not come to the better approach. Also I believe there is always a better approach, you just have to find it. But I do appreciate the fact that people actually spent time and thought about possible alternatives (even if they were not able to find one), which is better that doing whatever comes into mind first (which can be the right thing, it all depends on the experience and practice of the person), and simply ignore (or they are not brave enough to accept) and deny the fact the design they've implemented is not right.

As in my opinion, anything which doesn't sound logical, and is done to the sake of time or ease of implementation - most of the time is wrong and will bite you in the future. There is always a price to pay. If it is not now, then question is when? But it is unavoidable, unless you make things right. Or at least, doesn't make things so obviously illogical.

Are there some rules of defining what is illogical apart of being a common sense? The one I know for sure - if it makes difficult to test things, or construct the object, it is definitely a sign, that object does much more, and the solution should be architectured differently.

- Difficulty to test

- Assignability to null

- Difficulty to come with a simple concise name

- Not possible to use `final`

- Lots of dependencies to do the task

- Difficult to construct/setup a class

- Lots of unrelated (or from the different levels of abstraction) API methods

- Consistent naming

- "If the implementation is hard to explain, it's a bad idea" Zen of Python

Someone, can always, argue on some techniques - that they are minor, but I believe exactly attention to these kind of minor details - is what distinguish and well designed and well understood system, from badly designed and difficult to understand.

What if there is a time pressure or business demands? I don't know the answer there, probably try to build a good relationship and trust with each other, so could rely on anything you think right is also right for the business. Although don't know the right answer here.

What is solution? How people can be taught? I think it is very personal. Obviously, still everyone can read about clean code, know about practices like TDD or principles like YAGNI, KISS, Single Responsibility principle, Boy Scout rule, 12 factor apps, and many many others. But in its core, I think it all comes to either the person cares or he doesn't. And if the person doesn't care, nothing can persuade him. And if the person cares, he should keep looking and searching.

Software development is like a craft. It is mix of skills and knowledge which can be taught, and style, which is the result of the person's decisions and life philosophy (which can't be taught, but only enlightened). And you can't do nothing here, unless the person decides to change his life's philosophy. "Simplicity is a choice", as wisely told Rich Hickey. And either person makes this choice every day, on every line he writes, on every class or abstraction he creates (or more importantly doesn't create), or the person simply doesn't care. Unfortunately I see the later is happening much more often than former. But, no matter what, I will continue of bringing and sharing my own believes of the simple, beautiful code. I hope majority of the developers will get it one day.


