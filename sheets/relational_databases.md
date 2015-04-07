#Relational Databases#

In Codd we trust!

Databases are a collection of 'tables', called 'relations', these are metaphorical reperesentations of an entity/object in tabular format.

Coulmns are the attributes of an entity

Rows contain the instances of the entity known as 'tuples' or 'records'

DBMS must have ACID transactions

```sh
Atomicity - "All or Nothing" - if one part of the transaction fails, the entire transaction must fail and the database be left in an unchanged state.

Consistency - The transaction must bring a database from one valid state to another. Any data written to the database must meet the defined rules.

Isolation - Concurrent transactions must result in a state that would be obtained if they were executed serially.

Durability - Once a transaction has been commited it will remain so, despite crashes, power loss etc.
```
To meet these database must be able to consitently access one item solely in the database. To do this when a row is written to a database it is assigned a primary key.

Primary Keys - used within a database to define the relationships between entities, to create a relationship the primary key will appear in the child table as a 'foreign key'. These can manage a one-to-one relationship or one-to-may relationship. Many-to-many relationships can be managed in many RDMS with a link table containing foreign keys from both sides of the relationship.

##**The primary key in the parent is the foreign key in the child**##

Stored Procedures - Executeable code, generally stored in the database, allowing specific

##Normalization##

Normalization is a key topic in relation databases that looks to minimize data redundancy in the data store. Here the idea of the primary key, foreign key relationship allows large tables to be split into smaller tables, removing small sets of repeated data to individual tables, storing them only once with a primary/foreign key relationship.
