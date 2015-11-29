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

### [Nifty Architecture Tricks From Wix - Building A Publishing Platform At Scale](http://highscalability.com/blog/2014/11/10/nifty-architecture-tricks-from-wix-building-a-publishing-pla.html)

Platform on which over 54 million web sites have been created, most of which receive under 100 page views per day. So traditional caching does not apply.

Novel consequences:

* Does not use transactions. all data is immutable and they use a simple eventual consistency strategy.
* Does not cache. Instead, they pay great attention to optimizing the rendering path so that every page displays in under 100ms.

#### Guidelines For How To Build Services

* Services are stateless. To horizontally scale, just add more servers.
* With the exception of billing/financial transactions, all other services do not use transactions.
* When designing a new service caching is not part of the architecture. If a service performs poorly, first optimize the code before emplying caching.

#### Editor Segment

* MySQL is a great key-value store. Key is based on a hash function of the file so the key is immutable.
* An "active database" contains the 6% of sites that are still being updated. It is really fast and relatively small in terms of storage.
* An "archive database" contains the remaining data that is infrequently accessed. It is relatively slow, but contains huge amounts of storage.

#### High Availability For Editor Segment

* All data is immutable. Revisions are stored for everything. If a corruption can’t be fixed, revert to version where the data was fine.
* To protect against unavailability, data s replicated across different geographical locations and multiple cloud services.

#### Modeling Data With No Database Transactions

* JSON files are inserted into the database, and finally a manifest is inserted with the identifiers of all resources that were just saved.

#### Public Segment

* Public SLA is that response time is < 100ms at peak traffic. Web sites must be fast with no caching.
* Data is stored in a denormalized format, optimized for read by primary key. Everything that is needed is returned in a single request.
* The data returned is in JSON format. Rendering is offloaded to the client.

#### Lessons Learned

* Identify your critical path and concerns.
* De-normalize and precalculate data, and thereby reduce network chattter, for performance.
* Take advantage of client’s CPU power.
* Go immutable. Immutability has far reaching consequences for an architecture, and is an elegant solution to a lot of problems.
* What really locks you down is data. Moving lots of data to a different cloud is really hard.

### [StackOverflow Update: 560M Pageviews A Month, 25 Servers, And It's All About Performance](http://highscalability.com/blog/2014/7/21/stackoverflow-update-560m-pageviews-a-month-25-servers-and-i.html)

* 25 servers for everything. That’s high availability, load balancing, caching, databases, searching, and utility functions.
* Still uses Microsoft products.
* Uses a scale-up strategy. Not using any cloud service, where their SQL Servers loaded with 384 GB of RAM and 2TB of SSD would cost a fortune.
* Jeff Atwood said "Hardware is cheap, programmers are expensive" and this apparently still holds true.

#### Stats

* Peak is 2600-3000 requests/sec on most weekdays.
* Each web server has 2x 320GB SSDs in a RAID 1, and each ElasticSearch box has 300 GB also using SSDs.
* Has a 40:60 read-write ratio.
* 11 web servers using IIS, 2 load balancers using HAProxy, 4 active database nodes using MS SQL, 3 application servers implementing the tag engine, 3 machines running ElasticSearch, 2 machines running Redis for distributed cache and messaging.

#### Platform

* ElasticSearch, Redis, HAProxy, and MS SQL.

#### Servers

* The database server is at 10% usage except during backups. The database servers have 384GB of RAM and the web servers are at 10%-15% CPU usage.
* Scale-up is still working. Other scale-out sites with a similar numbers tend to run on between 100 and 300 servers.

#### SSDs

* ElasticSearch performs much better on SSDs, given SO writes/re-indexes very frequently.

#### High Availability

* The main datacenter is in New York and the backup datacenter is in Oregon.
* Not everything is replicated between data centers (very temporary cache data that's not needed to eat bandwidth by syncing, etc.), but the important resources are.
* Nginx was used for SSL, but a transition has been made to using HAProxy to terminate SSL.

#### Databasing

* Each site has its own database, and the schema for each is the same. This is effectively a form of partitioning and horizontal scaling.
* All the schema changes are applied to all site databases at the same time.
* Partitioning is not required. Indexing takes care of everything and the data just is not large enough.
* Scores are denormalized, so querying is often needed.
* The Tag Engine is entirely self-contained, and runs on three servers for redundancy. If all of them fail, the local web servers will load the tag engine in memory and keep on going.

#### Coding

* Compilation is very fast.
* New features are hidden via feature switches.
* Heavy usage of static classes and methods, for simplicity and better performance.

#### Caching

* Network level cache is caching in the browser, CDN, and proxies.
* Redis works as a distributed in-memory key-value store for all servers that serve the same site.
* SQL Server Cache caches the entire database in memory. 
* SSD is used as a cache, and is usually only hit when the SQL server cache is warming up.
* TODO


