# Microservive design patterns

### API Gateway Pattern
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
* Pro
    - Loose Coupling (Each service can have thier own database and we can have distributed transactions)
    - Avoid two phase commits
    - Supports both Asyc and Sync operation by variations
    - Scalable and Fault-Tolerant (if one service fails, compensating transactions can rollback)
* Cons
    - Eventual consistency
    - Harder to test e2e since events are aysnc
    - longer execution time than single db transactions
    - Tradeof
        - Choreography - event spaghetti
        - Orchestration - centrail corination dependency
* When to use 
    - When each service needs its own database and eventual consistency is ok
---
### Circuit Breaker Pattern
This patterns prevent service cascading failures to other service.It monitors for failures it crossed threshold it goes to half open then to closed state and vice versa.It send error message or fallback response on half and open state.
It has three states
* Closed state(Monitoring for errors)
* Half state(allow only few request to check service health)
* Closed state(complete block and wait for counter reset time to get back to half and then to closed state)

* Pros
    - Avoid cascading failures, helped with fault tolerance
    - Allows recovery via half open state and helps resilency
* Cons
    - Risk of false positive
    - Added compleixty for monitoring failures and maintaining state
* When to use
    - Dealing with external apis

---
### Retry Pattern
This patterns is designed to deal with transient failures, it involves retrying operations instead of immediete failure.

* Variation
    - Fixed delay retry
    - Exponential backoff ✅ (most common)
        - first attempt after 1sec -> 2sec -> 3sec (can cause retry storm)
    - Exponential backoff + jitter (recommended at scale)
        - first atempt after some random time for different request
* Pros
    - Increase resilency against transient errors
    - Auto success with manual intervention
* Cons
    - Can amplify load on struggling service with retry storm
    - Can cause infinte retries so must be combined with circuit breaker
* When to use
    - Use for transient error only(Ocassional 5xx),Should not be used for permananet errors
    - Should be combined with idempotent api to reduce transient errors

---
### Event Sourcing Pattern
Instead of storing just the current state of data (like in a typical database row), you store every change (event) that happened to the data.
* Pros
    - Auditability & Traceability
    - Rebuild State Anytime
    - Naturally fits with CQRS and event-driven microservices.
* Cons
    - Eventual consistency
    - Increased Complexity(Storing events, replaying them)
    - Event Versioning Problems
* When to use
    - When replaying events makes senses like audit logs

---
### CQRS Pattern(Command Query Responsibility Segregation)
CQRS separates writes (commands) from reads (queries) in a system using seperate models for both of them.
* Command side - handle create,update,delete (returns nothing)
* Read side - handle read queries(return data)
Using event sourcing for every write succeded event via event bus goes to read copy and change read model, it will be eventual consistent

* Pros
    - Read write can be scaled independently
    - Both read and write model can be optimised for different structures
* Cons
    - Eventual consistent
    - Complexity oeverkills if scale not needed
    - Race conditions are possible if read excepted immediete after write
* When to use
    - System have high read and write volumes immbalance
    - Need event replay and auditing
    - Denormalised reads needed


---
### Sidecar Pattern
The Sidecar pattern involves attaching a helper service (the sidecar) to the main microservice to provide additional functionalities such as logging, security, or communication with external services.
* Pros
    - Separation of Concerns (logging, monitorinh, circuit breaker, ratelimiting etc abstracted out)
    - App language independent (Runs of different container)
    - Loosely coupled
    - Standardization 
* Cons
    - Coupled at deployment, if sidecar fails can impact service
    - Operational complexities and extra resources needed
* When to use
    - You need cross-cutting features (logging, monitoring, service discovery, proxy, config) but don’t want to embed them in app code.

---
### Bulkhead Pattern
The Bulkhead pattern involves separating components or services to contain failures.Like divinding resources to difference service to reduce blast radius on outages.

* Pro
    - Fault Isolation
    - Improved Resilience
    - Better Resource Utilization
* Con
    - Resource Fragmentation(if strict resources partitions, it can not be utilised when needed for other)
    - Latecncy as working restricted resources
