# The Gang of Four and the Pattern Singularity

> A field study in design patterns, conducted at industrial scale,
> on a workload that prints a greeting.
>
> Русская версия: [GANG_OF_FOUR_ru.md](GANG_OF_FOUR_ru.md)

---

## 1. Who Is the Gang of Four

In 1994, four engineers — Erich Gamma, Richard Helm, Ralph Johnson, and
John Vlissides — published _Design Patterns: Elements of Reusable
Object-Oriented Software_. Its examples use C++ and Smalltalk; the book
predates TypeScript by a comfortable margin. It describes recurring
solutions to design problems. This project appears to have interpreted
"recurring" as a requirement to establish the same department 104 times.
The greeting has acquired an organizational chart.

The book catalogs **23 patterns** in three families:

- **Creational** (5): how to make objects without saying `new` out loud.
- **Structural** (7): how to wrap one thing in another thing.
- **Behavioral** (11): how to make objects talk without coupling them.

For decades, this book has been the industry's shared vocabulary —
and, on occasion, its shared delusion: that a problem is solved once it
has been _named_. This project takes the delusion to its logical
conclusion.

---

## 2. The 23 Patterns According to the Staffing Plan

The staffing plan includes all 23 positions from the catalogue.
Below are their assigned duties and the acceptance committee's findings.
Directory nameplates and working mechanisms are inspected separately.

### Creational

| Pattern              | Where it lives here                                                                                                                                                                                                                                                        |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Factory Method**   | `factories/` has established branches throughout the repository. `EventFactory.create()` uses static construction; a subclass creation hook is absent from this example. The factory is registered. Its manufacturing licence is still with the committee.                 |
| **Abstract Factory** | [`RateLimitingRedisAdapterFactory.createWithDefaults()`](src/layers/rate-limiting/adapters/RedisAdapter.ts) manufactures one concrete adapter. Families of related products have not reached the shop floor. The organizational chart is more abstract than the inventory. |
| **Builder**          | `EventConfigBuilder`, `SerializerBuilder`, `ControllerConfigBuilder` — because constructing a config object in one statement would be reckless.                                                                                                                            |
| **Prototype**        | [`HelloWorldMessage.withLanguage()` and `.withFormat()`](src/layers/domain/entities/HelloWorldMessage.ts) copy the entity and preserve its version. Changing the language of a greeting now leaves a paper trail. International expansion is under control.                |
| **Singleton**        | [`MetricsRegistry.getInstance()`](src/layers/serialization/metrics/MetricsRegistry.ts) has a private constructor and a cached instance. The remaining registries await their certificates of uniqueness.                                                                   |

### Structural

| Pattern       | Where it lives here                                                                                                                                                                                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Adapter**   | `adapters/` everywhere: `InMemoryAdapter`, `RedisAdapter`, `PostgresAdapter`, `ConsulLikeAdapter`, `EtcdLikeAdapter`. The greeting can now change databases more easily than departments.                                                                                    |
| **Bridge**    | `I*Strategy` provides interchangeable algorithms. The second independently varying hierarchy has not been supplied with this example. The bridge has a name, a budget, and a committee; the structural drawings are outstanding.                                             |
| **Composite** | Rule composition in `waf` (AND/OR/NOT rule trees), `fraud-detection` (composable rules), `rules-engine` (nested conditions). A rule that contains rules that contain rules.                                                                                                  |
| **Decorator** | An entire layer named after it: `LoggingDecorator`, `CachingDecorator`, `TimingDecorator`, `ValidationDecorator`.                                                                                                                                                            |
| **Facade**    | Also an entire layer. Plus `GitOpsEngine`, `IdentityEngine`, `SecretsEngine` — facades all the way down.                                                                                                                                                                     |
| **Flyweight** | [`ColumnStorageEngine`](src/layers/data-warehouse/engine/ColumnStorageEngine.ts) shares dictionary values; `backup` deduplicates blocks. Shared data has been entered in the register. Approval of the full Flyweight object structure remains with the relevant department. |
| **Proxy**     | Also an entire layer: virtual, protection, caching, remote, and smart-reference proxies. Five flavors of standing between you and the thing.                                                                                                                                 |

### Behavioral

| Pattern                     | Where it lives here                                                                                                                                                                                                                                                     |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Chain of Responsibility** | Middleware pipelines; the `CommandBus` onion — a request passes through handlers until one of them finally admits it is a greeting.                                                                                                                                     |
| **Command**                 | CQRS command objects with handlers, undo-capable operations in `unit-of-work`.                                                                                                                                                                                          |
| **Interpreter**             | GraphQL, SQL lineage, ICU MessageFormat, and cron have separate language-processing machinery. The GoF Interpreter object structure requires its own inspection. One grammar per department has already been approved.                                                  |
| **Iterator**                | [`ReadQueryEngine`](src/layers/read-model/implementations/QueryEngine.ts) issues pages through `offset`/`limit`. No cursors were found in this engine. The separate Iterator abstraction is awaiting inspection; applications for the next page are processed in order. |
| **Mediator**                | `message-bus` delivers messages between publishers and subscribers. Coordination of their interactions remains a separate responsibility. All departments can now shout into the void through an approved interface.                                                    |
| **Memento**                 | Snapshots in `event-sourcing`, savepoints in `unit-of-work`, snapshot mementos in `write-model` aggregates.                                                                                                                                                             |
| **Observer**                | Event bus, `WatchNotifier`, domain event propagation, `observers/` in every domain folder.                                                                                                                                                                              |
| **State**                   | Failover, outbox, account lifecycle, BPMN, and canary controllers maintain state machines. Delegation to state objects is inspected separately. A transition toward brevity has not been included in the transition table.                                              |
| **Strategy**                | [`StrategyStrategyFactory`](src/layers/strategy/factories/StrategyStrategyFactory.ts) creates strategies for the strategy layer. Selecting an algorithm now has its own algorithm-selection department. Additional headcount has been requested.                        |
| **Template Method**         | [`BaseLogger._log()`](src/layers/serialization/logging/BaseLogger.ts) checks the level, creates an entry, then calls the abstract `_write()` hook. The skeleton is in the base class; the greeting's soul awaits procurement approval.                                  |
| **Visitor**                 | GraphQL and lineage code walk syntax trees. Visitor dispatch over node types has not been established by that observation. All visitors have nevertheless been registered at reception.                                                                                 |

