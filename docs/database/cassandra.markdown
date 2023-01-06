
---
title: Master Cassandra Essentials
tags: database cassandra
parent: Database
---


# why cassandra?
- a lot data too much to be on one server.
- highly available - no downtime.
- data change rapidly, highly transactional.


# Origin
- built for facebook Messenger, fusing distributed architecture of dynamo, and data model of bigtable.
- Light weight transaction (LWT) vs ACID


# Data Model
- similar to SQL
- Tables: like relational table.
  - Narrow, a limited amount of columns
  - Tall, huge number of rows
  - No joins -> denormalize (normal and good in Cassandra)
  - No foreign key constraints 
- Keys:
  - Tables exists in *keyspace*(a namespace), replication is defined on keyspace level.
  - Normally use UUID instead of integer for key, cause there is no
    way to know the next integer among a cluster of node
  - All table accesses are by key
    - columns has to be in the primary key to be able to query (in the where clause)
  - Primary keys identify rows:
	- Composed of partition key and clustering key.
  - partition key:
    - a group of rows with a common partition key.
    - they are gauranteed to be local to a node.
  - **with clusgering order by**
  
	
	
	
