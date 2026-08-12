# Theatora

**Build a theatre for your application.**

Theatora is a composable kit for building application backends from interchangeable capabilities, modules, providers, and runtimes.

An application is a performance. Its user-facing experience is the stage. Behind it is a theatre: identity, data, storage, messaging, compute, workflows, realtime communication, policies, observability, and the infrastructure that keeps everything running.

Theatora gives you the pieces and conventions to build that theatre yourself.

## What is Theatora?

Theatora is not a preassembled backend service.

It is a framework for **describing, composing, implementing, and deploying backend systems**.

Instead of starting with a particular cloud service, database, runtime, or deployment model, you describe what the application needs:

```text
Application
    │
    ▼
Backend Definition
    │
    ├── Capabilities
    ├── Domain Modules
    ├── Bindings
    ├── Providers
    └── Runtimes
```

The resulting backend may run as a local process, containers, cloud services, edge functions, distributed infrastructure, or a combination of them.

## The Theatre Metaphor

A performance needs more than a stage.

# Theatora

**Build a theatre for your application.**

Theatora is a composable kit for building application backends from interchangeable capabilities, modules, providers, and runtimes.

An application is a performance. Its user-facing experience is the stage. Behind it is a theatre: identity, data, storage, messaging, compute, workflows, realtime communication, policies, observability, and the infrastructure that keeps everything running.

Theatora gives you the pieces and conventions to build that theatre yourself.

## What is Theatora?

Theatora is not a preassembled backend service.

It is a framework for **describing, composing, implementing, and deploying backend systems**.

Instead of starting with a particular cloud service, database, runtime, or deployment model, you describe what the application needs:

```text
Application
    │
    ▼
Backend Definition
    │
    ├── Capabilities
    ├── Domain Modules
    ├── Bindings
    ├── Providers
    └── Runtimes
```

The resulting backend may run as a local process, containers, cloud services, edge functions, distributed infrastructure, or a combination of them.

## The Theatre Metaphor

A performance needs more than a stage.

Different performances require different combinations of them.

## Core Model

### Capabilities

A capability describes **what an application needs**, independently of a particular implementation.

Examples include:

```text
Identity
RelationalStore
KeyValueStore
ObjectStore
Cache
Queue
EventBus
Realtime
Compute
Scheduler
Workflow
Secrets
Configuration
Observability
```

Capabilities establish contracts between applications, modules, and implementations.

### Providers

A provider supplies an implementation of one or more capabilities.

For example:

```text
RelationalStore
    ├── SQLite
    ├── PostgreSQL
    └── other providers

ObjectStore
    ├── local filesystem
    ├── S3-compatible storage
    └── cloud object storage

Queue
    ├── local implementation
    ├── NATS
    └── cloud queue services
```

Applications should depend on the capability they require rather than unnecessarily coupling themselves to a provider.

### Bindings

Bindings connect capabilities to their actual providers.

Conceptually:

```yaml
bindings:
  users:
    capability: relational
    provider: postgres

  cache:
    capability: kv
    provider: redis

  assets:
    capability: object
    provider: s3

  events:
    capability: queue
    provider: nats
```

The exact configuration format and interfaces are part of Theatora's specification and tooling.

### Modules

Modules package reusable backend behavior.

A module can consume capabilities while exposing higher-level application functionality.

Examples include:

```text
Authentication
Organizations
Notifications
Audit Log
Billing
Asset Library

Cloud Save
Inventory
Economy
Leaderboard
Lobby
Matchmaking
```

This creates a hierarchy such as:

```text
Provider
    ↓
Capability
    ↓
Module
    ↓
Application Backend
```

A leaderboard, for example, is not merely a database table. It is backend behavior that may use storage, caching, events, authorization, and other capabilities.

### Runtimes

Backend logic is not inherently tied to one execution environment.

Theatora's model accommodates runtimes such as:

```text
local processes
native executables
containers
WASM
serverless functions
edge runtimes
clusters
cloud infrastructure
```

Providers and runtimes can therefore evolve independently where their capability contracts permit it.

## Backend Definition

A Theatora backend can be described declaratively.

Conceptually:

```yaml
name: example-app

modules:
  - identity
  - users
  - notifications

bindings:
  database:
    capability: relational
    provider: postgres

  objects:
    capability: object
    provider: s3

  events:
    capability: queue
    provider: nats

runtime:
  provider: container
```

The definition represents the **desired composition of the backend**, rather than merely a list of infrastructure resources.

Tooling can use this definition for operations such as:

```text
create
compose
validate
inspect
diff
deploy
migrate
upgrade
```

## Domain Backends

Theatora's composition model is not restricted to conventional web applications.

### Web and SaaS

```text
Identity
Organization
Database
Object Storage
Jobs
Email
Webhooks
Billing
Audit
```

### Realtime Applications

```text
Identity
Realtime
Presence
Synchronization
Coordination
Events
Queues
Object Storage
```

### Games

```text
Player Identity
Cloud Save
Inventory
Economy
Leaderboard
Lobby
Matchmaking
Game Sessions
LiveOps
```

### Edge Applications

```text
Routing
Edge Compute
KV
Object Storage
Cache
Queues
Configuration
Coordination
Workflows
```

These are different backend compositions built from the same underlying model.

## Local and Distributed Systems

The logical backend and its physical deployment are separate concerns.

A capability may be implemented differently depending on the environment:

```text
Development

RelationalStore → SQLite
ObjectStore     → local filesystem
Queue           → local queue
Compute         → local process
```

and elsewhere:

```text
Production

RelationalStore → PostgreSQL
ObjectStore     → S3-compatible storage
Queue           → NATS
Compute         → containers
```

