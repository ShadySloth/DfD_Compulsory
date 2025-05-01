# Database for Developers Compulsory 2

This is the compulsory covers the following topics and decisions in design of them:
- Database Selection
- Database Schema and Storage Strategy
- Integration of Cloud Storage
- Caching Strategy
- CQRS Implementation
- Transaction Management

## Introduction
The project is to design a backend for an e-commerce platform. The platform allows users to buy and 
sell items, write reviews, and manage their orders. 

A diagram of the system is provided in the project root directory. The diagram shows gives a high-level 
view over components of the system, and how they are connected. Some classes are merged together and 
represented as a single component. This is done to simplify the diagram and make it easier to understand.

## Database Selection
For databases in the project, we have chosen PostgreSQL and MongoDB.

### Relational Database
- **PostgreSQL**: We have chosen PostgreSQL as our relational database because it is a powerful, 
open-source object-relational database system that uses and extends the SQL language.

### NoSQL Database
- **MongoDB**: We have chosen MongoDB as our NoSQL database because it is a document-oriented with
fast read operations.

### Cache
- **Redis**: We have chosen Redis as our caching solution because it is an in-memory key-value data 
structure.

## Database Schema and Storage Strategy
For the database schema, we have the following entities:
- **User**: Represents a user in the system. This could be both a customer and a seller.
- **Item**: Represents an item that can be bought or sold in the system.
- **Order**: Represents an order made by a user for one or more items.
- **Review**: Represents a review made by a user of an seller

For bigger objects, such as images, we will use cloud storage. This is because these objects can be large
and cloud storage offers an easy, scalable solution for storing and retrieving them.

## Integration of Cloud Storage
For cloud storage, there will be used MinIO. MinIO is a high-performance, S3-compatible object storage 
solution that can be used to store large amounts of unstructured data. With it being S3-compatible, it 
can be used with existing S3-compatible tools and libraries, and if one day we want to move to AWS S3, 
it will be a simple task.

In the solution, it will be used for images and larger objects, that could relate to items or users.

## Caching Strategy
For caching, we will use Redis. Redis is an in-memory data structure store that can be used for very
fast data access. It is suitable for caching frequently accessed data. 

We will also use the cache, to temporarily store newly created or updated items, orders, and reviews.
This will help hiding the fact that we will have an eventual consistency in the system. Cached objects
this way will have a short TTL, and will be removed from the cache after a short time.

## CQRS Implementation
CQRS (Command Query Responsibility Segregation) is a pattern that separates the read and write. For 
this, we will have an internal event bus that will be used to handle the commands and queries events.
There will then be subscribers and publishers for the events. The commands will be used to create, 
update, and delete.

For better overview of the system, we recommend checking the Diagram.jpg file in the root of the project.

## Transaction Management
### Relational Database
Communication with the relational database is done using Entity Framework Core. This will in itself include
transaction management, and will be used to handle the transactions, and cover the ACID principles.  
EF Core can only guarantee it for the relational Database, since the update into the NoSQL database will
be done via the event bus.

### NoSQL Database
For the NoSQL database, there will also be transaction management, but this will need to be implmeneted 
a bit more manually. We will be using the MongoDB.Driver package for communication with the database.
This package will allow us to handle transactions.