# Message-type catalog

This is the catalog of every message type currently defined in this registry, grouped by defining
system. All types listed here are `public` (they cross system boundaries). For each type, services
consume the generated artifact with group id `ch.admin.bit.jeap.messagetype.<system-lowercase>` and the
artifact id shown below (the type name in kebab-case); see [Getting started](getting-started.md).

None of the current descriptors pin a `topic` in the registry — the topic is configured by the
producing/consuming service. For the descriptor fields and version semantics see
[Registry structure](registry-structure.md).

## System `JEAP`

Group id `ch.admin.bit.jeap.messagetype.jeap`.

### Events

| Type                          | Artifact id                     | Versions                              | Purpose                                                                                                  |
|-------------------------------|---------------------------------|---------------------------------------|---------------------------------------------------------------------------------------------------------|
| `B2BMessageReceivedEvent`     | `b2b-message-received-event`    | 1.0.0, 1.1.0, 1.2.0, 1.3.0            | Signals that the Message Exchange Service received a new message.                                        |
| `B2BMessageSentEvent`         | `b2b-message-sent-event`        | 1.0.0, 1.1.0                          | Signals that an internal client of the Message Exchange Service posted a new message for a business partner. |
| `ProcessContextOutdatedEvent` | `process-context-outdated-event`| 1.0.0, 1.1.0, 1.2.0                   | Notifies the Process Context Service internally that a new message arrived for a process instance (has a key schema). |
| `ProcessSnapshotCreatedEvent` | `process-snapshot-created-event`| 1.0.0, 1.1.0, 2.0.0, 2.0.1            | Signals that a new process snapshot has been created.                                                   |
| `ReactionIdentifiedEvent`     | `reaction-identified-event`     | 1.0.0, 2.0.0                          | Announces a reaction identified for the first time since startup of a service using the jEAP Reaction Observer (has a key schema). |
| `ReactionsObservedEvent`      | `reactions-observed-event`      | 1.0.0                                 | Announces that reactions have been observed in a certain timeframe.                                      |

### Commands

| Type                       | Artifact id                  | Versions       | Purpose                                                                  |
|----------------------------|------------------------------|----------------|--------------------------------------------------------------------------|
| `CreateArtifactCommand`    | `create-artifact-command`    | 1.0.0          | Triggers asynchronous process-archive artifact creation for a backfill task. |
| `CreateAuditRecordCommand` | `create-audit-record-command`| 1.0.0, 1.0.1   | Creates an audit record.                                                 |
| `NotifyClientCommand`      | `notify-client-command`      | 1.0.0, 1.0.1   | Notifies clients about resource changes.                                 |

## System `_SHARED`

Types publishable by several systems. Group id `ch.admin.bit.jeap.messagetype._shared`.

### Events

| Type                                        | Artifact id                                      | Versions              | Purpose                                                                                              |
|---------------------------------------------|--------------------------------------------------|-----------------------|------------------------------------------------------------------------------------------------------|
| `SharedArchivedArtifactVersionCreatedEvent` | `shared-archived-artifact-version-created-event` | 2.0.0, 2.0.1, 2.0.2   | Published by a Process Archive Service instance of a system when a process artifact is created/updated (uses the shared `ProcessIdMessageKey`). |

## Shared records

`descriptor/_shared/_common/` holds the shared
`ch.admin.bit.jeap.messagetype.shared.processid.ProcessIdMessageKey` key record, reused as the key
schema of `SharedArchivedArtifactVersionCreatedEvent`.

## Related

- [Registry structure](registry-structure.md)
- [Getting started](getting-started.md)
- [Building and publishing](building-and-publishing.md)
- [jeap-message-type-registry](../README.md)
