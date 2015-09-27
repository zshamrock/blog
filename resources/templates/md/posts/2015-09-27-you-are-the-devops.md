{:title "You are the DevOps"
:layout :post
:tags ["project management" "practices"]
}

Amazon applies the motto **“You build it, you run it”**. Which I firmly believe.

It might be more difficult to do it in the past, more costly and less reusable between the products, but it is much more easier nowadays (although still not for free) with the tools like [docker](https://www.docker.com/), provisioning tools like [ansible](http://www.ansible.com/), [puppet](https://puppetlabs.com/) or [chef](https://www.chef.io/chef/), deployment tools like [capistrano](http://capistranorb.com/) or [fabric](http://www.fabfile.org/), lifecycle management tool for physical and virtual servers like [foreman](http://theforeman.org/) (and its plugins) and availability of IaaS, PaaS and SaaS (Amazon AWS, Digital Ocean, Google App Engine, to name a few), together with continuous integration like [Travis](https://travis-ci.org/) and continuous delivery tools. 

*You are as a developer will get a better experience with the whole deployment pipeline and the tools used.*

If talking about AWS it was built originally primarily for the development teams to be able to support and be responsible for the application deployment and managing it at production.

### Be ready to rollback

Fear to release? Not everyone can afford frequent releases, although, you should aim for it, to shorten feedback cycles, both from business stakeholders and customers.

If there are no restrictions on when you can release the software, and you still have a fear to do so, one of the way to overcome this fear is to build the deployment pipeline which will allow you to release fast, *but more importantly the one which allows you to rollback to the previous version even faster*. 

Again there are tools to help you to achieve this like capistrano, or you can build the one yourself, might reuse even the same idea as capistrano does keeping the symlink for the current release to the latest deployed version, and multiple previous releases, so you can rollback flawlessly. 

Although it is worth to mention is that while reverting the source code is painless, reverting database changes, for example, might be more challenging (depends on the type of the database changes were introduced with the latest release, as you might need to migrate the new data to fit the old database structure, if it is even possible). 

*So, every time you release, ask yourself, is there an easy way to rollback, and are you prepared enough to do this, if necessary?*

### Keep track of the incidents

If you eventually hit a bad release, it is wise to keep a track of all the incidents happened, especially if it was not possible to rollback to the previous version, while investigation/fixing the issue. One more tool which might be handy there as well - is [status.io](https://status.io).

### Monitoring the app

Releasing the app, is good, *as it is the only thing which matters after all*.
The app built during the hackaton and running online somewhere, is much better, than finely crafted code, sitting in the developer repository.

But, you have to know, if your app misbehaves in production, ideally even before your users will spot this. And this is important for you in order to know whether you should rollback your release to the previous one.
How it can be achieved?

*Monitoring and getting statistics about how app behaves in production should be done and planned from the early beginning of the project, and not as an after-thought.*

At least the first good starting point might be the [Metrics](https://dropwizard.github.io/metrics/3.1.0/) library.

More advanced tools like [Graphite](http://graphite.wikidot.com/) or [InfluxDB](https://influxdb.com/) are available as well, and ideally should be integrated with your project with the early beginning or at least the application should be designed the way that allows to integrate these tools on the later stage of the development easier.

Additional useful technique might be making the new release of the app available only to smaller percentage of all the users (if you have enough traffic), in case of the major changes, or even incorporate a "dark launch" as Facebook has done during their [chat release](https://code.facebook.com/posts/150168455181595/facebook-chat/):

> The secret for going from zero to seventy million users overnight is to avoid doing it all in one fell swoop. We chose to simulate the impact of many real users hitting many machines by means of a "dark launch" period in which Facebook pages would make connections to the chat servers, query for presence information and simulate message sends without a single UI element drawn on the page. With the "dark launch" bugs fixed, we hope that you enjoy Facebook Chat now that the UI lights have been turned on.

### Staying simple

Werner Vogels, CTO of Amazon, in his [interview](http://queue.acm.org/detail.cfm?id=1142065), pointed that in order to achieve the scalability they aim to, they have to start from the core: isolation by applying SOA and relative simplicity of the each independent service.

> For us service orientation means encapsulating the data with the business logic that operates on the data, with the only access through a published service interface. No direct database access is allowed from outside the service, and there’s no data sharing among the services. [Werner Vogels]

### Staying small

There are only a few companies who value the small size of the team, the most famous and successful one is [37signals](https://37signals.com/) with their [Getting Real - Use a team of three for version 1.0](http://gettingreal.37signals.com/ch03_The_Three_Musketeers.php).

Another  notable example is Tallinn based startup [Codeborne](https://codeborne.com/), where they state:

> We are software craftsmen. We don't hire analysts, testers, project managers nor simply programmers. Our people can create quality software from beginning to the end.

### So, you are the DevOps

Let me finish with 2 more quotes from Amazon's CTO of how the developers being as well the DevOps increased the overall quality of the product and services they offer.

> Being directly interacting with the customer developers could better understand the business problem, and the customer needs, so the requirements could be built better
in close loop with the customer. [Werner Vogels]

> Giving developers operational responsibilities has greatly enhanced the quality of the services, both from a customer and a technology point of view. [Werner Vogels]

So, sure enough, being responsible for all the aspects of the product development, including the DevOps, I firmly believe that it will speed up the development and deliver the better quality product for benefit of all.
