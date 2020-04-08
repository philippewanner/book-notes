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

### Hardware Faults

### Software Errors

### Human Errors

### How Important Is Reliability?

## Scalability

### Describing Load

### Describing Performance

### Approaches for Coping with Load

## Maintainability

### Operability: Making Life Easy for Operations

### Simplicity: Managing Complexity

### Evolvability: Making Change Easy

## Summary

[figure1-1]: img/figure1-1.png "Figure 1-1"