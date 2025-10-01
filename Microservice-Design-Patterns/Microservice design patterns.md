# Microservice design patterns — concise, interview-ready guide

Nice — below is a compact, organized reference of the **most useful microservice design patterns**, when to use them, trade-offs, and quick implementation notes. Use this in interviews or as a checklist while designing systems.

---

# 1. API Gateway / Edge Patterns

**What:** Single frontend for clients that routes, authenticates, rate-limits, and transforms requests to backend services.
**When:** Multiple services expose APIs to clients; you need a single public surface.
**Trade-offs:** Simplifies client logic but becomes a single point to scale/secure.
**Notes:** Combine with BFF (Backend-for-Frontend) when multiple client types need different APIs.

---

# 2. Backend-for-Frontend (BFF)

**What:** Per-client (mobile, web) gateway that adapts backend responses to client needs.
**When:** Different clients need different payloads/latency guarantees.
**Trade-offs:** Extra maintenance per client type, better UX and performance.

---

# 3. Service Discovery

**What:** Dynamic registration + discovery (DNS/registry like Consul/Eureka).
**When:** Services scale up/down or use ephemeral instances (containers).
**Trade-offs:** Adds operational components; avoid hard-coded addresses.

---

# 4. Circuit Breaker

**What:** Prevents cascading failures by short-circuiting calls to unhealthy services.
**When:** Calling potentially unreliable downstream services.
**Trade-offs:** Must tune thresholds; false trips can reduce availability.
**Notes:** Pair with retries + exponential backoff.

---

# 5. Bulkhead

**What:** Isolate resources (thread pools, connection pools) per service/function to limit blast radius.
**When:** High-value paths must be protected from noisy neighbors.
**Trade-offs:** May reduce overall resource utilization but increases stability.

---

# 6. Retry + Exponential Backoff + Idempotency

**What:** Retries transient failures with backoff; design endpoints to be idempotent.
**When:** Network calls that occasionally fail.
**Trade-offs:** Retries increase load; ensure idempotency or use dedupe keys (request IDs).

---

# 7. Circuit-breaker + Bulkhead + Retry (Resilience Triad)

**What:** Combine patterns above for resilient remote calls.
**When:** Building robust inter-service communication.

---

# 8. Database per Service / Polyglot Persistence

**What:** Each service owns its schema and DB technology.
**When:** Services have different data/consistency needs.
**Trade-offs:** Data duplication and cross-service transactions are harder; promotes autonomy.

---

# 9. Saga (Choreography or Orchestration)

**What:** Manage distributed transactions via a sequence of compensating actions.
**When:** You need eventual consistency across services (e.g., order + payment + inventory).
**Trade-offs:** Complex to reason about and test; compensate logic required.

---

# 10. Command Query Responsibility Segregation (CQRS)

**What:** Separate read models from write models; different stores/optimizations.
**When:** Heavy read/write asymmetry or complex read requirements.
**Trade-offs:** Increased complexity, eventual consistency between models.

---

# 11. Event Sourcing

**What:** Persist state as an ordered stream of events, derive current state by replaying events.
**When:** Auditability, temporal queries, or complex state transitions matter.
**Trade-offs:** Operationally and conceptually complex; event schema evolution matters.

---

# 12. Publish/Subscribe & Event-driven Architecture

**What:** Services communicate via events on a message bus (Kafka, RabbitMQ).
**When:** Loose coupling, scalability, async communication, stream processing.
**Trade-offs:** Harder debugging and causal tracing; eventual consistency.

---

# 13. Outbox Pattern

**What:** Ensure atomicity between DB write and produced event by writing event to an outbox table in same transaction, consumed later.
**When:** When you publish domain events reliably while updating service DB.
**Trade-offs:** Extra component to read and publish outbox.

---

# 14. Strangler Fig / Incremental Migration

**What:** Gradually replace parts of a monolith by routing specific functionality to new services.
**When:** Migrating a monolith to microservices.
**Trade-offs:** Requires careful routing and versioning; easier incremental migration.

---

# 15. Sidecar & Ambassador

