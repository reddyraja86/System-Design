# System Design Interview Template

### 1. Requirements Gathering

- ### Functional Requirements
  - Different users ( Drivers/customers)
  - Do we need to consider Administartion scenarios
  - How many user?
  - How many Active user per day?
  - How many years data should be stored ?
- ### Non-Functional Reqirements
  - Scalability
  - Availability
  - Consistancy ( Some modules should be consistent like payments)
  - Fault Tolerant
  - Rate Limiting
  - Latency

2. Capacity Planning ( Storage & Network bandwidth)

   - Find the Number of requests (Read and Write)
     - How many requests per Day (Ex Drivers 100k customers 500k)
     - How many request per second ( 100k/24*3600 , 500k/24*3600 )
   - Find the memory Utilization
     - Each request how much memory is required ( Ex : Driver message 12 bytes ,12*100k/24*3600 bytes)
     - How much data stored in cache
   - Expected Latency and Throughput requirements
   - Expected Consistency vs Availability [Weak/strong/eventual => consistency | Failover/replication => availability]

3. High level Design
   - Introduce (cache/Index)
   - Websockets/Rest Apis
   - Reactive streams
4. API Design with payload
5. Database schema Design
6. Deep Dive in to Design
   - Scaling Algorithms/Consistent Hashing etc
   - Scaling each Component
     - Micro services Communication/ scaling
     - DB Scaling
     - Availability, Consistency and Scale story for each component
     - Consistency and availability patterns
7. JUSTIFY
   - Throughput of each layer
   - Latency caused between each layer
   - Overall latency justification

# Topics

1. Latency and Throughput requirements
2. Consistency vs Availability [Weak/strong/eventual => consistency | Failover/replication => availability]
3. DNS
4. CDN [Push vs Pull]
5. Load Balancers [Active-Passive, Active-Active, Layer 4, Layer 7]
6. Reverse Proxy/Forward Proxy
7. Application layer scaling [Microservices, Service Discovery]
8. DB [RDBMS, NoSQL]

   - RDBMS

     - Master-slave
     - Master-master
     - Federation
     - Sharding

   - NoSQL

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

## 2.Consistency vs Avaialbaility

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

Solution : Improve consistency in replication using **_read after write consistency._**

## 3.DNS :

- Map the domain name to ip address ( Rout53 Amzon)

![alt](https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png)

## 4.CDN :

Content deliver Network can be places at different locations and static data is CDN closer to the user. ( AWS cloudfront)

##### Benefits :

1. Lowset latency
2. Increses the appliation performance ( as static data server from CDN)
3. Less burden on servers

##### CDN Types

1. Push : Application/user will push the in to the CDN.When the content is updated the you need to send the updated content again.
2. Pull : CDN is reposible for pulling the data from the application/original server.Pull CDN is often used for smaller files, such as website images, javascript, css and html files.

## 5.Load Balacers :

Routes the Request to one of the server.

##### Load Types :

- L4 - Transport Layer :
  - Generally, this involves the source, destination IP addresses, and ports in the header, but not the contents of the packet.
  - Based on the content, smart load balancing is not possible.
  - Fastest.
- L7 - Physical Layer :
  - This can involve contents of the header, message, and cookies.
  - Can route request based on content type.
- DNS - Included with in DNS server.( Route 53 )

##### Benefits :

1. Lowest Latancy
2. No Single point of failure
3. Distributes tthe Load on servers.

##### Load Balancer Alogorithms :

1. Round Robin : Follows round robin algo.
2. Wightes Round Robin : Add weights to servers and distribute load.
3. Ip Hash : hash the Ip address and route request to the server.

## 6.Reverse Proxy/Forward Proxy
