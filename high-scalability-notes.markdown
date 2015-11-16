Notes from posts on the blog [High Scalability](http://highscalability.com/).

### [A 360 Degree View Of The Entire Netflix Stack](http://highscalability.com/blog/2015/11/9/a-360-degree-view-of-the-entire-netflix-stack.html)

* Netflix infrastructure and transcoding is on EC2 with master copies of digital films from movie studios being stored on Amazon S3.
* Open-source software on the backend includes Java, MySQL, Gluster, Apache Tomcat, Hive, Chukwa, Cassandra, and Hadoop.
* The Netflix Open Connect CDN is provided for larger ISPs that have over 100,000 subscribers. It is a low-power, high-storage density appliance caches Netflix content within the ISPs’ data centers to reduce internet transit costs.

### [How Shopify Scales To Handle Flash Sales From Kanye West And The Superbowl](http://highscalability.com/blog/2015/11/2/how-shopify-scales-to-handle-flash-sales-from-kanye-west-and.html)

* Handles about 300 million unique visitors a month, but biggest challenge is from "flash sales".
* Requests received by Nginx and forwarded to a cluster of servers running Docker with the Rails app.
* Data tier includes Memcached, Redis, ElasticSearch, MySQL, Kafka, and Zookeeper.
* Performed load testing to populate a "Resiliency Matrix," which specifies happens to systems when a service (e.g. MySQL, Redis, etc.) goes down.
* Scaling databases is hard, especially sharding. Try to cache a lot before you have to do it. But shards to help isolate incidents.
* Most shortcomings from resilience testing have been fixed with database scaling and automatic failovers.

### [Architecting Backend For A Social Product](http://highscalability.com/blog/2015/7/22/architecting-backend-for-a-social-product.html)

#### Data Storage

* Identify which data is the most frequently queried, and which data is not so frequently required (e.g. can be archived for analysis).
* Data queried at a high frequency should be in a place where it is always available, faster to read and write, and horizontally scalable.
* Can represent connected or relational data using a graph database like Neo4j, FlockDB, AllegroGraph and InfiniteGraph.
* To store data with availability and scalability, consider S3, Google Cloud Storage, or Rackspace Cloud Files. But metadata indexing is a separate concern.

#### Session Data

* Can store this in a key-value store like Redis or a document-based storage solution like MongoDB.

#### Indexing

* To build an index, can write a simple system at point of content ingestion what creates an inverted index. Can graduate to Apache Storm.
* The indexing system itself could be Solr or ElasticSearch, both of which use Lucene.

#### Queuing & Push Notifications

* Events with high fanout can be processed using a queue such as ActiveMQ.

#### Caching Strategy

* The server-side application cache should store data in a format that is ready to render and requires no messaging or further action. This may require denormalization when content is ingested.
* Redis can be treated as an in-memory application cache with sharding, leader-follower node configuration, and is fast for incremental operations to collections.
* We can cache at the reverse proxy level, such as on Nginx. This requires setting HTTP response headers correctly.
* We should cache resources aggressively in the app or in the browser, such as by setting the proper HTTP caching headers, or persisting data in SQLite.

#### Transport Protocols

* Consider using HTTP/2 or SPDY over HTTP/1.1.

#### Components

* The load balancer determines which proxy server the request should be forwarded to based on a decided strategy.
* The proxy server handles all incoming requests. It is an SSL termination point, and will cache HTTP responses based on the defined policy.
* The event processor is responsilble for any events with a fan-out, such as notifications.

### [AppLovin: Marketing To Mobile Consumers Worldwide By Processing 30 Billion Requests A Day](http://highscalability.com/blog/2015/3/9/applovin-marketing-to-mobile-consumers-worldwide-by-processi.html)

#### Technology Stack

##### Data Storage

* MySQL for ad data.
* Spark for offline processing and deep data analysis.
* Redis for basic caching.
* Thrift for all data storage and transfers.

##### Core App And Services

* Jenkins for continuous integration and deployment.
* Zookeeper for distributed locking.
* HAProxy for high availability.
* Checkstyle and PMD for static code analysis.
* Syslog for DC-centralized log server.
* Hibernate for transaction-based services.

##### Servers And Provisioning

* Chef for configuring servers.
* Load balancing is a combination of software like HAProxy and hardware.

#### Monitoring Stack

##### Server Monitoring

* Graphite as data format.
* PagerDuty for issue escalation.

#### Architecture Overview

##### General Considerations

* Store everything in RAM. If it does not fit, save it to SSD.
* Automate everything.
* Zero-copy message passing.

##### Message Processing

* Sending a message means writing to disk.
* Message dispatching system provides isolation and extensibility of the system.

##### Ad Serving

* Nginx is really fast.
* jemalloc gave a 30% speed improvement.
* Torrents propagate serving data across all servers, yielding 83% network load drop on originating server.
* mmap is used to share ad serving data across nginx processes.
* XXHash is the fastest hash function with a low collision rate. 75x faster than SHA-1.

#### Data Warehouse

* Aggregation is key to allow fast reports on large amounts of data.
* 7 days of raw logs is usually enough for debug.
* Backup in S3 for catastrophic failures.
* Raw data is stored in a Spark cluster.


