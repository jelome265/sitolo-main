# Plan Contract

A plan must identify:

- the proposed full phase-part sequence, current part, dependencies and explicit human approval status;
- requirement/control-to-owner mapping: approved crate/app/path, exact source files, exact test files, verification command/evidence, and a reason for every relevant package not touched;
- the current part's exact write set and stop/replanning rule when ownership or scope changes;
- tests inside the package/app that owns the behavior; root-level tests only for genuine cross-package or repository-wide behavior;

- objective and non-goals;
- authority and assumptions;
- bounded contexts and owners;
- invariants and trust boundaries;
- proposed state transitions;
- transaction and concurrency behavior;
- idempotency and external failure handling;
- persistence and migration impact;
- tests and evidence;
- observability;
- rollback or disable path;
- security control ID → threat/requirement → implementation → negative test/evidence → CI/release gate → residual risk/exception mapping when security-relevant;
- unresolved decisions.

The plan specifies what and when. It must not silently settle an open architectural decision. **Implementation is blocked until the human accepts the current part and its write set.** The ownership map is a Phase 0 proposal until accepted; do not use it to resolve its own open package decisions by inference.
