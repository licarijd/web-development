Redis and Databases


Classification of NoSQL Databases 


Key-Value

- Redis 


Document 

- MongoDB 


Wide Column 

- Cassandra 


Graph 

- neo4j


Redis 

Redis is an in-memory database, and is used for short-lived data in applications. It is 
often used for things like sessions, and is fast + scalable.

- small pieces of data 

- stored in memory, not on disk 

- takes snapshots every one in a while to save data to disk

- data should not be important 


Relational Databases 

- 2 or more tables of columns and rows 

- each row represents an entry 

- each column represents an entry 

- each column sorts a specific kind of info, like name, address, etc

- the relation between tables and fields is called a schema, which must be clearly defined 

- uses SQL, which allows us to communicate with the database 


NoSQL / Non Relational Databases 

- let's you build an application without first having to define the schema

- can be thought of as folders, which organize information of all types 


Example - MongoDB

- document oriented 

- stores information as documents

- instead of columns of attribute data, we have entire documents for each entity 
(entities can be though of as a row in a database, eg. { id, name, address, age },
where in a relational DB, we'd have a column for each attribute)

- non relational databases have their own query languages

https://redis.io/commands/command

https://blog.faodailtechnology.com/getting-started-with-redis-i-ed55578f36d1

Look up Redis data types and data structures.