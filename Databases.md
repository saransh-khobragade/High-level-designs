# Cap Thorem

* Consistency + Availability
    - Traditional RDBMS (single-node, not distributed):
        - PostgreSQL 
        - MySQL
        - Oracle DB
        - SQL Server
* Availability + Partition Tolerance
    - Prioritize availability, may sacrifice strict consistency (eventual consistency instead).
        - Cassandra (Tables with rows & columns, but schema is flexible)
            - Massive write-heavy apps: IoT telemetry, time-series, logs, messaging platforms.
            - When you need multi-region replication and low-latency at scale.
        - DynamoDB (Tables with partition keys + sort keys, items are JSON-like documents.)
            - Cloud-first serverless apps.
            - When you want zero ops and are okay living inside AWS.
* Consistency + Partition Tolerance
    - Prioritize correctness (strong consistency), may sacrifice availability under partition.
        - HBase (built on Hadoop/HDFS)
        - MongoDB (in some modes, e.g. write concern majority)
        - Zookeeper (coordination service, ensures strict consistency)
        - Etcd (used in Kubernetes, consistent key-value store)
        - Etcd (used in Kubernetes, consistent key-value store)