
The hottest topics were Struts 1.0 (even not 2.0), and AJAX. Hibernate, Spring, SPA, JavaScript frameworks either have just started to appear, or even there were not such things yet.

UI rendering was done using JSP and JSTL, even scriptless. Maybe nowadays Java developers, even don't know what it is.

Occasionally some calls to the backend was done using AJAX (even without jQuery, but pure browser XMLHTTPRequest, i.e. manually).

It uses svn (back then it used cvs), even not Git, there are no branches, as it is. 

Communication with the database is done using pure JDBC, ResultSet, and PreparedStatements, again, maybe nowadays some of Java developers, who immediately jump into Hibernate, even don't know about such things.

No tests, no static code analysis tools, nothing. It has still survived and still delivering a value every day.

Back then the way how application was deployed - everything was manual. You have to build a WAR, stop Tomcat, copy a WAR, restart the container. It was not even the Tomcat, it was Resin Caucho, it was migrated to the Tomcat later 
in the stage (very later). Sometimes we have even to edit JSP manually outside of the SCM, and restart the web server.

Almost no web frameworks, even Struts 1 was introduced and used in about 80% of code right now, we have to write Servlets and Filters ourselfves, implementing doPost(), doGet(), and filter() correspondigly, ourselves,
manually working with raw HTTPRequest and HTTPResponse.

No maven, or any build tool, no dependencies management, everything was inside tomcat/lib, later in the development the only improvement was done, that libs were moved into SCM as well, storing right at svn.

Deployment nowadays due to efforts of the one of the developer from my days, TeamCity was introduced, the build takes about 1 minute 40 seconds in average. 1 minute 40 seconds! How long does it take for your system to build?

Deployment takes about 10 seconds, it still does what it did before (building a WAR, copying a WAR, restart the Tomcat), but just automated over TeamCity. 10 seconds! No plans, or release cycles, just hit the button when you are ready. And it still works.

You have to understand who are the developers of the system. They are students, who back then, even worse now, come without any knowledge at all (and as well, still "virgin", and not испорчены by the (over-)complexity mindset working in enterprise company). Actually back then it was not possible to get this "overcomplicate everything" mindset, as development at main software companies didn't look that much better. Yes, they might have tests, used more advanced features (started to overcomplicate) of Struts 1.0, and many others, but in general, it still felt like a simple thing, even they had paid customers and users.

So, old developers (graduated students) leave, leaving usually no documentation at all, no tests. But it still was possible for completely new to IT students to take over, understand the system, and continue it growths.
When I talk about new or old developers, I am not talking about team of 100 people, usually it average over all the time size (although they were mostly worked individually, while sharing successes with each other, and really friendly, like a family, relationship with each other) from 2 to max 8 people. And they still manage to deliver and be productive.

And, yes it is still running in Java 6, no lambdas, no new languages features or improvements, you might even seen non-generic collections (yes there were such things :), and used a lot. And it was ok.

And yes, it was done for free, no money paid, just people were enthusisted, yes, money would be handy, but people did it, because they liked it, and they wanted to learn, get some experience.

No meetings done at all. The whole discussion were happened just in small (I mean really small) university lab, it was a cool atmosphere. No managers, no supervisors, the unversity professor who was responsible for the laboratory, he is an outstanding teacher, and but that I mean, that usually he didn't know a lot about Java development, but he was able to define a clear goal of what must be done, and asked the right questions to the students, and talk the right things to motivate people, and let grow, pay attentiion to this, personally, not professionaly. I mean becoming a better person, with better values and attitude, working hard you will eventually reach the professionalism yourself. 

Almost no frameworks were existed, no scallable langaguages available, which promised to solve all the problems :), just pure old Java (do you still remember Java 5?).

No DevOps, no QAs, no "architects". Just developers.

From the technical standpoint, supervisors or teachers where older students. And it is unique as well. How often do you have collegues, who are willing to spend time, teach you, share some knowledge with you? How often you do it yourself?

How often in a big company you have a chance to learn every day.

Compare to nowadays, you have plenty of choices, all that cool features, and languages, hundreds of frameworks, libraries, Github, JavaScript domination, and still most of projects I joined on the later stage were suffered from over complexity, and usually not due to business complexity, but most of the time complexity introduced by the tools used (inherited?), and over engineering things, building at Facebook scale, while having only a fraction of users comparing to Facebook. Build takes minutes!, deployment takes days or weeks, automation tests takes days, sometimes not producing that much value comparing to the efforts spent. And still some project fail to achive the goals, and run over budget.

So, what is the takeaway?

Small team, simple tools, almost to its bare minimum, simple design and concepts, highly involved in the development, and feeling the responsibility and impact of everything you do, no managers or scrum masters, or anything else, just a well defined task (could be done, by the owner of the company as well), and let the developers do the rest. No devops team, no QA, just developers. Clean simple code, simple in reading and understanding, not much explictis things done via bunch of XML configuration files, or @Annotations. Yes, it took more code to write, but you don't need to master yet another framework to understand the code, just follow the execution.
