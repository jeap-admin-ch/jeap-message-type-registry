# Getting started

This registry has two kinds of users: services that **consume** a message type defined here, and
contributors that **add or evolve** a message type. This page covers both. For the directory layout
and descriptor fields see [Registry structure](registry-structure.md); for the full list of types see
the [Message-type catalog](message-types.md).

## Consume a message type

Each message-type version published from this registry is an ordinary Maven artifact containing the
generated Java binding (the Avro-generated event/command class plus its builder). Add it as a
dependency:

```xml
<dependency>
    <groupId>ch.admin.bit.jeap.messagetype.jeap</groupId>
    <artifactId>process-snapshot-created-event</artifactId>
    <version>2.0.1</version>
</dependency>
```

- **Group id**: the `groupIdPrefix` from the build (`ch.admin.bit.jeap.messagetype`) plus the lowercase
  defining system, e.g. `ch.admin.bit.jeap.messagetype.jeap` for system `JEAP` and
  `ch.admin.bit.jeap.messagetype._shared` for `_shared` types.
- **Artifact id**: the message-type name in kebab-case, e.g. `ProcessSnapshotCreatedEvent` becomes
  `process-snapshot-created-event`.
- **Version**: the descriptor `version` you want to depend on.

Once the dependency is on the classpath, use the generated type with the jeap-messaging library
(declare a message contract, build it with its generated builder, and publish/consume it). See the
[jeap-messaging Getting started](https://github.com/jeap-admin-ch/jeap-messaging/blob/master/docs/getting-started.md)
guide for the producing/consuming code.

## Add or evolve a message type

The registry is maintained through pull requests against `master`:

1. **Check out** the repository and create a feature branch.
2. **Add a new type**: create
   `descriptor/<system-lowercase>/<event|command>/<typename-lowercase>/` containing a `<Type>.json`
   descriptor and a `<Type>_<version>.avdl` value schema (plus an optional key schema). Follow the
   field rules in [Registry structure](registry-structure.md) and the Avro naming conventions.
3. **Evolve an existing type**: add a new entry to the descriptor's `versions` array with a new
   `.avdl` file and the appropriate `compatibilityMode`. **Never** change or delete an already
   released schema — released versions are immutable.
4. **Validate locally** with `./mvnw verify`. The validation build checks consistency and rejects
   changes to released schema versions.
5. **Open a pull request** to `master`. The pipeline must pass before merge; merging publishes the new
   artifacts.

## Related

- [Registry structure](registry-structure.md)
- [Message-type catalog](message-types.md)
- [Building and publishing](building-and-publishing.md)
- [jeap-message-type-registry](../README.md)
