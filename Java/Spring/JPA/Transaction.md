## `@Transactional`
## Why should put class level `@Transactional(readOnly = true)` #JavaGoodPractice
Here:
- all methods default to `readOnly = true`
- only methods explicitly annotated with `@Transactional` open a full read/write transaction

This is often cleaner because:
- 80% of service methods in many apps are “read-only” queries.
- You don’t repeat `@Transactional(readOnly = true)` everywhere

