# System Design Interview Template

---

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
      - Micro services scaling
      - DB Scaling
      - Availability, Consistency and Scale story for each component
      - Consistency and availability patterns
7. JUSTIFY
   1. Throughput of each layer
   2. Latency caused between each layer
   3. Overall latency justification

# Topics

---

1. Latency and Throughput requirements
2. Consistency vs Availability [Weak/strong/eventual => consistency | Failover/replication => availability]
3. DNS
   4
   b) CDN [Push vs Pull]
   c) Load Balancers [Active-Passive, Active-Active, Layer 4, Layer 7]
   d) Reverse Proxy/Forward Proxy
   e) Application layer scaling [Microservices, Service Discovery]
   f) DB [RDBMS, NoSQL] > RDBMS >> Master-slave, Master-master, Federation, Sharding, Denormalization, SQL Tuning > NoSQL >> Key-Value, Wide-Column, Graph, Document
   Fast-lookups:
   ------------- >>> RAM [Bounded size] => Redis, Memcached >>> AP [Unbounded size] => Cassandra, RIAK, Voldemort >>> CP [Unbounded size] => HBase, MongoDB, Couchbase, DynamoDB
   g) Caches > cache stampade > Client caching, CDN caching, Webserver caching, Database caching, Application caching, Cache @Query level, Cache @Object level > Eviction policies: >> Cache aside >> Write through >> Write behind >> Refresh ahead
   h) Asynchronism > Message queues > Task queues > Back pressure
   i) Communication > TCP > UDP > REST > RPC
   j) Indexing - solr

(5) DEEP DIVE [15-20 min]
(1) Scaling the algorithm
(2) Scaling individual components:
-> Availability, Consistency and Scale story for each component
-> Consistency and availability patterns
(3) Think about the following components, how they would fit in and how it would help
a) DNS
b) CDN [Push vs Pull]
c) Load Balancers [Active-Passive, Active-Active, Layer 4, Layer 7]
d) Reverse Proxy/Forward Proxy
e) Application layer scaling [Microservices, Service Discovery]
f) DB [RDBMS, NoSQL] > RDBMS >> Master-slave, Master-master, Federation, Sharding, Denormalization, SQL Tuning > NoSQL >> Key-Value, Wide-Column, Graph, Document
Fast-lookups:
------------- >>> RAM [Bounded size] => Redis, Memcached >>> AP [Unbounded size] => Cassandra, RIAK, Voldemort >>> CP [Unbounded size] => HBase, MongoDB, Couchbase, DynamoDB
g) Caches > cache stampade > Client caching, CDN caching, Webserver caching, Database caching, Application caching, Cache @Query level, Cache @Object level > Eviction policies: >> Cache aside >> Write through >> Write behind >> Refresh ahead
h) Asynchronism > Message queues > Task queues > Back pressure
i) Communication > TCP > UDP > REST > RPC
j) Indexing - solr
(6) JUSTIFY [5 min]
(1) Throughput of each layer
(2) Latency caused between each layer
(3) Overall latency justification
