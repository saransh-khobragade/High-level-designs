* Requirement
    - QPS(Query per second) (1000 QPS)
    - read/write ratio (80 % read)
    - latency SLO (p95 <100ms)

* High level arch
    - Clients → Load Balancer / API Gateway (TLS, rate-limit, auth).
    - Stateless app servers (horizontal scale, containerized).
    - Caching layer (Redis/Memcached) for hot reads.
    - Read/Write separation: use read replicas; consider CQRS if reads dominate.
    - Idempotency & retries: design APIs to be idempotent; use unique request IDs.
    - Backpressure & rate-limiting: prevent overload; use circuit-breakers.
    - Async for non-critical work: queue emails, thumbnails, analytics.
    - Message broker (Kafka/Rabbit/SQS) for async work, decoupling, retries.
    - CDN for static content.
    - Background workers for heavy tasks (image processing, notifications).
    - Search/index (Elasticsearch) for full-text queries.
    - Monitoring & tracing (metrics, logs, distributed traces).
    - Primary datastore (SQL or NoSQL) with replicas
        - Sharding / partitioning: partition users/data by user-id or hash when a single node can’t hold data. Plan for re-sharding.
        - Use SQL when transactions and strong consistency matter; NoSQL for high write scale and flexible schema.
    - Security: centralized auth, transport encryption, least privilege for services.

* Operational / delivery practices
    - Immutable infra (IaC), canary/blue-green deploys.
    - CI/CD with automated unit/integration/load tests.
    - Capacity planning + load testing before big releases.

* Observability & SLOs
    - Define SLOs and error budgets.
    - Instrument traces, metrics (latency/p95, error rate), structured logs, and dashboards.
    - Alert on symptoms (latency, queue depth) not downstream noise.

example
“For an image-sharing service: front-end + CDN, API gateway → stateless app (Node/Go) → metadata DB (RDS) + image store (S3). Use Redis for feed caching, Kafka for upload processing and thumbnail generation, workers for heavy jobs, and read replicas for scale. Autoscale app tier on CPU/latency, monitor p95 and queue size, and use canary deployments.”

Q: How will you shard data?
A: “Use user-id hash-based partitioning; keep routing layer aware of shard map; plan for re-sharding using consistent hashing or a mapping service.”

Q: How to handle cache invalidation?
A: “Use write-through or cache aside with TTL for soft invalidation; publish invalidation events on write for immediate consistency where needed.”

Q: How to ensure exactly-once processing?
A: “Use idempotency keys and dedupe in the consumer, or use transactional outbox pattern with at-least-once delivery.”