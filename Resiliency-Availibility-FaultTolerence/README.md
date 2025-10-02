
## Fault Tolerance
System continues to operate even if one part completely fails.

* If the entire Mumbai data center goes down,Traffic from Mumbai users is automatically redirected to Delhi or Bangalore.

## High Availability
Multiple redundant components ensure service is always available.

* If the primary server/database in Mumbai crashes, a standby replica in the same region takes over immediately. 

## Resiliency
System can not only withstand failure but also recover automatically.

* Over time, Mumbai DC becomes healthy again and traffic is rebalanced back.

---
## Core Resiliency Patterns
* Retries with backoff + jitter – retry transient failures smartly.
* Circuit Breaker – stop hammering failing dependencies.
* Timeouts – prevent infinite waits.
* Bulkhead pattern – isolate resources so one failure doesn’t sink everything.
* Fallbacks – provide alternative responses (cached data, defaults).
* Idempotency – ensure retries don’t corrupt state.
* Chaos Engineering – inject failures to test resiliency.

## Core Availability Patterns
* Redundancy / Replication – multiple service instances, multi-AZ, multi-region.
* Stateless Services – scale horizontally, sessions in shared cache.
* Auto-Scaling – dynamically add/remove instances.
* Zero-downtime Deployments – rolling / blue-green / canary releases.
* Caching – reduce dependency load and latency (CDN, Redis).
* Failover (Active-Passive / Active-Active) – reroute to healthy nodes when one fails.

## Core Fault Tolerence Patterns
* Replication with Consensus – majority voting (quorum reads/writes, Raft, Paxos, ZAB).
* Data Checksums & Self-healing Storage – detect and recover from corruption.
* Leader Election – if leader fails, elect a new one automatically.
* Hot Standby / Cold Standby Systems – pre-provisioned nodes ready to take over.
* Message Queues / Event Sourcing – buffer requests so system continues even if consumers fail.