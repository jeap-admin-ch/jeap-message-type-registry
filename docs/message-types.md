# Message-type catalog

This is the catalog of every message type currently defined in this registry, grouped by defining
system. All types listed here are `public` (they cross system boundaries). For each type, services
consume the generated artifact with group id `ch.admin.bit.jeap.messagetype.<system-lowercase>` and the
artifact id shown below (the type name in kebab-case); see [Getting started](getting-started.md).

None of the current descriptors pin a `topic` in the registry — the topic is configured by the
producing/consuming service. For the descriptor fields and version semantics see
[Registry structure](registry-structure.md).

In the tables below, each version links to its Avro `.avdl` value schema; key schemas are linked
where a type defines one.

## System `JEAP`

Group id `ch.admin.bit.jeap.messagetype.jeap`.

### Events

| Type                          | Artifact id                     | Versions | Purpose |
|-------------------------------|---------------------------------|----------|---------|
| `B2BMessageReceivedEvent`     | `b2b-message-received-event`    | [1.0.0](../descriptor/jeap/event/b2bmessagereceivedevent/B2BMessageReceivedEvent_v1.avdl), [1.1.0](../descriptor/jeap/event/b2bmessagereceivedevent/B2BMessageReceivedEvent_v1.1.0.avdl), [1.2.0](../descriptor/jeap/event/b2bmessagereceivedevent/B2BMessageReceivedEvent_v1.2.0.avdl), [1.3.0](../descriptor/jeap/event/b2bmessagereceivedevent/B2BMessageReceivedEvent_v1.3.0.avdl) | Signals that the Message Exchange Service received a new message. |
| `B2BMessageSentEvent`         | `b2b-message-sent-event`        | [1.0.0](../descriptor/jeap/event/b2bmessagesentevent/B2BMessageSentEvent_v1.avdl), [1.1.0](../descriptor/jeap/event/b2bmessagesentevent/B2BMessageSentEvent_v1.1.0.avdl) | Signals that an internal client of the Message Exchange Service posted a new message for a business partner. |
| `ModulithPublicationProcessingFailedEvent` | `modulith-publication-processing-failed-event` | [1.0.0](../descriptor/jeap/event/modulithpublicationprocessingfailedevent/ModulithPublicationProcessingFailedEvent_v1.0.0.avdl) | Signals that a Spring Modulith event publication has exhausted the retries of the publishing application, so an Error Handling Service instance can take it over. |
| `ProcessContextOutdatedEvent` | `process-context-outdated-event`| [1.0.0](../descriptor/jeap/event/processcontextoutdatedevent/ProcessContextOutdatedEvent_v1.0.0.avdl), [1.1.0](../descriptor/jeap/event/processcontextoutdatedevent/ProcessContextOutdatedEvent_v1.1.0.avdl), [1.2.0](../descriptor/jeap/event/processcontextoutdatedevent/ProcessContextOutdatedEvent_v1.2.0.avdl), [1.3.0](../descriptor/jeap/event/processcontextoutdatedevent/ProcessContextOutdatedEvent_v1.3.0.avdl) | Notifies the Process Context Service internally about process updates and maintenance tasks (has a [key schema](../descriptor/jeap/event/processcontextoutdatedevent/ProcessContextOutdatedEvent_key_v1.0.0.avdl)). |
| `ProcessSnapshotCreatedEvent` | `process-snapshot-created-event`| [1.0.0](../descriptor/jeap/event/processsnapshotcreatedevent/ProcessSnapshotCreatedEvent_v1.avdl), [1.1.0](../descriptor/jeap/event/processsnapshotcreatedevent/ProcessSnapshotCreatedEvent_v2.avdl), [2.0.0](../descriptor/jeap/event/processsnapshotcreatedevent/ProcessSnapshotCreatedEvent_v3.avdl), [2.0.1](../descriptor/jeap/event/processsnapshotcreatedevent/ProcessSnapshotCreatedEvent_v4.avdl) | Signals that a new process snapshot has been created. |
| `ReactionIdentifiedEvent`     | `reaction-identified-event`     | [1.0.0](../descriptor/jeap/event/reactionidentifiedevent/ReactionIdentifiedEvent_v1.0.0.avdl), [2.0.0](../descriptor/jeap/event/reactionidentifiedevent/ReactionIdentifiedEvent_v2.0.0.avdl) | Announces a reaction identified for the first time since startup of a service using the jEAP Reaction Observer (has a [key schema](../descriptor/jeap/event/reactionidentifiedevent/ReactionIdentifiedEvent_key_v1.0.0.avdl)). |
| `ReactionsObservedEvent`      | `reactions-observed-event`      | [1.0.0](../descriptor/jeap/event/reactionsobservedevent/ReactionsObservedEvent_v1.0.0.avdl) | Announces that reactions have been observed in a certain timeframe. |

