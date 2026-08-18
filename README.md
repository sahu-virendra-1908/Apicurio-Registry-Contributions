# Pull Requests & Issues — Apicurio Registry

## Contributor

Virendra Sahu
GitHub: https://github.com/sahu-virendra-1908

## Project Repository

https://github.com/Apicurio/apicurio-registry

## Pull Request Contributions

https://github.com/Apicurio/apicurio-registry/pulls?q=is%3Apr+author%3Asahu-virendra-1908

## Issue Contributions

https://github.com/Apicurio/apicurio-registry/issues?q=author%3Asahu-virendra-1908

---

# Pull Requests Under Review

| # | Pull Request Title                                                        | Status                  | Category                           | Pull Request Link                                       |
| - | ------------------------------------------------------------------------- | ----------------------- | ---------------------------------- | ------------------------------------------------------- |
| 1 | fix(kafkasql): fail when response timeout expires                         | completed  | KafkaSQL / Timeout Handling        | https://github.com/Apicurio/apicurio-registry/pull/9578 |
| 2 | fix(gitops): load validation tasks from disk across replicas              | Draft / Under Review 🔄 | GitOps / Multi-Replica Reliability | https://github.com/Apicurio/apicurio-registry/pull/9624 |
| 3 | Read-only mode does not properly intercept usage-event storage operations | Draft / Under Review 🔄 | Storage / Read-Only Mode           | https://github.com/Apicurio/apicurio-registry/pull/9598 |


---

# Issue Contributions

| # | Issue Title                                                                                                          | Status  | Category                                   | Issue Link                                                |
| - | -------------------------------------------------------------------------------------------------------------------- | ------- | ------------------------------------------ | --------------------------------------------------------- |
| 1 | fix(gitops): validate checkoutPath to prevent path traversal during dry-run validation                               | Open 🟡 | GitOps / Security / Path Validation        | https://github.com/Apicurio/apicurio-registry/issues/9625 |
| 2 | fix(gitops): validation task lookup fails across replicas                                                            | Open 🟡 | GitOps / Distributed Systems / Storage     | https://github.com/Apicurio/apicurio-registry/issues/9600 |
| 3 | Read-only mode does not properly intercept usage-event storage operations                                            | Open 🟡 | Storage / Read-Only Mode                   | https://github.com/Apicurio/apicurio-registry/issues/9580 |
| 4 | KafkaSQL waitForResponse silently returns null when response timeout expires                                         | Open 🟡 | KafkaSQL / Error Handling / Reliability    | https://github.com/Apicurio/apicurio-registry/issues/9574 |
| 5 | Scheduled orphan-content cleanup can delete content during artifact/version creation, causing foreign-key violations | Open 🟡 | SQL Storage / Concurrency / Data Integrity | https://github.com/Apicurio/apicurio-registry/issues/9571 |

---

# Contribution Details

## GitOps Multi-Replica Reliability

### Issue #9600

Identified a distributed-state problem where GitOps validation tasks stored only in a local `ConcurrentHashMap` could return HTTP 404 when subsequent requests were routed to another Registry replica.

### PR #9624

Implemented disk-based fallback for validation task lookup. When a task is missing from the local cache, the Registry loads the task from the shared validation volume and repopulates the cache.

Testing reported in the PR includes **7 GitOps validation tests passed** and `git diff --check` passed.

---

## Read-Only Storage Reliability

### Issue #9580

Identified missing interception for `recordUsageEvent()` and `deleteOldUsageEvents()` in `ReadOnlyRegistryStorageDecorator`, causing an unexpected `UnreachableCodeException` instead of the normal `ReadOnlyStorageException`.

### PR #9598

Added both missing decorator methods and updated `ReadOnlyRegistryStorageTest` to verify that the operations are treated as write operations in read-only mode.

Reported verification:

* Maven compilation passed
* Read-only storage test passed
* Checkstyle passed with 0 violations
* `git diff --check` passed

---

## KafkaSQL Timeout Reliability

### Issue #9574

Identified that `KafkaSqlCoordinator.waitForResponse(UUID)` ignored the boolean result of `CountDownLatch.await()`. When the response timed out, the method could return ordinary `null` instead of reporting a timeout.

### PR #9578

Changed the implementation to detect the timeout and throw a `RegistryException`, and added cleanup of the response entry in the `finally` block.

A review also prompted a dedicated timeout unit test, which was subsequently added to `KafkaSqlCoordinatorTest`.

---

## GitOps Security

### Issue #9625

Reported a path traversal risk in GitOps dry-run validation where `checkoutPath` from the sidecar status file could escape the expected task directory through `..` segments or absolute paths.

The proposed mitigation is to normalize the task directory and resolved checkout path and verify containment before opening the repository.

---

## SQL Storage Data Integrity

### Issue #9571

Identified a race condition between SQL orphan-content cleanup and artifact/version creation. The cleanup job could delete newly committed content before the corresponding `versions` row was inserted, potentially causing a foreign-key constraint violation.

---

# Contribution Areas

| Area                | Work                                                                                  |
| ------------------- | ------------------------------------------------------------------------------------- |
| GitOps              | Multi-replica task recovery, filesystem validation                                    |
| Distributed Systems | Shared-state recovery across Registry replicas                                        |
| Storage             | SQL, KafkaSQL, read-only storage behavior                                             |
| Reliability         | Timeout handling, race-condition analysis                                             |
| Security            | Path traversal / filesystem boundary validation                                       |
| Data Integrity      | Orphan cleanup and transaction-race analysis                                          |
| Testing             | Regression tests, unit tests, Checkstyle and diff validation                          |
| Open Source         | Issue investigation, root-cause analysis, fixes, testing and maintainer collaboration |

---

# Verification

## GitHub Profile

https://github.com/sahu-virendra-1908

## Main Repository

https://github.com/Apicurio/apicurio-registry

---

# Project

| Name              | Link                                          |
| ----------------- | --------------------------------------------- |
| Apicurio Registry | https://github.com/Apicurio/apicurio-registry |
