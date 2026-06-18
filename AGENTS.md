# AGENTS.md

Guidance for AI coding agents working **in this repository**. For how to *consume* the published
message-type artifacts in a service, read [README.md](README.md) and the [docs/](docs/) folder.

## Project

This repository is the **jEAP Common Message Type Registry** — a Git-based registry that describes the
message types (events and commands) exchanged by jEAP services and libraries. It is **not** application
code: it contains JSON type descriptors and Avro `.avdl` schemas only. A Maven build validates the
registry and publishes a generated Java message-type artifact per type and version. The tooling itself
(the validator, the Avro Maven plugin, the registry plugin) lives in the separate
[jeap-messaging](https://github.com/jeap-admin-ch/jeap-messaging) library.

## Repository layout

```
pom.xml                                  # Single module (packaging=pom); drives validation + publishing
messagetype.template.pom.xml             # POM template for each generated message-type artifact
descriptor/<system-lowercase>/           # One directory per defining system (e.g. jeap/)
  event/<typename-lowercase>/            #   <Type>.json descriptor + <Type>_<version>.avdl value schema
  command/<typename-lowercase>/          #   (+ optional <Type>_key_<version>.avdl key schema)
descriptor/_shared/                      # Types publishable by several systems (Shared* prefix)
descriptor/_common/ , <system>/_common/  # Shared records (keys, references) reused across types
schema/                                  # JSON schemas for the descriptors (CommandDescriptor, EventDescriptor, common)
Jenkinsfile, publiccode.yml, LICENSE
```

There are **no CHANGELOG.md** and **no child modules** in this repository.

## Build & validate

```bash
./mvnw verify        # validates the registry (consistency + immutability of released schemas)
```

- Parent: `ch.admin.bit.jeap:jeap-spring-boot-parent`. Java 25.
- The build runs the `jeap-messaging-registry-maven-plugin` (`registry` goal) and the
  `jeap-messaging-avro-maven-plugin` (`compile-message-types`, `deploy-message-type-artifacts`).
- The validation build **must pass** before a change is merged to `master`. It rejects edits to
  schema versions that are already released — never change or delete an existing `.avdl`; add a new
  version instead.

## Conventions (load-bearing)

- **Directory naming**: `descriptor/<system-lowercase>/<event|command>/<typename-lowercase>/`. The
  type-name directory is the descriptor name in lowercase.
- **Descriptor fields**: `name` (legacy aliases `eventName` / `commandName`), `definingSystem` (legacy
  alias `publishingSystem`), `description`, `scope` (`public` / `internal`), optional `topic` /
  `topics` / `documentationUrl`, and a `versions` array. Each version has `version`, `valueSchema`,
  optional `keySchema`, `compatibilityMode` (`BACKWARD` / `FORWARD` / `FULL` / `NONE`) and optional
  `compatibleVersion`. See [docs/registry-structure.md](docs/registry-structure.md).
- **Avro naming**: value record names end in `Event` or `Command`; key types end in `MessageKey`;
  reference types end in `References`/`Reference`; payload types end in `Payload`. These suffixes are
  used by the Avro plugin and validator and must not be used for unrelated inner types.
- **Immutability**: a descriptor/schema version checked into `master` is frozen. Evolve a type by
  adding a new entry to `versions` with a new `.avdl`, declaring its `compatibilityMode`.
- Reference the JSON schemas under `schema/` from descriptors in your IDE for validation and
  completion.

## Docs

When adding or changing message types, keep the catalog in
[docs/message-types.md](docs/message-types.md) in sync with the actual descriptors, and update the
documentation index in the [README.md](README.md) if a new doc topic is added.

## Versioning

- Semantic Versioning. The project `<version>` lives directly in `pom.xml` (single module).
- On a feature branch keep the `-SNAPSHOT` suffix; CI removes it when releasing.
- This repo has **no CHANGELOG.md**. When bumping the version, also update `softwareVersion` and
  `releaseDate` in `publiccode.yml`.
- Use the JIRA ID from the branch name as the commit-message prefix (e.g. `JEAP-1234 Add ...`); do not
  use conventional commits.