### Commands

| Type                       | Artifact id                  | Versions | Purpose |
|----------------------------|------------------------------|----------|---------|
| `AddProcessDataCommand`    | `add-process-data-command`   | [1.0.0](../descriptor/jeap/command/addprocessdatacommand/AddProcessDataCommand_v1.0.0.avdl) | Asynchronously adds process-data values to a process instance for a maintenance job, serialized by origin process ID (has a [key schema](../descriptor/jeap/command/addprocessdatacommand/AddProcessDataCommand_key_v1.0.0.avdl)). |
| `CreateArtifactCommand`    | `create-artifact-command`    | [1.0.0](../descriptor/jeap/command/createartifactcommand/CreateArtifactCommand_v1.0.0.avdl), [1.0.1](../descriptor/jeap/command/createartifactcommand/CreateArtifactCommand_v1.0.1.avdl) | Triggers asynchronous process-archive artifact creation for a backfill task. |
| `CreateAuditRecordCommand` | `create-audit-record-command`| [1.0.0](../descriptor/jeap/command/createauditrecordcommand/CreateAuditRecordCommand_v1.0.0.avdl), [1.0.1](../descriptor/jeap/command/createauditrecordcommand/CreateAuditRecordCommand_v1.0.1.avdl), [1.0.2](../descriptor/jeap/command/createauditrecordcommand/CreateAuditRecordCommand_v1.0.2.avdl) | Creates an audit record. |
| `DiscardModulithPublicationCommand` | `discard-modulith-publication-command` | [1.0.0](../descriptor/jeap/command/discardmodulithpublicationcommand/DiscardModulithPublicationCommand_v1.0.0.avdl), [1.1.0](../descriptor/jeap/command/discardmodulithpublicationcommand/DiscardModulithPublicationCommand_v1.1.0.avdl) | Gives up on a specific failed generation of a Spring Modulith event publication, marking it completed without processing it again. |
| `NotifyClientCommand`      | `notify-client-command`      | [1.0.0](../descriptor/jeap/command/notifyclientcommand/NotifyClientCommand_v1.avdl), [1.0.1](../descriptor/jeap/command/notifyclientcommand/NotifyClientCommand_v1.0.1.avdl) | Notifies clients about resource changes. |
| `RetryModulithPublicationCommand` | `retry-modulith-publication-command` | [1.0.0](../descriptor/jeap/command/retrymodulithpublicationcommand/RetryModulithPublicationCommand_v1.0.0.avdl), [1.1.0](../descriptor/jeap/command/retrymodulithpublicationcommand/RetryModulithPublicationCommand_v1.1.0.avdl) | Processes a specific failed generation of a Spring Modulith event publication again. |

## System `_SHARED`

Types publishable by several systems. Group id `ch.admin.bit.jeap.messagetype._shared`.

### Events

| Type                                        | Artifact id                                      | Versions | Purpose |
|---------------------------------------------|--------------------------------------------------|----------|---------|
| `SharedArchivedArtifactVersionCreatedEvent` | `shared-archived-artifact-version-created-event` | [2.0.0](../descriptor/_shared/event/sharedarchivedartifactversioncreatedevent/SharedArchivedArtifactVersionCreatedEvent_v2.0.0.avdl), [2.0.1](../descriptor/_shared/event/sharedarchivedartifactversioncreatedevent/SharedArchivedArtifactVersionCreatedEvent_v2.0.0.avdl), [2.0.2](../descriptor/_shared/event/sharedarchivedartifactversioncreatedevent/SharedArchivedArtifactVersionCreatedEvent_v2.0.0.avdl) | Published by a Process Archive Service instance of a system when a process artifact is created/updated (uses the shared [`ProcessIdMessageKey`](../descriptor/_shared/_common/ch.admin.bit.jeap.messagetype.shared.processid.ProcessIdMessageKey.avdl)). |

## Shared records

`descriptor/_shared/_common/` holds the shared
[`ch.admin.bit.jeap.messagetype.shared.processid.ProcessIdMessageKey`](../descriptor/_shared/_common/ch.admin.bit.jeap.messagetype.shared.processid.ProcessIdMessageKey.avdl)
key record, reused as the key schema of `SharedArchivedArtifactVersionCreatedEvent`.

## Related

- [Registry structure](registry-structure.md)
- [Getting started](getting-started.md)
- [Building and publishing](building-and-publishing.md)
- [jeap-message-type-registry](../README.md)
