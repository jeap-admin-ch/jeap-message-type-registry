# jEAP Common Message Type Registry

This repository is the jEAP Common Message Type Registry: a Git-based catalog of the message types
(events and commands) produced or consumed by jEAP services and libraries such as the jEAP Process
Context Service, the jEAP Message Exchange Service and the jEAP Reaction Observer. Each message type
is described by a JSON type descriptor plus one or more Avro `.avdl` schemas under
`descriptor/<system>/...`. A Maven build validates the registry and publishes a generated Java
message-type artifact per type and version, which consuming services pull in as ordinary Maven
dependencies.

* A versioned, design-time home for jEAP message-type descriptors and their Avro value/key schemas
* One JSON descriptor per type with `name`, `definingSystem`, `scope`, `topic` and `versions`
* A validation build that keeps the registry consistent and forbids changing released schema versions
* Publishing of generated Java bindings via the jeap-messaging Avro Maven plugin
  (`compile-message-types` / `deploy-message-type-artifacts`)
* Shared records under `_shared` / `_common` for keys and references reused across types and systems

## Documentation

| Topic                                                       | File                                                       |
|-------------------------------------------------------------|------------------------------------------------------------|
| Getting started (consume artifacts, add a new type)         | [docs/getting-started.md](docs/getting-started.md)         |
| Registry structure (layout, descriptor fields, validation)  | [docs/registry-structure.md](docs/registry-structure.md)   |
| Message-type catalog (the types this registry contains)     | [docs/message-types.md](docs/message-types.md)             |
| Building and publishing (Maven plugins, artifact IDs)       | [docs/building-and-publishing.md](docs/building-and-publishing.md) |

## Repository structure

This is a single-module Maven project (`packaging=pom`); the pom only drives the validation and
publishing build. The actual content lives under two directories:

```
descriptor/<system-lowercase>/event|command/<typename-lowercase>/   # descriptors + .avdl schemas
descriptor/_shared/ , descriptor/_common/                           # shared / common schemas
schema/                                                             # JSON schemas for the descriptors (IDE support)
```

## Note

This repository is part the open source distribution of jEAP. See [github.com/jeap-admin-ch/jeap](https://github.com/jeap-admin-ch/jeap)
for more information.

## License

This repository is Open Source Software licensed under the [Apache License 2.0](./LICENSE).
