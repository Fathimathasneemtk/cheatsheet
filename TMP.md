

## 2. Performance & Scalability
Optimizing for high traffic and low latency.

### Traffic & Infrastructure
* **Load Balancing:**
    * **Hardware:** F5, Citrix (L4 - Transport Layer).
    * **Software:** Nginx, HAProxy (L7 - Application Layer, allows URL-based routing).
* **Server Types:**
    * **Shared:** Multiple users/apps on one CPU.
    * **Dedicated:** High-performance tasks; no "noisy neighbor" effect.

### Database Optimization
* **Query Speed:**
    * **Indexing:** Use B-Trees for equality/ranges; Hash indexes for exact matches.
    * **Clustering:** Physical sorting of data on disk. Essential for range-based queries.
    * **Sharding:** Horizontal partitioning. Split data across multiple nodes based on a "Shard Key" (e.g., UserID).
* **Replication:**
    * **Master-Slave:** Master handles Writes; Slaves handle Reads. Improves read throughput and provides redundancy.

### Caching Strategies
* **Levels:** SQL Result Cache -> Redis/Memcached (App level) -> Nginx/Varnish (Proxy level) -> CDN (Edge level).
* **Caching Policy:**
    * **Write-Through:** Write to DB and Cache simultaneously. (Consistency: High, Latency: Higher).
    * **Write-Back (Write-Behind):** Write to cache first, async update DB. (Latency: Low, Risk: Data loss if cache crashes).
    * **Cache Aside:** App checks cache; if miss, loads from DB and updates cache.
* **Headers:** Use `E-Tag` (hash of content) and `Last-Modified`. If content hasn't changed, server returns `304 Not Modified`.

### Network & Assets
* **HTTP/2 & HTTP/3:** Multiplexing (many requests over one connection), Header compression (HPACK), and Server Push. HTTP/3 uses QUIC (UDP-based) to solve Head-of-Line blocking.
* **CDN:** Serve static assets (JS, CSS, Images) from the nearest geographical edge location.
* **Asset Optimization:** Use WebP for images, Gzip/Brotli for text compression, and "Responsive Images" (srcset) to serve correct sizes for mobile vs. desktop.

### Logging & Monitoring
* **Optimization:** Avoid logging massive JSON blobs. Log only metadata and error traces.
* **Batching:** Buffer logs in memory and write to disk/logging service in chunks to reduce I/O overhead.
* **Offloading:** Use a dedicated Logging Service (ELK Stack - Elasticsearch, Logstash, Kibana) so logging doesn't slow down the main app.

---

## 3. Deployment & DevOps
Strategies for releasing software with zero downtime.

* **Blue-Green Deployment:** Two identical environments. Blue is live; Green is the new version. Switch traffic to Green via Load Balancer. Easy rollback.
* **Canary Deployment:** Release to a small subset of users (5%) first. Monitor metrics, then roll out to 100%.
* **Rolling Update:** Update instances one by one in a cluster (Standard in Kubernetes).
* **CI/CD:** Continuous Integration (automated tests on push) and Continuous Deployment (automated release to prod).

---

## 4. Methodologies
Frameworks for managing the development lifecycle.

* **Agile:** Iterative development, constant feedback, and flexibility.
* **Waterfall:** Linear, sequential phases (Requirements -> Design -> Build -> Test). High risk if requirements change.
* **Extreme Programming (XP):** Focuses on technical excellence. Practices: Pair Programming, Test-Driven Development (TDD), and Continuous Integration.

---

## 5. Principles
Foundation of clean, maintainable code.

### SOLID Principles
1.  **S - Single Responsibility:** A class should have one reason to change.
2.  **O - Open/Closed:** Open for extension, closed for modification.
3.  **L - Liskov Substitution:** Subtypes must be substitutable for their base types.
4.  **I - Interface Segregation:** Don't force clients to depend on methods they don't use.
5.  **D - Dependency Inversion:** Depend on abstractions, not concretions.

### Other Vital Principles:
* **DRY (Don't Repeat Yourself):** Reduce repetition of patterns.
* **KISS (Keep It Simple, Stupid):** Avoid over-engineering.
* **YAGNI (You Ain't Gonna Need It):** Don't implement functionality until necessary.

---

## 6. Design Patterns
Reusable solutions to common software problems.

* **Producer-Consumer:** Decouples the work-generation from the work-processing. Often implemented via Message Queues (RabbitMQ/Kafka).
* **Service Locator:** Provides a global point of access to services without coupling the user to the concrete class. (Note: Often considered an anti-pattern compared to **Dependency Injection**).
* **Circuit Breaker:** Prevents an application from repeatedly trying to execute an operation that's likely to fail (e.g., a failing microservice).
* **Observer Pattern:** A one-to-many dependency where objects are notified of state changes (Pub/Sub).
* **Strategy Pattern:** Define a family of algorithms and make them interchangeable at runtime.

---

## 7. Vital "Must-Knows" for System Design Interviews
If you are aiming for senior or backend roles, be ready for these:

* **CAP Theorem:** You can only have 2 of 3: Consistency, Availability, Partition Tolerance.
* **ACID vs. BASE:** * **ACID:** (Relational DBs) Atomicity, Consistency, Isolation, Durability.
    * **BASE:** (NoSQL) Basically Available, Soft state, Eventual consistency.
* **Message Queues:** When to use them? (Async processing, spiking traffic, decoupling services).
* **Rate Limiting:** Protecting your API from abuse (Token Bucket, Leaky Bucket, Fixed Window).
* **Microservices vs. Monolith:** Trade-offs in complexity, deployment, and data consistency.