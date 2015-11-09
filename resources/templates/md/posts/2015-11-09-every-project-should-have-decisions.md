{:title "Every project should have a decisions making file"
:layout :post
:tags ["project management"]
}

Have you ever wondered when joining a new project, or exploring a public project on GitHub why specific decisions were made,
what was the motivation behind those decisions, if any, what were the alternatives considered? I definitely am. 

Projects might have nice architectural diagrams, API, technical documentation, this is all good, and valuable assets, 
but without knowing the reasoning behind why that exact architecture or even stack was chosen will impact the in-depth of the project understanding, 
quality of the project in the longer term and maybe even the passion around the project the developers have.

Sure, eventually you might reach the point where everything starts making sense, but it would be much better if it was an explicit document, instead of
gossips and rumors which are inevitable if decisions making process is hidden.

Before we start the discussion you might take a look into one of possible [decisions.md](https://github.com/zshamrock/stonesgame.io/blob/master/decisions.md) in action.

### What

What should you document. It is all about **Why, Alternatives, Risks**:

- **Why** that specific decision was made (even if it was just a personal taste, make it clear)

- What were the **alternatives** considered (it is ok to state that no alternatives were considered at all, this is still valid, as it is clear for everybody now that this was a deliberate choice)

- What are the potential **risks** you have foreseen by making this decision (risks could be from any aspects, like cost, extendibility, maintenance, learning curve, literally anything, or none, if you could not foresee any)

It is important to understand that the purpose of this document is not to build the "perfect" architecture and the system from the beginning (as it is impossible, while software development being partially an art, and as every art it requires iterations, and refinements in the process). 

It is about transparency, and if you like, maybe even about respect for everybody who is and will be involved in the project now and in the future.

What exact decisions could be? The rule of thumb - anything you consider important to mention, or would like to read yourself if you would join your own project.
But the minimal core should cover such areas as:

- technology stack (language, frameworks, libraries) (both backend and frontend)
- application architecture (again, not what, but why)
- building tools
- testing framework
- provisioning and deployment
- hosting
- CI/CD
- application/http server
- persistence storage (which database and why)
- caching
- security

If you don't have a specific decision yet made regards some of the aspects mentioned above (and they are relevant to your project), it still makes sense to say it explicitly, that this area is important and will require some revisiting later in the project's stage. Again it is all about transparency.

### How

Keep it simple. Markdown or any human readable/friendly format will do the job. It is good to put a date when you add the entries into the file, like "Sun 1 Nov, 2015".

**Never change anythig, but append only!** Do not try to change the past, if you have to change the decision you made before, just make a new decision and document it, keeping the old there as well, so the progress, and thinking process will stay visible. 

And this imperfection is what makes this document so valuable.

### When

Whenever the new important decision or step was made. When you feel comfortable and have time to do so. But, never postpone it for too long, as there is a danger, that then it will not reflect the exact thinking process you had in the moment of making the decision.

### Ok, if it is so coll, why nobody does it?

Good question ;) I can't answer for sure, I can only guess. I think it is fear:
- fear of sharing the knowledge with people around you, which is a bad sign by itself, as true leaders grow other leaders, and in order to have more power you have to share the power
- fear of being wrong, and the desire to have a "perfect" decisions making history record (again true leaders will admit their mistakes and learn from them, and it is easier to learn if you have all the thinking process documented, and true leaders respects the people they work with, and don't afraid of them)
- fear to be a human, to be yourself, no matter what the title you have, courage to admit, that some decisions were made, just because you feel confident with the technology, or just like it, or feel it is a right thing to do, don't play politics, be a human
- fear of transparency, that you are no longer the source of "secret" data and privileged access
- fear of taking responsibilities of the decisions made

### Give it a try

In the early beginning of the project, with lack of the information, it is difficult to make the right decisions. So, the goal is don't waste the time, and either make the reasonable decisions if possible (to provide a roadmap) or postpone it until more information and more important understanding is revealed.

The goal is not to build the perfect architecture/decisions from the beginning, but **document** the **why** of the important decisions made, so everyone can access the decisions making process, check which assumptions were right, which were changed while the project evolved, which were completely wrong, etc. So, everybody could learn, and it would be transparent to everyone, why the project is in the state it is at the moment.

I think that if just a fraction of enterprise and open source projects would integrate this practice in its software development process, it could be a huge step forward to make an IT industry a better place to work in.

People spent time and efforts to draw the final product architecture, but they miss the part of explaining why.

It is a decisions making history.
