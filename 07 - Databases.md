# Databases

## Multi-AZ vs Read Replicas

* Multi-AZ
  * Single DNS name
  * Automatic failover
  * Synchronous replication to standby instance
  * One region only
  * Useful for disaster recovery
* Read replicas
  * Each replica has its own connection string
  * Asynchronous replication, eventually consistent reads
  * Up to 15 replicas for Aurora, 5 for other DBs
  * Useful for scaling read traffic, cross-region disaster recovery
  * Not supported for Oracle
  * Replicas can also be multi-AZ
  * Aurora specifically supports cross-region replication and promotion of replicas to master

## CloudWatch

* Important metrics
  * DatabaseConnections
  * SwapUsage
  * ReadIOPS and WriteIOPS
  * ReadLatency and WriteLatency
  * ReadThroughPut and WriteThroughPut
  * DiskQueueDepth
  * FreeStorageSpace
* Can enable enahcned monitoring for advanced hardware metrics

## RDS Performance Insights

* Visualize database performance
* Filter Loads
  * By waits - find bottleneck of request
  * By SQL statements - find problematic SQL statement
  * By hosts - find server requesting from DB the most
  * By users - find user requesting from DB the most
