### Flat files storages

- .csv,.txt,.xls
- limited storage
- limited programatic support
- limited security/backup options

### DBMS

Software that stores data securely.

### Types Of DBMS

- RDBMS : stores data in tabular form

  - data is generally normalised to minimize redundancy
  - querying data becomes slow if the data size increases and joins are involved.Sql is used to perform CRUD(insert/select/update/delete) operation.

  - Oracle, MySQL, Microsoft SQL Server, and PostgreSQ

  - Rigid schema

- NoSql Databases : stores data in the form of documents

  - Document: JSON documents, Key-value: key-value pairs, Wide-column: tables with rows and dynamic columns, Graph: nodes and edges

  - Document: MongoDB and CouchDB, Key-value: Redis and DynamoDB, Wide-column: Cassandra and HBase, Graph: Neo4j and Amazon Neptune
  - Document: general purpose, Key-value: large amounts of data with simple lookup queries, Wide-column: large amounts of data with predictable query patterns, Graph: analyzing and traversing relationships between connected data

  - Flexible schema

  - Trade off b/w normalization and cost of storing duplicates(very low)
