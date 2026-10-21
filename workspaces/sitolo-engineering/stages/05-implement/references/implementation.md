# Implementation Reference

Preserve:

- the accepted phase-part write set and ownership map;
- implementation and behavior tests inside the package/app that owns the capability;
- root-level tests only for true cross-package/system behavior;
- the rule that a missing or unclear owner blocks the change and returns it to planning rather than permitting misplaced code;

- dependency direction;
- tenant and branch authorization;
- explicit domain transitions;
- authoritative PostgreSQL state;
- history and compensating-operation semantics;
- bounded resources;
- idempotency for retryable mutations;
- external calls outside long database transactions;
- real negative tests for security boundaries.

Prefer existing correct abstractions. Do not introduce an unrelated refactor while implementing a bounded change.