More complex deployments can introduce replication, clustering, edge execution, managed services, or other infrastructure without redefining the application's backend solely in infrastructure terms.

## Extensibility

Theatora is designed around extension points rather than a closed catalogue of services.

The model provides places for:

* new capabilities
* new providers
* new domain modules
* new runtimes
* lifecycle hooks
* configuration
* policies
* deployment integrations

The ecosystem can therefore grow horizontally without requiring every backend capability to be implemented by the Theatora project itself.

## The Idea

Software development has accumulated excellent databases, object stores, queues, identity systems, runtimes, cloud services, edge platforms, and domain-specific backend components.

The difficult part is often not the absence of these pieces.

It is turning them into **one coherent backend for a particular application**.

Theatora provides the theatre in which those pieces can work together.

**Your application is the performance. Build the theatre it needs.**

Different performances require different combinations of them.

Application backends have the same property.

A web application might need identity, relational data, object storage, background jobs, and email.

A realtime application might add presence, synchronization, queues, and coordination.

A game might require player identity, cloud saves, inventories, leaderboards, matchmaking, sessions, and LiveOps.

There is no single backend configuration that represents all of these systems.

Theatora treats the backend as something to **compose for the application that uses it**.

## Core Model

### Capabilities

A capability describes **what an application needs**, independently of a particular implementation.

Examples include:

```text
Identity
RelationalStore
KeyValueStore
ObjectStore
Cache
Queue
EventBus
Realtime
Compute
Scheduler
Workflow
Secrets
Configuration
Observability
```

Capabilities establish contracts between applications, modules, and implementations.

### Providers

A provider supplies an implementation of one or more capabilities.

For example:

```text
RelationalStore
    ├── SQLite
    ├── PostgreSQL
    └── other providers

ObjectStore
    ├── local filesystem
    ├── S3-compatible storage
    └── cloud object storage

Queue
    ├── local implementation
    ├── NATS
    └── cloud queue services
```

Applications should depend on the capability they require rather than unnecessarily coupling themselves to a provider.

### Bindings

Bindings connect capabilities to their actual providers.

Conceptually:

```yaml
bindings:
  users:
    capability: relational
    provider: postgres

  cache:
    capability: kv
    provider: redis

  assets:
    capability: object
    provider: s3

  events:
    capability: queue
    provider: nats
```

The exact configuration format and interfaces are part of Theatora's specification and tooling.

### Modules

Modules package reusable backend behavior.

A module can consume capabilities while exposing higher-level application functionality.

Examples include:

```text
Authentication
Organizations
Notifications
Audit Log
Billing
Asset Library

Cloud Save
Inventory
Economy
Leaderboard
Lobby
Matchmaking
```

This creates a hierarchy such as:

```text
Provider
    ↓
Capability
    ↓
Module
    ↓
Application Backend
```

A leaderboard, for example, is not merely a database table. It is backend behavior that may use storage, caching, events, authorization, and other capabilities.

### Runtimes

Backend logic is not inherently tied to one execution environment.

Theatora's model accommodates runtimes such as:

```text
local processes
native executables
containers
WASM
serverless functions
edge runtimes
clusters
cloud infrastructure
```

Providers and runtimes can therefore evolve independently where their capability contracts permit it.

## Backend Definition

A Theatora backend can be described declaratively.

Conceptually:

```yaml
name: example-app

modules:
  - identity
  - users
  - notifications

bindings:
  database:
    capability: relational
    provider: postgres

  objects:
    capability: object
    provider: s3

  events:
    capability: queue
    provider: nats

runtime:
  provider: container
```

The definition represents the **desired composition of the backend**, rather than merely a list of infrastructure resources.

Tooling can use this definition for operations such as:

```text
create
compose
validate
inspect
diff
deploy
migrate
upgrade
```

## Domain Backends

Theatora's composition model is not restricted to conventional web applications.

### Web and SaaS

```text
Identity
Organization
Database
Object Storage
Jobs
Email
Webhooks
Billing
Audit
```

### Realtime Applications

```text
Identity
Realtime
Presence
Synchronization
Coordination
Events
Queues
Object Storage
```

### Games

```text
Player Identity
Cloud Save
Inventory
Economy
Leaderboard
Lobby
Matchmaking
Game Sessions
LiveOps
```

### Edge Applications

```text
Routing
Edge Compute
KV
Object Storage
Cache
Queues
Configuration
Coordination
Workflows
```

These are different backend compositions built from the same underlying model.

## Local and Distributed Systems

The logical backend and its physical deployment are separate concerns.

A capability may be implemented differently depending on the environment:

```text
Development

RelationalStore → SQLite
ObjectStore     → local filesystem
Queue           → local queue
Compute         → local process
```

and elsewhere:

```text
Production

RelationalStore → PostgreSQL
ObjectStore     → S3-compatible storage
Queue           → NATS
Compute         → containers
```

More complex deployments can introduce replication, clustering, edge execution, managed services, or other infrastructure without redefining the application's backend solely in infrastructure terms.

## Extensibility

Theatora is designed around extension points rather than a closed catalogue of services.

The model provides places for:

* new capabilities
* new providers
* new domain modules
* new runtimes
* lifecycle hooks
* configuration
* policies
* deployment integrations

The ecosystem can therefore grow horizontally without requiring every backend capability to be implemented by the Theatora project itself.

## The Idea

Software development has accumulated excellent databases, object stores, queues, identity systems, runtimes, cloud services, edge platforms, and domain-specific backend components.

The difficult part is often not the absence of these pieces.

It is turning them into **one coherent backend for a particular application**.

Theatora provides the theatre in which those pieces can work together.

**Your application is the performance. Build the theatre it needs.**
