{:title "Understanding the problem"
:layout :post
:tags ["architectural thinking" "distributed systems"]
}

Understand your problem first in order to choose the right tool. 
So whenever you need to choose a new tool for the product or just study it, it is better to ask yourself the following base questions:

1. what are the use cases this tool is better suitable for or another way  - which problems this tool promises to solve
2. what are the limitations/constraints of the tool (memory, cpu, data model, flexibility, etc.)
3. what are the trade-offs made and why in order to get the benefits/advantages the tool provides (as [there is no a free lunch](https://en.wikipedia.org/wiki/There_ain%27t_no_such_thing_as_a_free_lunch))
4. do you need all of the features of the tool or only a smaller subset of it, i.e. are you ready to pay extra for the features you are not using
5. how the tool is different or similar to other tools from the same category (to link together all the pieces of the knowledge you have, to help your brain to build stronger connections)

Point #3 might look like point #2, but it is more about how it works or why it was done that way, as point #2 is more about "what are they".

A good analogy to this can be found in algorithmic problems: if you know the constraint of your problem (size of the expected data input, restrictions on the upper value bound, memory limits, time constraints) you might want to go with O( N^2 ) solution instead of O(N log N), like using simple sorting, instead of quicksort or heapsort on the array of 10 elements, as you will get the same result (might be even faster), but with more simple, reliable and maintainable implementation, and simplicity matters a lot.

Also starting with the simple solution which solves the problem as well, and switch to more complex one when the current solution is no longer enough - you will be equipped with even more better understanding of your system/problem, so you could make a much better choice. As the development and architecture is all about the trade-offs, and the "price" you are ready to pay.

Everything written above could be applicable to any tool or decision you make, but it is even more the case nowadays with the distributed systems. All of them help you to solve one problem, but introduce a few others (like eventual consistency, lack of transactions, poor data model, more complex setup and management, synchronization, conflicts), which you might not have thought or considered when making the choice. 

So, asking those questions, will help to make the good choice (not the perfect one), as there is no perfect, there is always the price to pay, the question is how more value it brings comparing to the cost (usually not money, but resources and complexity) it introduces.

