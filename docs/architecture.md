# Architecture Overview

This document describes the architecture of the previous PetMicroservices project and captures the key lessons we want to preserve for the new Smart Forms system.

## 1. What the old system was

The old project was a microservices-based application built around a modular backend and a frontend application. The main idea was to split responsibilities across several independent services instead of putting everything into one large application.

The project included:
- API Gateway
- Comment Service
- Template Service
- Answer Service
- Web frontend application
- SQL Server database
- RabbitMQ message broker

## 2. Main architecture pattern

The system used a classic API Gateway + multiple services model:

- Clients communicate with a single entry point: the API Gateway
- The API Gateway routes requests to relevant downstream services
- Each microservice owns a specific business domain
- Services are deployed as separate containers or processes
- Shared infrastructure components such as SQL Server and RabbitMQ are used by services

This pattern helped separate responsibilities and gave each service a clear purpose.

## 3. Services and responsibilities

### 3.1 API Gateway

The API Gateway was the main external entry point for the application.

Responsibilities:
- route incoming HTTP requests to the right service
- hide internal service topology from the client
- provide a single URL for API access
- centralize routing logic

Technology used:
- Ocelot
- ASP.NET Core

Example behavior:
- `/comment` -> Comment Service
- `/template` -> Template Service
- `/answer` -> Answer Service

### 3.2 Comment Service

This service handled comment-related functionality.

Responsibilities:
- manage comments
- manage comment templates
- expose REST endpoints for comment operations

Typical purpose:
- create comment
- get comments by template
- update or delete comments

### 3.3 Template Service

This service was responsible for general template and metadata entities.

Responsibilities:
- users
- topics
- tags
- templates
- template-related CRUD operations

This service acted like a general content and metadata domain.

### 3.4 Answer Service

This service was more domain-specific and handled answers/questions logic.

Responsibilities:
- answer management
- question management
- user records in the answer domain
- template data used by answer flows
- gRPC and REST support

This service showed how a domain can have structured APIs and event-driven integration patterns.

### 3.5 Web Application

The frontend was a web application that connected to the backend through the API Gateway.

Responsibilities:
- user interface for the product
- request routing to backend services through one API endpoint
- display forms, data, and lists

This made the frontend simpler and separated it from backend complexity.

## 4. Communication model

The old project used two communication styles:

### 4.1 HTTP / REST between client and gateway

The browser or client app called the API Gateway over HTTP.

### 4.2 Service-to-service communication

Microservices were connected through the gateway and sometimes via messaging.

The project also used RabbitMQ for asynchronous communication, especially around the answer domain. This is important because it shows the value of event-driven patterns for decoupling workloads and processing.

In short:
- synchronous HTTP for request/response flows
- asynchronous messaging for decoupled processing and integration

## 5. Data storage pattern

The old project used a shared infrastructure approach:

- each service had its own database or at least a dedicated logical data area
- SQL Server was the main relational database platform
- Docker Compose started services with environment variables and connection strings

This was a typical microservices pattern: separate data ownership per service.

Example:
- CommentDb
- TemplateDb
- AnswerDb

This helped keep domains isolated and simplified future service evolution.

## 6. Local environment setup

The project used Docker Compose to run the entire environment together.

Included systems:
- SQL Server
- RabbitMQ
- backend microservices
- frontend app
- API Gateway

This made local onboarding much easier and demonstrated a good developer experience.

## 7. Why this architecture was useful

The old project showed several strengths:

- clear service boundaries
- easier independent scaling of services
- better separation of concerns
- easier maintenance of domain-oriented modules
- possibility of different service technologies in the future
- good learning project for distributed systems

## 8. Problems and lessons learned

Even though the architecture was useful, it also showed the complexity of microservices.

Main challenges:
- more infrastructure to manage
- more deployment complexity
- cross-service debugging is harder
- data consistency becomes more complex
- each service needs separate configuration and monitoring
- more moving parts than a monolith

This is the biggest lesson: microservices are powerful, but only when the system is truly complex enough to justify them.

## 9. What to keep for Smart Forms

For Smart Forms, we want a simpler and more pragmatic version of microservices architecture.

Recommended principles:
- keep the API Gateway pattern if traffic and routing grow
- keep separate domain services only where it adds value
- do not over-split the application too early
- start with a clean modular monolith or few services, not a large distributed system

## 10. Recommended architecture for the new project

For Smart Forms, we should preserve the good parts of the old architecture while reducing unnecessary complexity.

Preferred shape:
- Angular frontend
- ASP.NET Core Minimal API backend
- modular business services inside the backend
- optional domain/service split later if needed
- SQL Server / Azure SQL for persistence
- Docker Compose for local development
- optional API Gateway only if multi-service evolution is needed

### Preferred strategy

Use a pragmatic architecture:
- start with a single API backend and use modular internal boundaries
- if the product grows, split into services by domain such as:
  - identity
  - forms
  - submissions
  - reporting
- keep the gateway only when cross-domain routing becomes necessary

This preserves the microservices learning but avoids starting with too much operational overhead.

## 11. CQRS and MediatR direction

For Smart Forms we plan to use CQRS as the application pattern. The backend will separate commands and queries so that write operations and read operations are handled independently.

This is useful because the project has two very different workloads:
- write-heavy flows: create/update forms, publish forms, submit answers
- read-heavy flows: dashboard, list forms, review submissions, analytics, export

### Why MediatR

MediatR is useful for internal in-process messaging between modules inside the same service.

It gives us:
- cleaner handler-based architecture
- easier testability
- clear separation between commands, queries, and notifications
- faster future refactoring as the codebase grows

Important: MediatR is not a replacement for an API Gateway. It does not route external HTTP traffic. It solves internal application composition and orchestration.

### MediatR vs broker-based messaging

MediatR is designed for in-memory communication inside one process. It is ideal for the initial version of Smart Forms.

Later, if the system grows and we need asynchronous communication between services, we can introduce a broker such as:
- RabbitMQ
- Azure Service Bus

A good migration path is:
1. use MediatR for commands and domain events inside the app
2. define domain events and integration events
3. add an event abstraction such as `IEventPublisher`
4. implement it with RabbitMQ or Azure Service Bus later
5. use the Outbox pattern for reliability if needed

This means the architecture remains flexible: we can start with a simple internal CQRS model and transition to broker-based messaging when real product complexity requires it.

## 12. Final conclusion

The old PetMicroservices project is a strong reference for learning microservice design:
- API Gateway as a front door
- service isolation by business capability
- external infrastructure like SQL and RabbitMQ
- Docker-based local development
- domain-oriented service boundaries

For Smart Forms, the right approach is not to copy the old project exactly, but to preserve the architectural ideas that are valuable while simplifying the implementation so the project remains maintainable and fast to build.

The main takeaway is:
- microservices are good for business decomposition
- but for a new product, start lean and grow only when complexity requires it
- CQRS + MediatR is a good fit for the Smart Forms backend and provides a safe path toward broker-based messaging in the future
