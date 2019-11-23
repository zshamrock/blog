{:title "JFuture 2019 Notes"
:layout :post
:tags ["spring" "conference"]
}


This year (although I attended it for the first time) the set of topics were the ideal match of the areas which I highly interested. And which are very relevant for the current state of the industry. Very great job by the program committee.
But if take apart the topics - the venue, food, price is the superb! I can compare it with the many other conferences I attended in Russia (JPoint, Jocker) and Europe (Devoxx, and various smaller local conferences) I would rate JFuture to be the number one right now in terms of organization, topics and the price. Very well done!

Below I summarize only the talks I attended. Some triggered a few thoughts, some a lot. Below are not the retelling the talk and all slides, but just my thoughts, what triggered my attention, and what were my takeaways/action points. Also the below are only my intepretation and understanding or sometimes extension of the talk by some extra personal research, and could be not necessary what the speaker talked. So all opinions and thoughts are mine.

I don't know exactly how to summarize the conference, this is mainly for myself, the raw list notes, action items, tools to explore to come back some time in the future to coordinate the thought process and choose the roadmap.

### Uncovering Project Amber by Mala Gupta

Java is changing. Like we or not, but it does. For the better. I already started as part of the work on Dynoman to experiment with Java 11, while there are other interesting features are coming, it is good to understand them, but until the next LTS release, which is Java 17, which is planned to be released somewhere fall 2021, so might be good to be aware of the new changes, but realistically everything above Java 11 unlikely will be used by major production systems until Java 17.

Other benefits apart from the new language features is the performance and startup time improvement which was more covered in details by David Delabassee in his JVMs in Containers: Best Practices talk.

### Battle of the Circuit Breakers: Istio vs Hystrix/Resileince4J

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://www.slideshare.net/nfrankel/jfuture-battle-of-the-circuit-breakers">JFuture - Battle of the circuit breakers</a>

The fallback options it is the business decision. Circuit Breakers libraries are just the handy ways to implement graceful degradation and fallback options.

Fallback should be fast. There is the temptation to implement another service (with lets say outdated data) as the fallback service, although if it also requires the network call, it might not be the ideal form.

There in the discussion has been suggested the option to use or consider to use Spring abstraction over circuit breaker, looks like that one [Spring Cloud Circuit Breaker](https://spring.io/projects/spring-cloud-circuitbreaker). It support Netflix Hystrix and Resillience4J as the optional implementations, although no [Failsafe[(https://github.com/jhalterman/failsafe), for example. But personally I would use the library directly rather than using Spring abstraction.

Also another point discussed (asked from the audience) that it might be crucial to know that some particular service is down and the data is serving from the fallback, so the monitoring system should clearly reflect that.

### Efficient web apps with Spring Boot 2

For the new project it is recommended to start using [WebClient]() instead of [RestTemplate]() and go reactive all way down, i.e. reactive end to end.

When go reactive the visible benefit of using reactive approach, when all the layers (or as many as possible) will be implemented in the reactive manner. The obvious or immediate bottleneneck so far could be the access to the data storage, although many providers starting to provide reactive implementation of its data access. And also under [R2DBC](https://r2dbc.io/) initiative major DB providers work on the reactive programming API to SQL database.

Tools which attracted my attention and worth to explore more:

- [Micrometer](https://micrometer.io/)
- [OpenTracing](https://opentracing.io/)
- [Jaeger](https://www.jaegertracing.io/) in conjunction with OpenTracing

Actions:

- Upgrade to the latest Project Reactor
- Utilize more of the Spring Actuator (including metrics support)


### JVMs in Containers

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://www.slideshare.net/delabassee/jvms-in-containers">JVMs in Containers</a>

#### Tools

- [Helidon](https://helidon.io/#/)
- [Quarkus](https://quarkus.io/)
- [Micronaut](https://micronaut.io/)

Actions:

- Explore more of the [GraalVM](https://www.graalvm.org/) (in particular latest [GraalVM 19.3 release with JDK 11 Support](https://medium.com/graalvm/graalvm-19-3-0-dfdb6f4ec8ed), announced a few days after the conference) and in the context of native Dynoman GraalVM based distribution

Use latest JVM, apart from the issues resolved and new features, another important aspect is the startup time, and less memory and CPU consumption. Also latest JVM honors the Docker environment, and correctly reports to the underlying Docker container resources constraints the Java application is running in (like available memory, and so available heap size, and CPUs available, i.e. Runtime.getCpu() call).

### The State of Reactive Streams

<a class="embedly-card" data-card-controls="0" data-card-theme="dark" href="https://www.icloud.com/keynote/0mDKJN0zavg7KZzCBYgBKL4PQ#The_State_Of_Reactive_Streams_En">The State Of Reactive Streams En</a>

The talk by itself might require the separate post, so simply follow the slides.

Actions:

- [RSocket](http://rsocket.io/) has been a while on my radar, might be good to explore it more
- [Reactive Streams](http://www.reactive-streams.org/) and [Reactive Streams JVM](https://github.com/reactive-streams/reactive-streams-jvm) simply RTFM.

<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>
