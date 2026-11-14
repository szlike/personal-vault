# Spring Boot — IoC, Beans, Transactions, and REST

**Recorded:** 2026-11-14  
**Original preparation:** 2026-09-11; revisited 2026-10-29

## Spring core concepts

- **IoC:** container controls object creation/configuration; **dependency injection** supplies required collaborators.
- **Bean:** container-managed object with lifecycle and scope. Most services are singleton by default.
- **`@Component` / `@Service` / `@Repository`:** stereotypes; `@Repository` can also participate in exception translation.
- **Constructor injection:** explicit required dependencies, enables `final` fields and simple unit tests; prefer over hidden field injection for required collaborators.
- **`@SpringBootApplication`:** combines configuration, auto-configuration and component scan conventions.
- **`@Autowired`:** requests dependency resolution; a single constructor generally needs no explicit annotation in modern Spring.

## Transactions

`@Transactional` commonly uses Spring AOP proxy interception. Important pitfalls:

- A call from one method to another transactional method **inside the same object** usually bypasses proxy advice (self-invocation).
- Unchecked exceptions normally trigger rollback by default; checked exceptions may require explicit rollback configuration.
- `save()` and `flush()` are **not the same as commit**; changes can still roll back.
- Isolation level changes visibility of concurrent writes; be able to explain dirty/nonrepeatable/phantom reads and transaction boundaries.
- Calling external APIs inside a long-running DB transaction increases lock/latency risk; prefer outbox/saga pattern when needed.

## Request flow and API design

HTTP request → filter/authentication → controller/validation → service/business logic → repository/transaction → response/exception handler. Use coherent resource naming, meaningful HTTP status, input validation, pagination, backward-compatible changes and correlation IDs.

## Past mistakes and feedback

- **Observed preparation:** Needed crisp definitions of `@Service` vs `@Autowired`, bean creation, and why constructor injection helps.
- **Identified knowledge gap:** Java/Spring internal behavior was harder to recall than React and JavaScript experience.
- **Potential trap:** Thinking annotation presence alone guarantees a transaction when proxy interception is skipped.

## Points to improve

1. Give a 30-second answer: “What does the Spring container do?”
2. Trace a request through controller, service, transactional repository and database.
3. Explain `@Transactional` self-invocation with a tiny code example.
4. Compare a DB transaction with a cross-service workflow and the purpose of outbox.
5. Prepare a REST error-handling example with retries/idempotency.

