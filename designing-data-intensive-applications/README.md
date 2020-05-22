# Chapter 1 - Reliable, Scalable, and Maintainable Applications
A data-intensive application is typically built from **standard building blocks** that provides commonly needed functionality:
- *Databases*: store data so that they, or another application, can find it again later.
- *Caches*: remember the result of an expensive operation, to speed up reads.
- *Search indexes*: allow users to search data by keyword or filter it in various ways.
- *Stream processing*: send a message to another process, to be handled asynchronously.
- *Batch processing*: periodically crunch a large amount of accumulated data.

When building an application, we still need to figure out which tools and which approaches are the most appropriate for the task at hand. 

## Thinking About Data Systems
We typically think of databases, queues, caches, etc. as being very different categories of tools. Although a database and a message queue have some superficial similarity - both store data for some time - they have very different access patterns, which means different performance characteristics, and thus very different implementations.

Many applications have such demanding or wide-ranging requirements that a single tool can no longer meet all of its data processing and storage needs. The work is **broken down** into tasks that can be performed efficiently on a single tool, and those different tools are stitched together using application code. 

Example of an application managed caching layer with full-text search server:
- application code's responsibility keeps those caches and indexes in sync with the database
- combine several tools in order to provide a service, the service's interface or API hides those implementation details from clients. 
![One possible architecture for a data system that combines several components][figure1-1]

Tricky questions arise:
- How do you ensure that the data remains correct and complete, even when things go wrong internally?
- How do you provide consistently good performance to clients, even parts of your system are degraded?
- How do you scale to handle an increase in load?
- What does a good API for the service look like?

Three concerns that are important in most software systems:
- *Reliability*: the system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or software faults, and even human error).
- *Scalability*: as the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.
- *Maintainability*: overtime, many different people will work on the system (engineering and operations), and they should all be able to work on it productively. 

## Reliability
Typical expectations include:
- The application performs the function that the user expected.
- It can tolerate the user making mistakes or using the software in unexpected ways.
- Its performance is good enough for the required use case, under the expected load and data volume.
- The system prevents any unauthorized access and abuse.

If all those things together mean "working correctly", then we can understand *reliability* as meaning, roughly "continuing to work correctly, even when things go wrong."

The things that can go wrong are called *faults*, and systems that anticipate faults and can cope with them are called *fault-tolerant* or *resilient*. The former term is slightly misleading: a system cannot be tolerant of every possible kind of fault, but to *certain types* of faults.

Note that a fault is not the same as failure. A fault is usually defined as one component of the system deviating from its spec, whereas a *failure* is when the system as a whole stops providing the required service to the user. It is impossible to reduce the probability of a fault to zero; therefore it is usually best to design fault-tolerance mechanisms that prevent faults from causing failures.

### Hardware Faults
Hardware faults: hard disks crash, RAM becomes faulty, power grid blackout, someone unplugs the wrong network cable, etc. Those things happen all the time when you have a lot of machines.

First response is usually to add redundancy in order to reduce the failure rate of the system, e.g. RAID configuration or hot-swappable CPUs. Redundancy of hardware components was sufficient for most applications, since it makes total failures of a single machine fairly rare. As long as you can restore a backup onto a new machine fairly quickly, the downtime in case of failure is not catastrophic in most applications.
In cloud platforms such as AWS, it is fairly common for virtual machine instances to become unavailable without warning, as the platforms are designed to prioritize flexibility and elasticity over single-machine reliability.
Hence, there is a move toward systems that can tolerate the loss of entire machines, by using software fault-tolerence technique in preference or in addition to hardware redundancy. Such systems also have operational advantages: a single-server system requires planned downtime if you need to reboot the machine, whereas a system that can tolerate machine failure can be patched one note at a time, without downtime of the entire system.

