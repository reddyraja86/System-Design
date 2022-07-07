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
