# Design Microservices Architecture with Patterns & Principles

*Author: Mehmet Ozkaya*  
*Published: September 6, 2021*  
*Link: [Medium Article](https://medium.com/design-microservices-architecture-with-patterns/microservices-architecture-2bec9da7d42a)*

---

## Introduction

In this article, we’re going to learn about Microservices Architecture. We'll start with definitions, benefits, and challenges. After that, we will design a Microservices architecture in the e-commerce domain.

By the end of the article, you will learn where and when to apply Microservices Architecture by designing systems with high availability, high scalability, and low latency.

---

## Step by Step Design Architectures with Course

I have just published a new course — *Design Microservices Architecture with Patterns & Principles*.

In this course, we’re going to learn how to design Microservices Architecture using Design Patterns, Principles, and Best Practices. We will start by designing Monolithic to Event-Driven Microservices step by step, using the right architecture design patterns and techniques.

---

## What are Microservices?

Microservices are small business services that can work together and can be deployed autonomously/independently. These services communicate with each other over the network and bring many advantages. One of the biggest advantages is that they can be deployed independently. Moreover, they offer the opportunity to work with many different technologies.

From Martin Fowler's Microservices article:

> The microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP or gRPC API.

These services are built around business capabilities and are independently deployable through fully automated deployment processes. There is minimal centralized management of these services, which may be written in different programming languages and use different data storage technologies.

So, we can say that Microservices architecture is a cloud-native architectural approach in which applications are composed of many loosely coupled and independently deployable smaller components.

**Microservices:**

- Have their own technology stack, including the database and data management model.
- Communicate with each other over a combination of REST APIs, event streaming, and message brokers.
- Are organized by business capability, with the line separating services often referred to as a bounded context.

---

## Microservices Characteristics

Microservices are small, independent, and loosely coupled. A single small team of developers can write and maintain a service. Each service is a separate codebase, which can be managed by a small development team.

Services can be deployed independently. A team can update an existing service without rebuilding and redeploying the entire application. Services are responsible for persisting their own data or external state. This differs from the traditional model, where a separate data layer handles data persistence.

Services communicate with each other by using well-defined APIs. Internal implementation details of each service are hidden from other services. Services don’t need to share the same technology stack, libraries, or frameworks.

During the article, we will refer to these microservices characteristics when we develop our reference application.

Referring to Martin Fowler's article, there are some common characteristics for microservices architectures:

- **Componentization via Services:** Component is a unit of software that is independently replaceable and upgradeable.
- **Organized around Business Capabilities:** The microservice approach to division is splitting up into services organized by business capability.
- **Products not Projects:** This is Amazon’s notion of “you build it, you run it,” where a development team takes full responsibility for the software in production.
- **Smart Endpoints and Dumb Pipes:** Microservices aim to be as decoupled and cohesive as possible, owning their own domain logic and using RESTful APIs.
- **Decentralized Governance:** Sharing useful and tested code as libraries encourages other developers to solve similar problems in similar ways.
- **Decentralized Data Management:** Microservices prefer letting each service manage its own database, either different instances of the same database technology or entirely different database systems.
- **Infrastructure Automation:** Automate deployment to each new environment and for every microservice separately.

---

## Benefits of Microservices Architecture

We are going to see the benefits, pros, and advantages of microservices architecture. Let's elaborate on these benefits one by one.

### Data Isolation

Since microservices follow the database-per-service pattern, databases are separated from each other according to microservices design. This makes it easier to perform schema updates because only a single database is affected. In a monolithic application, schema updates can become very challenging and risky.

### Data Integrity

Each microservice has its own data persistence. So, data consistency can be a challenge. Mostly, we should follow eventual consistency where possible. But transactional operations will always be challenging.

Nevertheless, these challenges aren’t stopping the adoption of microservices. Most organizations accept these challenges and adapt their technologies to microservices architecture to gain its benefits.

---

## Reference Architectures of Microservices Architecture

We are going to see examples of reference architectures of Microservices Architecture. We will follow these reference architectures and create our own e-commerce architecture.

### Reference Microservice Architecture 1

As you can see, the image has several microservices and one API Gateway, which provides communication between microservices and client applications.

### Reference Microservice Architecture 2

This is a common view of microservices architecture. It has several microservices, an API Gateway, and a message broker to provide communication among microservices.

### Reference Microservice Architecture 3

Another reference architecture. This is also one of the e-commerce reference architectures that we will evolve step by step.

### Reference Microservice Architecture 4

An advanced microservices architecture. You can see several API Gateways for different client applications and several microservices communicating via Event Bus systems.

We will see why we use these components when architecting microservice architecture step by step in the next section.

---

## Design Microservice Architecture — E-Commerce App

We are going to design the Microservice architecture step by step. Iterate the architecture design one by one as per requirements. We had a requirement to save the orders and see the baskets, so we need database design in this architecture.

As you can see, we have applied “Database per Microservice” and assigned a database for every microservice in our e-commerce application. These databases can be polyglot persistence. That means:

- Product microservice can use a NoSQL document database.
- Shopping Cart microservice can use a NoSQL key-value pair database.
- Order microservice can use a relational database as per microservice data storage requirements.

We have designed our e-commerce application as a Microservices architecture and handled the functional and non-functional requirements like scalability, reliability, and so on.

---

## Technology Choices — Adapting Technology Stack

We are going to adapt the technology stack and implement possible technology choices.

Microservices are polyglot environments. You can pick any tech stack per microservice. Communication will be handled via REST APIs.

**Backend Web API Languages and Frameworks:**

- Java — Spring Boot
- C# — ASP.NET
- JavaScript — Node.js
- Python — Django
- Python — Flask

Since microservices are polyglot environments, you can pick one of them for each particular microservice.

**Databases:**

*NoSQL:*

- MongoDB
- Redis
- Cassandra

*Relational:*

- PostgreSQL
- MySQL
- Oracle
- SQL Server

These are the options that we can use in our e-commerce microservice architecture. We will decide on these tools as per our requirements and company IT strategy.

---

## What's Next?

We see that UI and microservices communication are direct, and it seems hard to manage communications. Now, we should focus on microservices communications by applying the API Gateway pattern and evolving this architecture step by step.

---

## Step by Step Design Architectures with Course

I have just published a new course — *Design Microservices Architecture with Patterns & Principles*.

In this course, we’re going to learn how to design Microservices Architecture using Design Patterns, Principles, and Best Practices. We will start by designing Monolithic to Event-Driven Microservices step by step, using the right architecture design patterns and techniques.

---

*Note: This content is based on the original article by Mehmet Ozkaya. For more details and diagrams, please refer to the [original Medium article](https://medium.com/design-microservices-architecture-with-patterns/microservices-architecture-2bec9da7d42a).*