### Software Errors
Another class of fault is systematic error within the system. Such faults are harder to anticipate, and because they are correlated across nodes, they tend to cause many more system failures than uncorrelated hardware faults. Example includes:
- A software bug that causes every instance of an application server to crash when given a particular bad input.
- A runaway process that uses up some shared resource - CPU time, memory, disk space, or network bandwidth.
- A service that the system depends on that slows down, becomes unresponsive, or starts returning corrupted responses.
- Cascading failures, where a small fault in one component triggers a fault in another component, which in turn triggers further faults.
There is no quick solution to the problem of systematic faults in software. Lots of small things can help: carefully thinking about assumptions and interactions in the system; thorough testings; process isolation; allowing processes to crash and restart; measuring, monitoring, and analysing system behavior in production.

### Human Errors
How do we make our systems reliable, in spite of unreliable humans?
- Design system in a way that minimizes opportunities for error.
- Decouple the places where people make the most mistakes from the place where they can cause failures. In particular, provide fully featured non-production sandbox environments where people can explore and experiment safely, using real data without affecting real users.
- Test thoroughly at all levels, from unit testing to whole-system integration tests and manual tests.
- Allow quick and easy recovery from human errors, to minimize the impact in the case of failure.
- Set up detailed and clear monitoring, such as performance metrics and error rates.
- Implement good management practices and training. 

### How Important Is Reliability?
There are situations in which we may choose to sacrifice reliability in order to reduce development cost (e.g., when developing a prototype) or operational cost (e.g., for a service with a very narrow profit margin) - but we should be very conscious of when we are cutting corners.

## Scalability
Even if a system is working reliably today, that doesn’t mean it will necessarily work reliably in the future. 
One common reason for degradation is increased load (system has grown in concurrent users, or processing much larger volumes of data).
Scalability is the term we use to describe a system’s ability to cope with increased load.
Discussing scalability means considering questions like “If the system grows in a particular way, what are our options for coping with the growth?” and “How can we add computing resources to handle the additional load?”

### Describing Load
First, succinctly describe the current load on the system; only then can we discuss growth questions (what happens if our load doubles?). 
Load can be described with a few numbers which we call load parameters. The best choice of parameters depends on the architecture of your system: requests per second to a web server, the ratio of reads to write in a database, the number of simultaneously active users in a chat room, the hit rate on a cache, or something else.

### Describing Performance
Once you have described the load on your system, you can investigate what happens when the load increases. 
You can look at it in two ways:
- When you increase a load parameter and keep the system resources (CPU, memory, network bandwidth, etc.) unchanged, how is the performance of your system affected?
- When you increase a load parameter, how much do you need to increase the resources if you want to keep performance unchanged?
Both questions require performance numbers.
In a batch processing system such as Hadoop, we usually care about throughput — the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size. 
In online systems, what’s usually more important is the service’s response time — the time between a client sending a request and
receiving a response.

*Latency* and *response time* are often used synonymously, but they are not the same. The response time is what the client sees: besides the actual time to process the request (the service time), it includes network delays and queueing delays. Latency is the duration that a
request is waiting to be handled—during which it is latent, awaiting service.

Even if you only make the same request over and over again, you’ll get a slightly different response time on every try. In practice, in a system handling a variety of requests, the response time can vary a lot. We therefore need to think of response time not as a single number, but as a distribution of values that you can measure.

It’s common to see the average response time of a service reported. However, the mean is not a very good metric if you want to know your “typical” response time, because it doesn’t tell you how many users actually experienced that delay. Usually it is better to use percentiles. The median is also known as the 50th percentile, and sometimes abbreviated as p50. This makes the median a good metric if you want to know how long users typically have to wait: half of user requests are served in less than the median response time, and the other half take longer than the median.
In order to figure out how bad your outliers are, you can look at higher percentiles:t he 95th, 99th, and 99.9th percentiles are common (abbreviated p95, p99, and p999).

### Approaches for Coping with Load
How do we maintain good performance even when our load parameters increase by some amount?

