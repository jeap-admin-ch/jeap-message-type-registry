# Registry structure

The registry stores one type descriptor per message type, organised by the system that defines it, and
keeps the Avro schemas next to their descriptor. A validation build (see
[Building and publishing](building-and-publishing.md)) keeps the registry internally consistent and
prevents released schema versions from being changed.

## Directory layout

```
descriptor/
  <system-lowercase>/                       # one directory per defining system, e.g. jeap/
    event/<typename-lowercase>/             # one directory per event type
      <Type>.json                           #   the type descriptor
      <Type>_<version>.avdl                 #   value schema per version
      <Type>_key_<version>.avdl             #   optional key schema per version
    command/<typename-lowercase>/           # commands follow the same structure as events
    _common/                                # shared records for this system (keys, references)
  _shared/                                  # types that several systems may publish (Shared* prefix)
  _common/                                  # shared records available to all systems
schema/                                     # JSON schemas for the descriptors (IDE validation/completion)
```

When a message can be published by more than one application, place it under `_shared` and prefix the
type name with `Shared` (for example `SharedArchivedArtifactVersionCreatedEvent`).

Common records such as a shared key or reference go under `descriptor/<system>/_common` (per system) or
`descriptor/_common` (all systems). They are either used directly in a descriptor or imported into
other Avro schemas. A record checked into `master` must never be changed or deleted — create a new
record with a new name (e.g. a version suffix) instead.

## Descriptor data model

A type descriptor is a JSON file. Top-level fields:

| Field              | Content                                                          | Mandatory |
|--------------------|------------------------------------------------------------------|-----------|
| `name`             | Message type name (legacy aliases: `eventName` / `commandName`)  | Y         |
| `definingSystem`   | System that defines the type (legacy alias: `publishingSystem`)  | Y         |
| `description`      | Human-readable description of the type                           | Y         |
| `scope`            | `public` (crosses system boundaries) or `internal`              | Y         |
| `documentationUrl` | Link to further documentation                                    | N         |
| `topic`            | Default topic for the type (a single topic)                      | N         |
| `topics`           | List of topics for shared types (alternative to `topic`)         | N         |
| `versions`         | List of version entries (see below)                              | Y         |

> The JSON schemas under `schema/` still use the legacy property names: events require `eventName`
> and commands require `commandName`, and `definingSystem` is accepted in place of the deprecated
> `publishingSystem`. The descriptors in this registry use `eventName` / `commandName`.

Each entry in `versions` describes one schema version:

| Field               | Content                                                                                       | Mandatory |
|---------------------|-----------------------------------------------------------------------------------------------|-----------|
| `version`           | Semantic version, e.g. `1.2.0`                                                                 | Y         |
| `valueSchema`       | `.avdl` value schema file next to the descriptor                                              | Y         |
| `keySchema`         | `.avdl` key schema file                                                                        | N         |
| `compatibilityMode` | `BACKWARD` / `FORWARD` / `FULL` / `NONE` — compatibility to the previous (or `compatibleVersion`) | N         |
| `compatibleVersion` | The version this schema must be compatible with (default: the previous semver version)        | N         |

## Example descriptor

```json
{
  "eventName": "ProcessSnapshotCreatedEvent",
  "definingSystem": "JEAP",
  "description": "This event signals that a new process snapshot has been created.",
  "scope": "public",
  "versions": [
    {
      "version": "1.0.0",
      "valueSchema": "ProcessSnapshotCreatedEvent_v1.avdl",
      "compatibilityMode": "BACKWARD"
    },
    {
      "version": "2.0.0",
      "valueSchema": "ProcessSnapshotCreatedEvent_v3.avdl",
      "compatibilityMode": "NONE"
    }
  ]
}
```

## Schemas for the descriptors

`schema/` contains the JSON schemas used to validate and auto-complete descriptors in an IDE:
`EventDescriptor.schema.json`, `CommandDescriptor.schema.json` and the shared `common.schema.json`
(version, semantic-version and compatibility-mode definitions). Reference the matching schema from a
descriptor to get editor validation.

## Immutability and evolution

Released schema versions are frozen: the validation build rejects any change to an already published
`.avdl` file or version entry. To evolve a type, add a new entry to `versions` with a new value-schema
file and the compatibility mode it keeps to its predecessor.

## Related

- [Message-type catalog](message-types.md)
- [Building and publishing](building-and-publishing.md)
- [Getting started](getting-started.md)
- [jeap-message-type-registry](../README.md)
