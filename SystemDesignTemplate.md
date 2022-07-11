# System Design Interview Template

1. Requirements Gathering
   1. Functional Requirements
      - Different users ( Drivers/customers)
      - Do we need to consider Administartion scenarios
      - How many user?
      - How many Active user per day?
      - How many years data should be stored ?
   2. Non-Functional Reqirements
      - Scalability
      - Availability
      - Consistancy ( Some modules should be consistent like payments)
      - Fault Tolerant
      - Rate Limiting
      - Latency
2. Capacity Planning

   1. Find the Number of requests (Read and Write)
      - How many requests per Day (Ex Drivers 100k customers 500k)
      - How many request per second ( 100k/24*3600 , 500k/24*3600 )
   2. Find the memory Utilization
      - Each request how much memory is required ( Ex : Driver message 12 bytes ,12*100k/24*3600 bytes)
      - How much data stored in cache
   3. Expected Latency and Throughput requirements
   4. Expected Consistency vs Availability [Weak/strong/eventual => consistency | Failover/replication => availability]

3. High level Design
   - Introduce (cache/Index)
   - Websockets
   - Reactive streams
4. API Design with payload
5. Database schema Design
6. Deep Dive in to Design
   1. Scaling Algorithms/Consistent Hashing etc
   2. Scaling each Component
      - Micro services Communication/ scaling
      - DB Scaling
      - Availability, Consistency and Scale story for each component
      - Consistency and availability patterns
7. JUSTIFY
   1. Throughput of each layer
   2. Latency caused between each layer
   3. Overall latency justification

# Topics

1. Latency and Throughput requirements
2. Consistency vs Availability [Weak/strong/eventual => consistency | Failover/replication => availability]
3. DNS
4. CDN [Push vs Pull]
5. Load Balancers [Active-Passive, Active-Active, Layer 4, Layer 7]
6. Reverse Proxy/Forward Proxy
7. Application layer scaling [Microservices, Service Discovery]
8. DB [RDBMS, NoSQL]

   1. RDBMS

   - Master-slave
   - Master-master
   - Federation
   - Sharding

   2. NoSQL

   - Key-Value
   - Wide-Column
   - Graph
   - Document
   - Fast-lookups:
     - RAM [Bounded size] => Redis, Memcached
     - AP [Unbounded size] => Cassandra, RIAK, Voldemort
     - CP [Unbounded size] => HBase, MongoDB, Couchbase, DynamoDB

9. Caches

   1. Client caching, CDN caching, Webserver caching, Database caching, Application caching, Cache @Query level, Cache @Object level
   2. Cache Strategies -
      1. Cache aside
      2. Write through
      3. Write behind
      4. Refresh ahead
   3. Cache Eviction Policies - LRU
   4. cache stampade

10. Asynchronism

- Message queues
- Task queues
- Back pressure

11. Communication

- TCP
- UDP
- REST
- RPC

12. Indexing - solr

## Consistency vs Avaialbaility

- **As per CAP thorem :** In case of network partiotion you can have Availability or Consistency
- **Tradeoffs :** In order to achieve a higher degree of consistency, a write operation should be successful once all the replica successfully process writes.This increases the latency and the systems are not highly avaialable.

  - **To Achieve both consistency and avialability :** we can have read and write quorum ,in which we need to write 2 out of 3 nodes and same should be followed for read operations.This is a tradeoff.

  - To Make More Consistent Systems in Netwrok Partition
    - Write is successful once write operation is successful all replica nodes  
      OR
    - Write is successful : Out of n nodes m nodes are successful.
    - `Where m<n High Consistency with high m value for write operations ,hight avaialbity when less m value `

##### Consistency Types

1. Strong : At any point of time all nodes will have same data.
2. Eventual : If an update is made on a node then the updated value will eventually be propagated all nodes to make then consistent.

##### Replication :

- Replication : Data is backedup in another server.(master to backup read DB)
- Replication Lag : Time taken to replicate the data from master to read/backup nodes,If replication lag is more and the number of read nodes are more then users may not get the latest updated data as it will take some time to sync.

- Replication Latency can be fixed by following either Sync or Async replication or hybrid replication in which certain nu

- Syn vs Async Replication :

  - Sync : A master node recives write request and it will apply write on all the replication nodes,once it recieves the ack from all then considers write is successful. A write Transaction to the master is blocked until it is written to the backup nodes.
    - It reduce Application performance.
  - Async : A write Transaction to the master is considered as successful if it written to master ,then asynchrnously the writes are applied to backup nodes.

- How repplication is done between write and read databases ( depends on Database)

  - Time based snap shot
  - Change Data stream

- Replication Types :
  - Master-Master : Multi master node allows write operations on multiple nodes.
  - Master-Backup (Read/Write) : Write opertions are performed on master node and read operations are done on replication nodes.
- Failover : when the primary database fails and one of the standby databases is transitioned to take over the primary role.

Benefits of replication :

1.  Faulttolerance
2.  Scalability
3.  Avaialbility
4.  Latency Reduction

Disadvantages :

1. Consistency issue

Solution : Improve consistency in replication using read after write consistency.

## DNS :

- Map the domain name to ip address ( Rout53 Amzon)

![alt](https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png)

## CDN :
