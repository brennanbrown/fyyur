# Udacity

**Table of Contents:**

- [Udacity](#udacity)
  - [Assignment One: SQL and Data Models](#assignment-one-sql-and-data-models)
  - [Tech Stack](#tech-stack)
  - [Topics Covered](#topics-covered)
  - [Relational Databases](#relational-databases)
    - [Keys](#keys)
    - [SQL](#sql)
    - [Execution Plan](#execution-plan)
      - [On Performance](#on-performance)
    - [Client-Server Models](#client-server-models)

**Learning Schedule:**

_Estimate:_ 10 hrs/week for 4 months OR 40 hrs/week for 1 month.

- Start by defining a learning path.
  - Eg. Complete a course or an entire program, or just learn more.
  - Set short-term and long-term goals.
  - Write out an incremental plan with milestones to help you stay on track and not feel overwhelmed.

## Assignment One: SQL and Data Models

**Learning Outcomes:**

- How to do Create, Read, Update, and Delete (CRUD) operations
- How to apply these operations across both databases and web applications
- How to set up relationships between elements of an application
- How to think about important principles and patterns in building data models for a web application

## Tech Stack

- Python 3
- Flask
- PostgresSQL
- psycopg2
- SQLAlchemy
- Flask-SQLAlchemy

## Topics Covered

1. Interacting with a (remote) database

Backend developers need to interact with databases on a regular basis in order to manipulate and maintain the models behind their web applications. In this lesson, we'll build a foundational understanding of how those interactions work.

This foundational understanding will be essential in later lessons when we get into more advanced concepts related to database interactions.

In working with a database, we'll need to use a **Database Management System** (DBMS).

    A Database Management System (DBMS) is simply software that allows you to interact with a database (e.g., to access or modify the data in that database).

There are many different Database Management Systems out there, but the particular DBMS we'll be using is called PostgreSQL (or simply Postgres).

2. **Database Application Programming Interfaces** (DBAPIs)

Once we've looked at the basics of interacting with a database, we'll need to understand how to interface with that database from another language or web server framework (such as Python, NodeJS, Ruby on Rails, etc.). This is where DBAPIs come in.

In this lesson, we'll go over the basics of DBAPIs, and how they are used to interact with a database from another language (like Python).

3. psycopg2

Finally, we'll get some experience working with the widely used psycopg2 library, which will allow us to interact with a database from Python.

    psycopg2 is a database adapter that allows us to interact with a Postgres database from Python.

## Relational Databases

- A **database** is a collection of data.
  - A **database system** is a system for storing collections of data in some organized way.

Things to consider:

- **Persistance:** Allowing access later, after something was created.
- **Shared source of truth:** All the users of a web application can access the same data source.
- **Concurrency control:** Multiple users of a web application can write and read data at the same time, while the data is kept in a consistent and accurate state.
- **Efficient data type storage:** Ability to efficiently store of strings, numbers, booleans, etc.
- **Data integrity:** Rules for enforcing cohesion, such as _contraints_ or _triggers_.

### Keys

**Primary Key**

    The primary key is the unique identifier for the entire row, referring to one or more columns.
    If there are more multiple columns for the primary key, then the set of primary key columns is known as a composite key.

**Foreign Key**

    A primary key in another (foreign) table.
    Foreign keys are used to map relationships between tables.

- When a primary key consists of more than 1 column, we call the set of primary key columns a **composite key**.

### SQL

**Structured Query Language** is a domain-specific language used in programming and designed for managing data held in a relational database management system, or for stream processing in a relational data stream management system.

- Every relational database system has its own particular implementation of SQL.
- Different relational database systems have different "flavors" of SQL; each of these varieties is referred to as a _dialect_.

Manipulating Data:

    INSERT
    UPDATE
    DELETE

Querying Data:

    SELECT

Structuring Data:

    CREATE TABLE
    ALTER TABLE
    DROP TABLE
    ADD COLUMN
    DROP COLUMN

Joins & Groupings:

    INNER JOIN, OUTER JOINS (LEFT, RIGHT)
    GROUP BY, SUM, COUNT

- Inner joins between two tables returns rows of data that exist across all joined tables, excluding rows that may only exist in one of the tables but not the other table.
- Outer joins return every row that exists in the left (in a left outer join) or right (in a right outer join) joined table, while rendering NULL values on rows whose foreign key does not match a record in the other (right or left) table.
- The "left" table refers to the table to the left of the JOIN statement in the query, whereas the "right" table refers to the table to the right of the JOIN statement in the query.

### Execution Plan

- The DBMS takes a SQL query and generates a **execution plan** for the database engine to follow.
  - Takes a SQL query: `SELECT \* from vehicles WHERE driver_id = 3
  - _Go through every row in the vehicles table, whenever driver_id = 3, copy the row._

**Example:**

```sql
SELECT make, model from vehicles
JOIN drivers on vehicles.driver_id = drivers.id;
```

The execution plan looks like this:

- [Hash Join](https://www.depesz.com/2013/05/09/explaining-the-unexplainable-part-3/#hash-join): joins two record sets. It is the most expensive part of the plan, as indicated by the 'cost', it is joining every row! (Is that necessary? Can we accomplish finding out what we need while devising an execution plan that doesn't require this?) The hash join creates a hash in-memory that hashes based the driver_id column.
- [Seq Scan](https://www.depesz.com/2013/04/27/explaining-the-unexplainable-part-2/#seq-scan): a sequential scan is done across the entire vehicles table. This makes sense since we're looking to fetch all make and model information across all records in the vehicles table.
- Hash with Seq Scan on drivers: as the sequential scan continues, the join key is checked in the Hash returned from Step 1, where if it does NOT exist, given that this is an Inner Join, we ignore that row, and if it does exist (a record was found that does intersect between the vehicles and drivers tables), then we fetch the row from the hash to generate the outputted, joined row.

#### On Performance

- Learning how to write efficient queries is practically its own field: https://www.ibm.com/support/knowledgecenter/SSZLC2_9.0.0/com.ibm.commerce.developer.doc/refs/rsdperformanceworkspaces.htm
- There are techniques for improving the performance of SQL queries to consider, we can use critical indexes to speed up information lookups: http://www.postgresqltutorial.com/postgresql-indexes/postgresql-create-index/
  - and there are helpful utilities like SQL views for splitting queries into subroutines: https://db.grussell.org/sql3.html#_myauto10

### Client-Server Models

- In order to build database-backed web applications, we first need to understand how servers, clients, and databases interact.
- A **server** is a centralized program that communicates over a network (such as the Internet) to serve clients.
- And a **client** is a program (like the web browser on your computer) that can request data from a server.
- When you go to a web page in your browser, your browser (the client) makes a request to the server—which then returns the data for that page.

Relational database systems follow a client-server model:

**Servers, Clients, Hosts**

    In a Client-Server Model, a server serves many clients.
    Servers and clients are programs that run on hosts.
    Hosts are computers connected over a network (like the internet!).

**Requests and Responses**

    A client sends a request to the server
    The server's job is to fulfill the request with a response it sends back to the client.
    Requests and responses are served via a communication protocol, which sets up the expectations and rules for how the communication occurs between servers and clients.

**Relational Database Clients**

    A database client is any program that sends requests to a database
    In some cases, the database client is a web server! When your browser makes a request, the web server acts as a server (fulfilling that request), but when the web server requests data from the database, it is acting as a client to that database—and the database is the server (because it is fulfilling the request).