An architecture that is appropriate for one level of load is unlikely to cope with 10 times that load. If you are working on a fast-growing service, it is therefore likely that you will need to rethink your architecture on every order of magnitude load increase — or perhaps even more often than that.
People often talk of a dichotomy between scaling up (vertical scaling, moving to a more powerful machine) and scaling out (horizontal scaling, distributing the load across multiple smaller machines). Distributing load across multiple machines is also known as a shared-nothing architecture. A system that can run on a single machine is often simpler, but high-end machines can become very expensive, so very intensive workloads often can’t avoid scaling out. In reality, good architectures usually involve a pragmatic mixture of approaches: for example, using several fairly powerful machines can still be simpler and cheaper than a large number of small virtual
machines.
Some systems are elastic, they can automatically add computing resources when they detect a load increase, whereas other systems are scaled manually (a human analyzes the capacity and decides to add more machines to the system). An elastic system can be useful if load is highly unpredictable, but manually scaled systems are simpler and may have fewer operational surprises.

While distributing stateless services across multiple machines is fairly straightforward, taking stateful data systems from a single node to a distributed setup can introduce a lot of additional complexity. For this reason, common wisdom until recently was to keep your database on a single node (scale up) until scaling cost or high-availability requirements forced you to make it distributed. 
As the tools and abstractions for distributed systems get better, this common wisdom may change, at least for some kinds of applications. It is conceivable that distributed data systems will become the default in the future, even for use cases that don’t handle large volumes of data or traffic.

The architecture of systems that operate at large scale is usually highly specific to the application — there is no such thing as a generic, one-size-fits-all scalable architecture (informally known as magic scaling sauce). The problem may be the volume of reads, the volume of writes, the volume of data to store, the complexity of the data, the response time requirements, the access patterns, or (usually) some mixture of all of these plus many more issues. For example, a system that is designed to handle 100,000 requests per second, each 1 kB in size, looks very different from a system that is designed for 3 requests per
minute, each 2 GB in size—even though the two systems have the same data throughput.

An architecture that scales well for a particular application is built around assumptions of which operations will be common and which will be rare — the load parameters. If those assumptions turn out to be wrong, the engineering effort for scaling is at best wasted, and at worst counterproductive. In an early-stage startup or an unproven product it’s usually more important to be able to iterate quickly on product features than it is to scale to some hypothetical future load.

## Maintainability
It is well known that the majority of the cost of software is not in its initial development, but in its ongoing maintenance—fixing bugs, keeping its systems operational, investigating failures, adapting it to new platforms, modifying it for new use cases, repaying technical debt, and adding new features.

Three design principles for software systems:
- Operability: Make it easy for operations teams to keep the system running smoothly.
- Simplicity: Make it easy for new engineers to understand the system, by removing as much complexity as possible from the system. (not the same as simplicity of the user interface.)
- Evolvability: Make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. Also known as extensibility, modifiability, or plasticity.

### Operability: Making Life Easy for Operations
It has been suggested that “good operations can often work around the limitations of bad (or incomplete) software, but good software cannot run reliably with bad operations”.

A good operations team typically is responsible for the following, and more:
• Monitoring the health of the system and quickly restoring service if it goes into a bad state
• Tracking down the cause of problems, such as system failures or degraded performance
• Keeping software and platforms up to date, including security patches
• Keeping tabs on how different systems affect each other, so that a problematic change can be avoided before it causes damage
• Anticipating future problems and solving them before they occur (e.g., capacity planning)
• Establishing good practices and tools for deployment, configuration management, and more
• Performing complex maintenance tasks, such as moving an application from one platform to another
• Maintaining the security of the system as configuration changes are made
• Defining processes that make operations predictable and help keep the production environment stable
• Preserving the organization’s knowledge about the system, even as individual people come and go

Good operability means making routine tasks easy, allowing the operations team to focus their efforts on high-value activities. Data systems can do various things to make routine tasks easy, including:
• Providing visibility into the runtime behavior and internals of the system, with good monitoring
• Providing good support for automation and integration with standard tools
• Avoiding dependency on individual machines (allowing machines to be taken down for maintenance while the system as a whole continues running uninterrupted)
• Providing good documentation and an easy-to-understand operational model (“If I do X, Y will happen”)
• Providing good default behavior, but also giving administrators the freedom to override defaults when needed
• Self-healing where appropriate, but also giving administrators manual control over the system state when needed
• Exhibiting predictable behavior, minimizing surprises

