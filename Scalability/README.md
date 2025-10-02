# Scalability

### Core Scalability Pattens

* Horizontal Scaling (Scale-Out)
    - Add more instances (servers, containers, pods).
* Vertical Scaling (Scale-Up)
    - Increase resources of a single node (CPU, RAM, Disk, GPU).
* Load Balancing
    - Distribute requests across multiple servers.
* Partitioning / Sharding
    - Split data or workload across multiple nodes.
* Caching
    - Reduce repeated computation and database hits.
* Database Scaling
    - Read replicas → offload read-heavy traffic.
* Asynchronous Processing
    - Use message queues (Kafka, RabbitMQ, SQS) to decouple heavy tasks.
* Auto-Scaling
    - Dynamically add/remove servers based on metrics (CPU, QPS, latency).
* CQRS (Command Query Responsibility Segregation)
    - Separate read path from write path.