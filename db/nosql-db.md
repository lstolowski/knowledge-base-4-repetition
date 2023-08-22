## Nosql
- pros and cons https://mansfeld.pl/bazy-danych/bazy-danych-nosql-zalety-wady/
- Dynamo 
  - keys
  - Java api
- mongodb
  - features
- Redis
- GraphQL

### Podział baz nosql
- bazy danych dokumentów – MongoDB, CouchDB,
- kolumnowe bazy danych – Apache Cassandra,
- bazy danych typu klucz-wartość – Redis, Couchbase Server, DynamoDB,
- systemy pamięci podręcznej – Redis, Memcached,
- grafowe bazy danych – Neo4J, ArangoDB, FaunaDB, OrientDB

### Zagadnienia

- CAP Theorem/Brewer's Theorem  https://www.ibm.com/topics/cap-theorem
  - trójkąt gdzie tylko 2 punkty można zapewnić: Consistency, Availability, Partition tolerance
  - bazy typu CP, AP, CA
  - MongoDB - przykład CP, Cassandra - AP, przykłady CA w distributed systems nie istnieją (musza mieć partycje)