---

## 3. The Math of Pattern Density

The Gang of Four needed 23 patterns to describe reusable object-oriented
software. This project needed:

| Measure                             | Count                                                             |
| ----------------------------------- | ----------------------------------------------------------------- |
| Catalogue entries discussed         | 23 positions in the staffing plan                                 |
| Architectural layers in the project | 104 departments; pattern allocation requires a separate inventory |
| Audited pattern instantiations      | Inventory pending; the counting committee has not been staffed    |
| Patterns _not_ in the book          | see below                                                         |

The book's patterns turned out to be merely the **starter pack**. This
repository also implements the enterprise patterns the Gang never got
around to, because in 1994 nobody had yet suffered enough:

- **Repository, Unit of Work, Specification** — all three have dedicated layers. Even the selection criteria have office space.
- **CQRS, Event Sourcing, Saga, Outbox, Inbox** — five more layers.
- **Circuit Breaker, Retry, Throttling, Feature Flag** — resilience as a lifestyle.
- **Sidecar, Service Mesh, API Gateway** — patterns about patterns about deployment.
- **Anti-Corruption Layer** — implementation not located. Interdepartmental apologies are currently handled manually.

When a codebase has more pattern-_shaped_ directories than the book has
patterns, the book stops being a reference and becomes a seed. This
repository is what grew.

---

## 4. What the Gang Would Say

Minutes of an imaginary review meeting:

- **Gamma** (Erich): "That static factory makes one adapter. Your
  organizational chart is more abstract than your factory."
- **Helm** (Richard): "You made the _Facade_ pattern a layer. The facade
  of the facade layer is... the facade layer."
- **Johnson** (Ralph): "In Smalltalk this would have been one message
  send. Here, the message first needs an architecture review."
- **Vlissides** (John): "We described a vocabulary. You turned the
  dictionary into a staffing plan."

The original edition has [416 pages](https://www.informit.com/store/design-patterns-elements-of-reusable-object-oriented-9780321770462). The repository reached 1,000,000
lines under its own counter. Even the units need an adapter to compare.

---

## 5. Findings from the Architecture Committee

| Finding                              | Evidence                                                                                                                                                                                           | Committee minutes                                                           |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| The greeting takes a shortcut        | [`Pipeline._process()`](src/application/Pipeline.ts) reaches the controller without invoking the constructed security or rate-limiting managers.                                                   | 104 departments, and the customer found the side entrance.                  |
| E2E stops before HTTP                | [`HttpE2E.test.ts`](src/test/e2e/HttpE2E.test.ts) calls individual classes; its full-pipeline case sends no HTTP request.                                                                          | The departments exist. Interdepartmental cooperation is assumed.            |
| The test runners disagree            | [`npm test`](package.json) runs Jest; files such as [`DPoPVerifier.test.ts`](src/layers/jwt-validation/engine2/__tests__/DPoPVerifier.test.ts) import `bun:test`.                                  | Even verification has acquired incompatible regional offices.               |
| Daily quota has no next day          | [`GetHelloWorldUseCase`](src/layers/service/use-cases/GetHelloWorldUseCase.ts) only increments `_dailyCount`; [`MessagePolicy`](src/layers/domain/services/MessagePolicy.ts) caps it at 1,000,000. | One million lines is a milestone. One million greetings is retirement.      |
| Metrics count their own audience     | [`GetMetricsUseCase`](src/layers/service/use-cases/GetMetricsUseCase.ts) increments `totalRequests` when metrics are requested.                                                                    | No observer, no workload. A breakthrough in operational reporting.          |
| The million is a defined measurement | [`count-lines.js`](scripts/count-lines.js) counts source, tests, documentation, and configuration.                                                                                                 | The paperwork contributes to the KPI. Finally, an honest enterprise metric. |

---

## 6. Release Approval

The Hello World endpoint has been submitted for architectural
acceptance. Its file contains a greeting; the supporting paperwork
contains factories, grammars, and a token-based BPMN engine.
The proposal to remove these dependencies has been returned
for lack of an impact assessment on the departments concerned.

The committee confirms working doubly-linked lists in LRU,
scheduler heaps, BPMN joins, and localization machinery.
The departments have equipment, operating procedures, and tests.
Whether each nameplate matches the machinery behind it is recorded
in the inspection table above. Renaming a machine requires approval;
purchasing another machine has already been included in the budget.

The Gang of Four supplied the vocabulary. The project has supplied
the offices, reporting lines, and a factory for deciding which
strategy should select the next strategy. The book's
first chapter [accounts for the cost of patterns](https://www.cs.miu.edu/courses/cs525/References/designpatterns_ch01.pdf).
That cost has been allocated to 104 departments. A proposal to stop
700,000 lines earlier was returned: the greeting had not yet
obtained architectural clearance.

The project's corridor has 104 doors, with factories, strategies,
and health checks waiting inside. The greeting has found a shortcut.
The architecture committee would like a word with that greeting.

---

_This document is itself a pattern: the "Meta-Document" pattern, applied
twice in this repository (see also `UNCLE_BOB.md`). Its implementation
is left as an exercise for the reader._