**What (Sidecar):** Co-located helper process for a service (e.g., logging, proxy, service mesh sidecar).
**What (Ambassador):** Proxy that exposes service features to the outside.
**When:** Observability, service mesh, language-agnostic helpers.
**Trade-offs:** Adds complexity and resource overhead per instance.

---

# 16. API Versioning / Backward Compatibility

**What:** Maintain multiple API versions, use contract testing.
**When:** Public APIs or multiple clients requiring different schemas.
**Trade-offs:** Maintenance overhead; careful deprecation policy needed.

---

# 17. Rate Limiting & Throttling

**What:** Protect services from abuse/overload at gateway or service level.
**When:** Entry points subject to spikes or malicious traffic.
**Trade-offs:** Might deny legitimate traffic during bursts; should be combined with graceful degradation.

---

# 18. Caching Patterns (Cache-Aside, Read-Through, Write-Through)

**What:** Reduce load/latency for hot reads.
**When:** Read-heavy workloads or expensive computations.
**Trade-offs:** Invalidation complexity; stale reads — choose TTLs and invalidation carefully.

---

# 19. CDN for Static Content

**What:** Offload static assets to CDN.
**When:** Serving images, JS/CSS, large assets to global users.
**Trade-offs:** Cache invalidation and edge consistency considerations.

---

# 20. Anti-Corruption Layer

**What:** A translation layer to integrate with legacy or external systems without leaking their models.
**When:** Integrating external or legacy domains into your microservices.
**Trade-offs:** Additional code but prevents model pollution.

---

# 21. Consumer-Driven Contract Testing

**What:** Tests that consumers define expectations of providers (e.g., Pact).
**When:** Many teams independently deploy services that interact.
**Trade-offs:** Extra test infra but prevents breaking changes and reduces integration risk.

---

# 22. Observability Patterns (Tracing, Metrics, Structured Logs)

**What:** Instrument with distributed tracing, metrics, logs, and health endpoints.
**When:** Always — critical for operating microservices.
**Trade-offs:** Instrumentation cost but indispensable for debugging and SRE.

---

# 23. Dead Letter Queue (DLQ)

**What:** Capture failed messages after retries for manual inspection/repair.
**When:** Message processing may permanently fail for certain inputs.
**Trade-offs:** Requires monitoring and remediation processes.

---

# 24. Feature Flags & Canary Releases

**What:** Gradually roll out features; enable quick rollback.
**When:** Deploying risky changes or experimenting.
**Trade-offs:** Complexity to manage flags and cleanup technical debt.

---

# 25. Immutable Infrastructure & GitOps

**What:** Deploy immutable artifacts, infra as code, and automated pipelines.
**When:** Scaling teams and environments reliably.
**Trade-offs:** Requires investment in CI/CD and infra automation.

---

# Quick interview-ready summary (30–45s)

“Microservice patterns solve distributed-system problems: API Gateway/BFF for client-facing surfaces; Service Discovery, Circuit Breaker, and Bulkhead for resilient communication; Database-per-service, Saga, CQRS, and Outbox for data consistency; Event-driven + Pub/Sub for loose coupling; and Sidecar/Service Mesh, Observability, and Contract Tests for operations. Choose patterns based on the scale, team structure, and SLOs — start simple, add patterns where they solve real pain.”

---

If you want, I can:

* give **one-line code/stack examples** for 6–8 key patterns (e.g., Circuit Breaker with Hystrix/resilience4j),
* sketch an **architecture** using 6 of these patterns for a concrete app (chat, e-commerce, image service), or
* prepare **5 common interview questions + concise answers** about microservice patterns. Which next?

---
# Microservive design patterns
---
### API Gateway
Single endpoints for client aggregating multiple backend services

* Pro
    - centralised authentication,authorization(JWT, OAuth2), logging ,monitoring
    - versioning support
* Cons
    - Single point of failure
    - Performance bottle, if overload it can throttle entire system
---
### Saga Pattern
This pattren is used for maintaining distributed transactions
Each micro-service involved in saga handles its own transaction and publishes event to trigger subsequent actions

* Two variations
    - Choreography(event based) - servives listen and react to events
    - Orchestration (central controller) - one service cordinates the saga transactions
* Cons
    - Eventual consistency
    - Harder to test e2e since events are aysnc
    - longer execution time than single db transactions
* When to use 
