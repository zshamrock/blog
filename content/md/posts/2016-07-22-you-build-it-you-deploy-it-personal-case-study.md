{:title "\"You Build It, You Run It\" a personal case study"
:layout :post
:tags ["practice" "craft"]
}

There are a few things which I advocate and believe in (technically wise). One of them is that the team which has built the application, should be responsible for deploying and managing it solely by its own.
So no need for a separate DevOps team. I have talked about this more in one of my previous blog post [You are the DevOps](posts/2015-09-27-you-are-the-devops.html). 

And I am very upset and disappointed when there is a big gap between those two teams, if there is such a separation in the first place.

Here I want to show you of a personal example of this principle in action.

### "I deployed the app, and REST endpoint didn't work"

Even if I do lots of automation testing, including integration, before releasing the application and deploying it on the server. It still nice to see its working. See how it replies to the request, and the data being returned. This is one of the reason after all we all are in this industry, we like to see things actually happening.

So here is the deal. This week I've deployed our latest development build into the development server. Deployment went fine, as expected, as the application was thoughtfully tested during the build by running set of unit and integration tests from Jenkins. After it was deployed, as a habit, and just for the personal peace of mind, I went through all the new features which we have just built for the release. All of them worked, except one REST API. Which was strange?

Ok, the first thing you do is checking the logs. All right, `NullPointerException`. Not clear yet why, all the data in the query is 100% correct, and it was 100% covered by the tests. Digging deeper.

I see, `Teams` attribute is missing from the data returned by the query. Interesting. It has started to ring a bell. About a week ago, I have changed the projection to `ALL` for the index for one of our DynamoDB tables. And we recreate the tables during integration tests in Docker in automated fashion from scratch. So, this is why it has worked fine and all the tests have passed.

So, I went to AWS console, indeed querying by index didn't return the required attribute, as the old index was of projection type `INCLUDE`. My first thought was that I have to delete and recreate the table from scratch, although didn't want to lose the data (even if it is a development database). But, then I went to the `Indexes` tab of the table in AWS console:

<img src="img/dynamodb-table-indexes.png" alt="DynamoDB Table Indexes" class="img-thumbnail">

And have noticed that there is the ability to both create and delete index, and as when I was working on the repository layer implementation for DynamoDB I was extensively reading and walking through the DynamoDB development guide and architecture, so I knew that this is what I needed.

So, I have deleted the current index, created the new one, but using the projection of type `ALL` this time. Copied the name of the index from the JSON request data file we use during the testing. And ... still didn't work.

All right, looking into the logs one more time, now it says that the specified index doesn't exist, so the query can't be done. Of course, I forgot to prepend the prefix to the index, the one we configure through the environment properties (to separate production and development tables). This time recreating the index with the correct name, everything worked. Nice.

### "So what?"

Good question.

Take into account that everything described above, although it might look like a lot of time doing, actually took no more that 5 minutes - from detecting the problem, diagnosis the cause and fixing it. 

And now imagine the same problem is happening (and even worse, lets say in production) when we would have a separate DevOps team which were responsible for the complete infrastructure setup and the deployment process.

I would immediately foresee the following problems:

- by them being in control of the deployment and infrastructure, I would not necessarily know where to look for for the details. Where the logs file is located? Do I have the access to machine? Should I ask someone to look it for me?

- as they are who deploy the app - the time between the deployment done and me detecting the problem would be bigger, as it is not me who deploying the app and seeing the progress, but somebody else, they might not necessary notify me immediately that deployment is done, and the application is ready. As they might be interrupted or just doing other things, so the actual confirmation that the deployment has been finished and the actual time when it was finished could already be a few minutes

- ok, now I know there is a problem, I have the access to the logs. What is next? Will I have the idea why the attribute `Teams` could not be found in the result set? If I was not the one who changed the index, and designed the schema for the table, it would be difficult to answer this question. So, again extra time to figure out (by chasing DevOps team and sharing the exception with them)

- now we know the problem. How we are going to fix it? Should I ask DevOps to do what? I would not be able to provide them a reasonable suggestions of what to do, and have to rely on their competency to get the problem fixed, which would be out of my control

As a result:

- longer time before the problem gets detected
- much longer time before the problem gets solved
- potential fear and resistance of doing the deployment anytime soon

Obviously, the imaginary situation I have described is probably slightly over exaggerated, but still I think it would not be that much different from the reality.

So, what is the answer to this. The answer is "You Build It, You Run It!". The team who is building the application, should be also responsible for every other aspect of the application: from database schema design, to the infrastructure setup, deployment and monitoring and anything else required for the application to be built and deployed.

### "Yes, but the team should have the expertise to do it"

Absolutely! And there is a simple solution for this - allow them to do it. Everything we learn we learn by doing. Another question is that not everybody in the team might like do it. But this is another question. And me being curious for so many years already about every stage of the product development over time gained the necessary skills from the different areas, nowadays it feels very natural to me, but I imagine not everyone will be comfortable working in a such environment.

### Automation is a key

By being responsible for all the aspects of the application development, especially when the team is small, it would not be feasible to cope with everything required in a timely manner. So, automating all the manual (and so potentially error-prone, as we are all humans after all) processes is not a luxury, but a necessity. Automation is a key to productivity and high results. 

How do you get there? It takes time. It is an investment, and as every investment it will pay off itself later, in the longer run. Which, then it is a problem of business understanding this, as not a waste of time, but investing into the future of the product success. And eventually saving money too.

### Bonus point

By encouraging the team being responsible for all the stages of the application development, including the deployment and infrastructure, I believe it helps to architect and come with a better solution. As, being the one who built it, you know what and more importantly how the application works, and can plan the infrastructure accordingly. And the details of the infrastructure used can influence the architecture or the decisions taken during day to day development, and as a result no surprises when there is a time to share the app with the world.

And in general broader expertise of a team (and I also believe, happier and more productive team). A very competent and reliable one.

***

So, this is it. Would be interesting to know what you think. Do you think there should be two separate teams - the one who builds the app and the one who manages and deploys it in production? How is it in your organization? What are the challenges in this approach you could see?

Happy weekend!