### Simplicity: Managing Complexity
Small software projects can have delightfully simple and expressive code, but as projects get larger, they often become very complex and difficult to understand. This complexity slows down everyone who needs to work on the system, further increasing the cost of maintenance. A software project mired in complexity is sometimes described as a big ball of mud.

There are various possible symptoms of complexity: explosion of the state space, tight coupling of modules, tangled dependencies, inconsistent naming and terminology,
hacks aimed at solving performance problems, special-casing to work around issues elsewhere, and many more.

When complexity makes maintenance hard, budgets and schedules are often overrun. In complex software, there is also a greater risk of introducing bugs when making a change: when the system is harder for developers to understand and reason about, hidden assumptions, unintended consequences, and unexpected interactions are more easily overlooked. Conversely, reducing complexity greatly improves the maintainability of software, and thus simplicity should be a key goal for the systems we build.

Making a system simpler does not necessarily mean reducing its functionality; it can also mean removing accidental complexity. Moseley and Marks define complexity as accidental if it is not inherent in the problem that the software solves (as seen by the users) but arises only from the implementation.

One of the best tools we have for removing accidental complexity is abstraction. A good abstraction can hide a great deal of implementation detail behind a clean, simple-to-understand façade. A good abstraction can also be used for a wide range of different applications. Not only is this reuse more efficient than reimplementing a similar thing multiple times, but it also leads to higher-quality software, as quality improvements in the abstracted component benefit all applications that use it.

For example, high-level programming languages are abstractions that hide machine code, CPU registers, and syscalls. SQL is an abstraction that hides complex on-disk and in-memory data structures, concurrent requests from other clients, and inconsistencies after crashes. Of course, when programming in a high-level language, we are still using machine code; we are just not using it directly, because the programming language abstraction saves us from having to think about it.
However, finding good abstractions is very hard. In the field of distributed systems, although there are many good algorithms, it is much less clear how we should be packaging them into abstractions that help us keep the complexity of the system at a manageable level.

### Evolvability: Making Change Easy
It’s extremely unlikely that your system’s requirements will remain unchanged forever. They are much more likely to be in constant flux: you learn new facts, previously unanticipated use cases emerge, business priorities change, users request new features, new platforms replace old platforms, legal or regulatory requirements change, growth of the system forces architectural changes, etc.
In terms of organizational processes, Agile working patterns provide a framework for adapting to change. The Agile community has also developed technical tools and patterns that are helpful when developing software in a frequently changing environment, such as test-driven development (TDD) and refactoring. Most discussions of these Agile techniques focus on a fairly small, local scale (a couple of source code files within the same application).

The ease with which you can modify a data system, and adapt it to changing requirements, is closely linked to its simplicity and its abstractions: simple and easy-to-understand systems are usually easier to modify than complex ones. But since this is such an important idea, we will use a different word to refer to agility on a data system level: evolvability.

## Summary
An application has to meet various requirements in order to be useful. There are functional requirements (what it should do, such as allowing data to be stored,
retrieved, searched, and processed in various ways), and some nonfunctional requirements (general properties like security, reliability, compliance, scalability, compatibility, and maintainability). In this chapter we discussed reliability, scalability, and maintainability in detail.

Reliability means making systems work correctly, even when faults occur. Faults can be in hardware (typically random and uncorrelated), software (bugs are typically systematic and hard to deal with), and humans (who inevitably make mistakes from time to time). Fault-tolerance techniques can hide certain types of faults from the end user.

Scalability means having strategies for keeping performance good, even when load increases. In order to discuss scalability, we first need ways of describing load and performance quantitatively. We briefly looked at Twitter’s home timelines as an example of describing load, and response time percentiles as a way of measuring performance. In a scalable system, you can add processing capacity in order to remain reliable under high load.

Maintainability has many facets, but in essence it’s about making life better for the engineering and operations teams who need to work with the system. Good abstractions can help reduce complexity and make the system easier to modify and adapt for new use cases. Good operability means having good visibility into the system’s health, and having effective ways of managing it.

There is unfortunately no easy fix for making applications reliable, scalable, or maintainable. However, there are certain patterns and techniques that keep reappearing in different kinds of applications. 

[figure1-1]: img/figure1-1.png "Figure 1-1"