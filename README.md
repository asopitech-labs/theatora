# Theatora

**Change the theatre. Keep the code.**

Theatora is a composable kit for building your own application backend from capabilities, modules, providers, bindings, and runtimes.

An application can begin on one laptop and grow into a distributed system. Theatora keeps application code coupled to capability contracts instead of a particular database, queue, object store, or execution environment.

- [Explore the landing page](https://asopitech-labs.github.io/theatora/)
- [View the repository](https://github.com/asopitech-labs/theatora)

> Theatora is an early-stage open-source project. The model and interfaces are being shaped in public.

## Why Theatora?

A performance may move from a small theatre to a much larger venue without changing the performance itself. Application backends should be able to evolve in the same way.

Theatora separates four concerns:

```text
Modules           reusable backend behavior
Capabilities      contracts describing what the application needs
Bindings          connections from contracts to implementations
Providers         concrete databases, queues, object stores, and services
Runtimes          local, container, serverless, edge, cluster, or mixed execution
```

Applications depend on capabilities. A backend definition selects the implementations and runtimes appropriate for the current stage.

## Follow and participate

- **Star** — [save Theatora on GitHub](https://github.com/asopitech-labs/theatora)
- **Watch** — [follow repository activity](https://github.com/asopitech-labs/theatora/subscription)
- **Share** — send the [landing page](https://asopitech-labs.github.io/theatora/) to someone designing a backend that needs room to grow
- **Discuss** — [bring a use case, provider, runtime, or migration question](https://github.com/asopitech-labs/theatora/discussions)
- **Request or report** — [open a feature request or bug report](https://github.com/asopitech-labs/theatora/issues/new/choose)

Concrete applications are especially useful: what capabilities do they need, which providers do they use today, where should the work run, and what would make migration risky?

## Core model

### Modules

Modules package reusable backend behavior such as authentication, organizations, billing, notifications, inventories, leaderboards, lobbies, and matchmaking. A module can consume several capabilities while exposing higher-level application behavior.

### Capabilities

A capability describes **what the application needs**, independently of a specific implementation.

Examples include:

```text
Identity          RelationalStore   ObjectStore
Queue             Realtime          Compute
Workflow          Observability     Secrets
```

### Bindings and providers

Providers implement capabilities. Bindings select which implementation supplies each capability.

```text
RelationalStore  → PostgreSQL or AlopexDB
ObjectStore      → local filesystem or S3-compatible storage
Queue            → local queue or NATS
```

The application should depend on `RelationalStore`, not on PostgreSQL-specific calls, when the capability contract is sufficient.

### Runtimes

Backend work can run where it belongs:

```text
local process · native executable · container · WASM
serverless · edge · cluster · cloud infrastructure
```

Provider composition and runtime topology are related, but they are not the same decision.

## Backend definition

A composition can be described declaratively:

```yaml
name: example-app

modules:
  - identity
  - notifications

bindings:
  database:
    capability: relational
    provider: alopexdb

  objects:
    capability: object
    provider: s3

  events:
    capability: queue
    provider: nats

runtime:
  provider: container
```

The definition represents the desired application backend, not merely a list of infrastructure resources.

## From one laptop to a distributed system

Theatora aims to let the application keep the same capability-facing code as its backend composition and execution topology evolve.

A conventional growth path may replace several implementations:

| Capability | One laptop | Production |
| --- | --- | --- |
| Relational data | SQLite | PostgreSQL |
| Objects | Local filesystem | S3-compatible storage |
| Queue | In-process queue | NATS |
| Compute | Local process | Containers |

Changing the composition does **not** by itself move persisted data. Replacing SQLite with PostgreSQL, for example, still requires a data migration plan.

AlopexDB is the preferred relational path when the goal is to preserve the data layer while changing topology:

| Stage | Relational provider | Data transition |
| --- | --- | --- |
| One laptop | AlopexDB embedded/local | Start with the production data model |
| Production | AlopexDB durable/managed topology | Reconfigure or expand the topology |
| Distributed | AlopexDB distributed topology | Scale the same data system |

The intended advantage is not that migration disappears by definition. It is that staying within AlopexDB can avoid a cross-database engine migration as the audience grows. Any topology change still needs operational planning, validation, backup, and a rollout strategy.

## Application backends

The same model can describe different backend compositions:

- **Web and SaaS** — identity, organizations, relational data, objects, jobs, email, webhooks, billing, audit
- **Realtime applications** — identity, presence, synchronization, coordination, events, queues, objects
- **Games** — player identity, cloud save, inventory, economy, leaderboards, lobbies, matchmaking, sessions, LiveOps
- **Edge applications** — routing, edge compute, key-value data, objects, caches, queues, configuration, workflows

## Project direction

Theatora is designed around extension points for new capabilities, providers, domain modules, runtimes, lifecycle hooks, policies, and deployment integrations.

The current priority is to test the composition model against real backend requirements before locking down interfaces. Join the [first use-case discussion](https://github.com/asopitech-labs/theatora/discussions) or open an [issue](https://github.com/asopitech-labs/theatora/issues/new/choose).

## License

[MIT](LICENSE)
