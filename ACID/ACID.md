# ACID (Atomicity + Consistency + Isolation + Durable)

### Atomicity
All commit or nothing, if any transaction fails rollback everything.

### Consistency
Database should remain in valid state before and after transaction following every constraints or rollback.

### Isolation
Changes made by one transaction should not be visible to other transaction before commited.

### Durability
Oncs changes are saved they are permanent, even if system fails they should be recoverable.

