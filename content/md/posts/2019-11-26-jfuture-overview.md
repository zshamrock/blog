{:title "JFuture 2019 Notes"
:layout :post
:tags ["spring" "spring boot" "conference" "jfuture" "circuit breakers" "reactive" "docker"]
}

About a week ago (on November, 16th, Saturday) this year I attended (although for the first time) [JFuture](https://jfuture.dev/) conference. 

The set of topics was the ideal match of the areas which I am at the moment highly interested in. And which are very relevant for the current state of the industry. Very great job by the program committee!

But if take apart the topics - the venue, food, price are superb! I can compare it with the many other conferences I attended in Russia (JPoint, Jocker, Highload++) and Europe (Devoxx, GOTO, and various smaller local conferences) I would rate JFuture to be the number one right now in terms of organization, topics and the price. Very well done!

Below I summarize only the talks I attended. Some triggered a few thoughts, some a lot. Below are not the retelling of the talks and all slides, but just my thoughts, what triggered my attention, and what my takeaways/action points are. 

Also, the below are only my interpretation and understanding or sometimes extension of the talk by some extra personal research and could be not necessary what the speaker exactly talked about. So all opinions and thoughts are mines.

I don't know exactly how to summarize the conference, this is mainly for myself, the "raw" list notes, action items, tools to explore to come back sometime in the future to coordinate the thought process and choose the roadmap. And scheduled actions, which feel free to skip unless it is also relevant for you.

### 1. Uncovering Project Amber by Mala Gupta

Java is changing. Like we or not, but it does. For the better. 

It is good to understand new language features, but until the next LTS release, which is Java 17, which is planned to be released somewhere fall 2021, realistically everything above Java 11 unlikely will be used by major production systems until Java 17.

Other benefits apart from the new language features are the performance and startup time improvement which has been more covered in details by David Delabassee in his JVMs in Containers: Best Practices talk below.

### 2. Battle of the Circuit Breakers: Istio vs. Hystrix/Resilience4J by Nicolas Frankel

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://www.slideshare.net/nfrankel/jfuture-battle-of-the-circuit-breakers">JFuture - Battle of the circuit breakers</a>

[Istio](https://istio.io/) provides global unified fallback implementation without possibility (at least as it has been said) to inject or intercept any business logic behind, so more like "black box" implementation. 

While circuit breakers library is the "white box" implementation instead, where you are providing case by case business-specific fallback option.

The fallback options it is the business decision. Circuit Breakers libraries are just handy ways to implement graceful degradation and fallback options.

Fallback should be fast. There is the temptation to implement another service (with lets say outdated data) as the fallback service, although if it also requires the network call, it might not be the ideal form.

It has been suggested as the option to use or consider to use Spring abstraction over circuit breaker, looks like that one [Spring Cloud Circuit Breaker](https://spring.io/projects/spring-cloud-circuitbreaker). It supports [Netflix Hystrix](https://github.com/Netflix/Hystrix) and [Resilience4J](https://github.com/resilience4j/resilience4j) as the possible implementations, although no [Failsafe](https://github.com/jhalterman/failsafe), for example. But personally, I would use the library directly rather than using Spring abstraction.

Also, another point discussed (asked from the audience) that it might be crucial to know that some particular service is down and the data is serving from the fallback, so the monitoring system should clearly reflect that.

### 3. Efficient web apps with Spring Boot 2 by Stéphane Nicoll

For the new project, it is recommended to start using [WebClient](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-client) instead of [RestTemplate](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#webmvc-resttemplate).

Also when you go reactive go it all way down, i.e. reactive end to end.

The obvious or immediate bottleneck so far could be access to the data storage in a reactive manner, although many providers starting to provide the reactive implementation of its data access. And also under [R2DBC](https://r2dbc.io/) initiative major DB providers work on the reactive programming API to SQL database.

#### Tools: 

Which attracted my attention and worth to explore more.

- [Micrometer](https://micrometer.io/) _vendor-neutral application metrics facade (think SLF4J, but for metrics)_
- [OpenTracing](https://opentracing.io/) _vendor-neutral APIs and instrumentation for distributed tracing_
- [Jaeger](https://www.jaegertracing.io/) _open source, end-to-end distributed tracing (in conjunction with OpenTracing)_

#### Actions:

- Utilize more of the Spring Actuator (including metrics support)

### 4. JVMs in Containers: Best Practices by David Delabassee

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://www.slideshare.net/delabassee/jvms-in-containers">JVMs in Containers</a>

Use the latest JVM, apart from the issues resolved and new features, another important aspect is the startup time, and less memory and CPU consumption. Also, latest JVM honors the Docker environment and correctly reports to the underlying Docker container resource constraints the Java application is running in (like available memory and CPUs [JDK-8146115](https://bugs.openjdk.java.net/browse/JDK-8146115) (since Java 10)).

There are various optimization techniques to reduce the resulting Docker image size containing the Java application:

- Use [jlink](https://docs.oracle.com/javase/9/tools/jlink.htm) to include only the necessary packages (which allowed to reduce the size of the "Hello World" app from 316mb to 34mb) `--add-modules $(jdeps --print module deps ...) --no-header-files --no-man-pages --strip-java-debug-attributes --compress=2`
- Use smaller/compact baseline OS image, like [Alpine](https://hub.docker.com/_/alpine/) (when the [Project Portola](https://openjdk.java.net/projects/portola/) is completed)
- If using [Debian](https://hub.docker.com/_/debian) as the base image always remove native debug symbol, it reduces the size of the resulting image a lot `--strip-native-debug-symbols` <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span>
- Using tools like [Jib](https://github.com/GoogleContainerTools/jib)

In the context of startup time the following optimization techniques could be used:

- Utilize [CDS/Class Data Sharing](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/class-data-sharing.html) (which has been enabled since Java 1.5, but it improves in terms of the availability, features and ease of use every Java release, and is enabled by default from Java 12+). Good post on that subject: [Improve Launch Times On Java 13 With Application Class-Data Sharing](https://blog.codefx.org/java/application-class-data-sharing/)
- Use [GraalVM](https://www.graalvm.org/) and build the native binary for the platform

Also, the overall recommendation is to use the latest possible Java version, as it improves the overall memory footprint and startup time: [OpenJDK Startup - Late 2019 Edition](https://cl4es.github.io/2019/11/20/OpenJDK-Startup-Update.html). As well it has been said that JVM tries to tune itself depends on the system it is running, and it does that better and better every release, which means less of manual JVM optimizations would be necessary.

<img src="/img/hello14.png" alt="OpenJDK Startup - Late 2019 Edition" class="img-thumbnail">

_(credit of the image [OpenJDK Startup - Late 2019 Edition](https://cl4es.github.io/2019/11/20/OpenJDK-Startup-Update.html)_

For the long-running application it might make more sense to use the [HotSpot](https://www.oracle.com/technetwork/java/javase/tech/index-jsp-136373.html) option, where you run the application once, and it is running for days/weeks/months/years.

Another aspect of choosing the base image is the security, and whether (and how many) [vulnerabilities](https://snyk.io/blog/top-ten-most-popular-docker-images-each-contain-at-least-30-vulnerabilities/) it has.

#### Tools:

- [Helidon](https://helidon.io/#/) _is a collection of Java libraries for writing microservices that run on a fast web core powered by Netty (Oracle initiative)_
- [Quarkus](https://quarkus.io/) _a Kubernetes Native Java stack tailored for OpenJDK HotSpot and GraalVM, crafted from the best of breed Java libraries and standards_
- [Micronaut](https://micronaut.io/) _a modern, JVM-based, full-stack framework for building modular, easily testable microservice and serverless applications_
- [OpenFaaS](https://github.com/openfaas/faas) _Serverless Functions Made Simple_
- [Fn Project](https://fnproject.io/) _Open Source. Container-native. Serverless platform_
- [dive](https://github.com/wagoodman/dive) _awesome tool for exploring a docker image, layer contents, and discovering ways to shrink the size of your Docker/OCI image_
- [musl libc](https://www.musl-libc.org/) _a new standard library to power a new generation of Linux-based devices_
- [Portola Project](https://openjdk.java.net/projects/portola/) _to provide a port of the JDK to the Alpine Linux distribution, and in particular the musl C library_

#### Actions:

- Explore more of the [GraalVM](https://www.graalvm.org/) (in particular latest [GraalVM 19.3 release with JDK 11 Support](https://medium.com/graalvm/graalvm-19-3-0-dfdb6f4ec8ed), announced a few days after the conference) and in the context of native [Dynoman](https://github.com/zshamrock/dynoman) GraalVM based distribution


### 5. The State of Reactive Streams by Oleh Dokuka

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://www.icloud.com/keynote/0mDKJN0zavg7KZzCBYgBKL4PQ#The_State_Of_Reactive_Streams_En">The State Of Reactive Streams En</a>

The talk by itself might require a separate post, so simply follow the slides.

#### Actions:

- [RSocket](http://rsocket.io/) has been a while on my radar, might be good to explore it more
- [Reactive Streams](http://www.reactive-streams.org/) and [Reactive Streams JVM](https://github.com/reactive-streams/reactive-streams-jvm) simply RTFM
- Explore [Java 9 Flow API](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Flow.html). That course on LinkedIn Learning could be the good starting point: [Reactive Java 9](https://www.linkedin.com/learning/reactive-java-9) 
- Explore more of the [Kotlin Coroutines](https://kotlinlang.org/docs/reference/coroutines-overview.html) and read the guide (I expect it is somehow similar to Go goroutines)
- Explore [Project Loom](https://wiki.openjdk.java.net/display/loom/Main) specification/status and available online talks and resources. Good to understand on how it resembles the available these days coroutines in Kotlin.
- Explore [Spring web reactive support](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#spring-webflux). Oleh's book could the good option: [Hands-On Reactive Programming in Spring 5](https://www.packtpub.com/application-development/hands-reactive-programming-spring-5) (as his understanding of the subject is deep and trusted).
- Understand how [RxJava](https://github.com/ReactiveX/RxJava) fits now into the world of Java Flow API, Spring Reactive and in general how do they interconnect to each other. Maybe this book could be helpful as it covers RxKotlin implementation and highlights what is the difference or similarity between internal Rx implementation and Kotlin Coroutines: [Reactive Programming in Kotlin](https://www.packtpub.com/application-development/reactive-programming-kotlin) (although it is old from 2017, maybe it is still relevant).
- Use the latest version of [Project Reactor](https://projectreactor.io/) on the project

<div class="alert alert-warning" role="alert">
Have to remind to myself that goal is to understand the subject in order to make the conscious decision, and not based on the "hype", and just use it, because it is "cool and trendy", but rather understand how it works, and what problems it solves, and apply only when there are benefits of doing so.
</div>

### 6. Dealing with Time and Clocks in Code by Kees Jan Koster 

<img src="https://imgs.xkcd.com/comics/iso_8601.png " alt="ISO 8601" class="img-thumbnail">

_(credit of the image [xkcd](https://xkcd.com/1179/)_)

Always set up [NTP](http://www.ntp.org/) on the server. It is highly critical for correctness for many distributed systems. Although looks like it is not that straight forward as in these 2 articles explained some common pitfalls and the solutions to them: [Synchronizing Clocks In a Cassandra Cluster Pt. 1 - The Problem](https://blog.rapid7.com/2014/03/14/synchronizing-clocks-in-a-cassandra-cluster-pt-1-the-problem/) and [Synchronizing Clocks In a Cassandra Cluster Pt. 2 - Solutions](https://blog.rapid7.com/2014/03/17/synchronizing-clocks-in-a-cassandra-cluster-pt-2-solutions/) (although they are 5 years old, so things could be improved/changed over this period of time, but at least how it was in the past, and could be still relevant up today).

Always use UTC. And convert to UTC as soon as possible, and convert back to the user locale-specific format as late as possible (mainly on the UI layer).

Use ISO 8601 format/Zulu to store date time, i.e. `yyyy-MM-ddTHH:mm:ss.SSSZ` (or UTC timestamp).

User's locale is most of the time the only source of data on how to convert UTC date time format back to the user's date time. Although it is not always reliable. So maybe this is why so many applications (web and mobile) require or at least allow the user to explicitly set the time zone the user is in.

If you can avoid working with time zone do this. Time zones are difficult.

Always use the library which deals with the date time conversion, do not reinvent the wheel and write your own.

### 7. Teach your PacMan to play with ML and Reactive Streams by Mary Grygleski/Oleh Dokuka

As I understood the idea of the talk/research/project was to explain that simpler techniques than classical Machine Learning could be used to solve at first look difficult problems (sorry if interpret or call things wrong). One such approach is the Q-Learning/Reinforcement learning.

I found that article that looks like the gentle introduction into the topic [An introduction to Q-Learning: Reinforcement Learning](https://blog.floydhub.com/an-introduction-to-q-learning-reinforcement-learning/) and probably more in-depth paper [Combining Case-Based Reasoning andReinforcement Learning for Unit Navigation inReal-Time Strategy Game AI](https://www.cs.auckland.ac.nz/research/gameai/publications/Wender_Watson_ICCBR14.pdf).

One of the points of interest of mine whether this approach (if it is indeed simple enough) could be used to program AI in the [CodinGame multiplayer contests](https://www.codingame.com/multiplayer).

### 8. Exploring Neural Networking with Kotlin Metaprogramming by Amanda Hinchman

UI testing is hard, and from what I understood there are no good (or any at all) tools available to solve this problem. 

UI automation testing or other intelligent testing approaches/frameworks should not be considered as the replacement of the testers, but rather as the tools to simplify and improve the quality of their job.

I've tried [TornadoFX-Suite](https://github.com/ahinchman1/TornadoFX-Suite) on my own [Dynoman](https://github.com/zshamrock/dynoman) project and on the TornadoFX-Suite itself. It failed in both times unless I used it wrong.

- - -

### Other Talks

Below are available slides of the other talks (for which the slides were published online, but which I didn't attend, so I can't comment on anything).

#### Hey Google, send 20$ to Mummy!? by Julien Lengrand-Lambert

[Hey Google, send 20$ to Mummy!?](https://github.com/jlengrand/google-send-money-talk)

#### IoT powered by Microprofile – Microservices in practice by Mads Opheim

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://speakerdeck.com/madsopheim/iot-powered-by-microprofile-jfuture">IoT Powered by MicroProfile - JFuture</a>

#### Distributed Caching Patterns in Kubernetes by Mesut Celik

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://www.slideshare.net/mesutcelik/distributed-caching-in-kubernetes-with-hazelcast">Distributed Caching in Kubernetes with Hazelcast</a>

#### Purely Dysfunctional Data Structures in Java by Grzegorz Piwowarek

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://pivovarit.github.io/talks/purely-functional-data-structures/#/">Purely Dysfunctional Data Structures</a>

#### Spring Data Moore and beyond by Christoph Strobl

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://speakerdeck.com/christophstrobl/spring-data-moore-and-beyond">Spring Data Moore and Beyond</a>

#### Collaborating on Open Source Software by Marit van Dijk

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://speakerdeck.com/mlvandijk/collaborating-on-open-source-jfuture-dot-dev-minsk-nov-16th-2019">Collaborating on Open Source - JFuture.dev (Minsk) Nov 16th, 2019</a>

#### Live Coding Music 101 by Piotr Jagielski

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://docs.google.com/presentation/d/1otj7JbYx9VPJkPrIO4FY0Pbsv92NVf3izKyNmuPsRA4/">Live coding music 101</a>

- - -

Again the conference was very good in all the aspects. And I would definitely recommend anyone (although check whether you are interested in the particular talks) attend it the next time!


<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>
