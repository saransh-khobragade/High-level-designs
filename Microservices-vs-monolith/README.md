# Monolith vs Microservices

### Monolith

* Pros
    - Simple to develop, test, deploy.
    - Easier local debugging and end-to-end testing.
    - Lower operational overhead early on.
    - Faster to iterate for small teams / early-stage products.

* Cons
    - Hard to scale specific bottlenecks without scaling everything.
    - Large codebase can slow builds and deployments.
    - Risk of tight coupling, brittle changes, and release coordination pain.

* Choose Monolith if:
    - You’re early-stage / MVP and need to iterate fast.
    - Team is small (1–8 engineers).
    - Requirements aren’t clear and you’ll change domain models often.
    - You want low operational overhead.

---
### Microservices

* Pros
    - Independent deploy, independent scale, and language/runtime freedom.
    - Clear service boundaries → easier ownership and focused teams.
    - Fault isolation: failure in one service less likely to take down entire system.
    - Enables polyglot persistence and different scaling/consistency choices per service.

* Cons
    - Much higher operational complexity: CI/CD, service discovery, API gateways, monitoring.
    - Distributed system problems: partial failures, retries, consistency, transactions.
    - More expensive (more infra, more cross-service latency).
    - Requires mature devops and strong observability.

* Choose Microservices if:
    - You need independent scaling for different parts (hot paths).
    - Team size > ~8–12 and you want team autonomy.
    - You need polyglot persistence or different SLAs per domain.
    - You have mature DevOps, monitoring, and clear domain boundaries.