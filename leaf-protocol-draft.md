# Leaf Protocol Draft

The Leaf Protocol is a data format on top of the [Willow Protocol][willow]. Willow deals with how data is replicated and stored amongst peers, providing access control as well as key-value-based byte storage. On top of Willow, Leaf provides:

- A schema system and serialization format.
- A standard for creating an Entity-Component ["Web of Data"][wod].

[willow]: https://willowprotocol.org/specs/index.html#specifications
[wod]: https://zicklag.katharos.group/blog/a-web-of-data/

<!-- > **Note:** The current tech prototype for the leaf protocol, [Weird], is built on [Iroh], which is still catching up to the latest version of the Willow protocol. I ( `@zicklag` ) am still catching up on some of the later details on the Willow protocol, so some of this may need to be refined and updated. -->

[Weird]: https://github.com/commune-os/weird/
[Iroh]: https://github.com/n0-computer/iroh

## Data Model

The three major components of the Leaf Protocol data model are [`Entity`]s, [`Component`]s, and [`Schema`]s.

### Entities

An [`Entity`] represents any distinct "thing". This could be a chat message, a blog post, a comment, a feed, a profile, or anything else.

Entities are able to store data by attaching [`Component`]s to them, and they may also be the target of [`Link`]s.

Each [`Entity`] is stored in a Willow [`Namespace`][`NamespaceId`], under a specific [`Subspace`][`SubspaceId`] and [`Path`].  The [`PathComponent`]s must follow the [rules](#entity-path-components) below.

> **Note:** In this spec we refer to [Willow path components][`PathComponent`] as [`PathComponent`]s, to distinguish it from [`Component`][`Component`]s for [`Entity`]s in this specification.

[`NamespaceId`]: https://willowprotocol.org/specs/data-model/index.html#NamespaceId
[`SubspaceId`]: https://willowprotocol.org/specs/data-model/index.html#SubspaceId

#### Entity Path Components

Each [`PathComponent`] for an entity must be [Borsh] serialized data matching the following format:

```rust
enum PathComponent {
    Null,
    Bool(bool),
    Uint(u64),
    Int(i64),
    String(String),
    Bytes(Vec<u8>),
}
```

Additionally, the last [`PathComponent`] in the [`Path`] must always be suffixed with a single null byte ( `0x00` ).

> **ℹ️ Explanation:** The null byte at the end of the last [`PathComponent`] makes sure that creating an entity will never accidentally trigger _prefix pruning_ and cause other entities to be deleted. See "prefix pruning" in the [Willow data model][wdm].
>
> This means it is possible to store an entity at `"Hello"`/`"World"`/`1`, and still be able to store an entity at `"Hello"`/`"World"` without overwriting it.
>
> This is incredibly useful for allowing for the existence of "Feed" entities, or other similar group entities, that describe the purpose of or add metadata relevant to entities in it's sub-paths.

[`Path`]: https://willowprotocol.org/specs/data-model/index.html#Path
[`PathComponent`]: https://willowprotocol.org/specs/data-model/index.html#Component
[wdm]: https://willowprotocol.org/specs/data-model/index.html
[`Entity`]: #entities
[Borsh]: https://borsh.io/

#### Entity Payloads

The [`Payload`] of an entity must be a sorted list of [`ComponentId`]s.

> **Note:** Since [`ComponentId`]s are each [`PayloadDigest`]s, they must be sorted according to the [total order][ordered] of the [`PayloadDigest`]. The Willow protocol requires that [`PayloadDigest`]s have a total order.

This sorted list of [`ComponentId`]s is called an <a id="EntitySnapshot" href="#EntitySnapshot">`EntitySnapshot`</a>.

The [`PayloadDigest`] of the [`EntitySnapshot`] is called an <a id="EntitySnapshotId" href="#EntitySnapshotId">`EntitySnapshotId`</a>.

[ordered]: https://en.wikipedia.org/wiki/Total_order
[`EntitySnapshot`]: #EntitySnapshot
[`EntitySnapshotId`]: #EntitySnapshotId
[`Payload`]: https://willowprotocol.org/specs/data-model/index.html#Payload
[`PayloadDigest`]: https://willowprotocol.org/specs/data-model/index.html#PayloadDigest

### Components

[`Component`]s are pieces of data that may be attached to an [`Entity`]. The [`PayloadDigest`] of a [`Component`] is called it's <a id="ComponentId" href="#ComponentId">`ComponentId`</a>.

Components are stored individually in a [content addressable store][`PayloadDigest`]. This is usually the same store used by the Willow implementation for storing [`Entity`] [`Payload`]s.

The data of a [`Component`] is [Borsh] serialized data matching the following format:

```rust
enum Component {
    Unencrypted(ComponentData),
    Encrypted {
        algorithm: EncryptionAlgorithmId,
        key_id: [u8; 32],
        encrypted_data: Vec<u8>,
    }
}

struct ComponentData {
    schema: SchemaId,
    data: Vec<u8>,
}
```

In the `ComponentData` struct, the [`SchemaId`] is the ID of the [`Schema`] that describes the
`data` field.

#### Component Encryption

Components may be either encrypted or unencrypted. The Leaf Protocol allows any number of [`EncryptionAlgorithm`]s to be implemented. It is up to implementations to choose which algorithms to support.

When a component is encrypted, the [Borsh] serialized data matching the `ComponentData` struct will be encrypted using the algorithm and stored in the `encrypted_data` field.

The interpretation of `key_id` may be different between different encryption algorithms. This field may be used to store the public key in asymmetric key algorithms for example. An algorithm may choose to put the entire key into the `key_id` field, or it may choose to store the key in the content addressed store and put its [`PayloadDigest`] in the `key_id` field.

[`Component`]: #components
[`ComponentId`]: #ComponentId

> **ℹ️ Explanation:** This design allows for individual [`Component`]s of an [`Entity`] to be encrypted, even if other components are not encrypted. This could be useful, for example, on a user profile, where the Email for the user profile might be encrypted so that the user can choose to share it only with specific users or services.
>
> This does not prevent you from using Willow's own encryption mechanisms to encrypt the entire [`Entity`] or it's [`Path`].

### Schemas

A [`Schema`] is a description of the data that is in a component. The [`PayloadDigest`] of a [`Schema`] is called a <a id="SchemaId" href="#SchemaId">`SchemaId`</a>.

The data of a schema is [Borsh] serialized data matching the following format:

```rust
struct Schema {
    name: String,
    format: BorshSchema,
    specification: EntitySnapshotId,
}
```

The `name` of the schema is a human-readable name, for documentation purposes only.

The `format` is a [Borsh] serialized [`BorshSchema`]. This [`BorshSchema`] may be used to deserialize the [`Component`]'s `data`.

The `specification` is an [`EntitySnapshotId`] that represents the human-readable specification describing how the component data is meant to be interpreted.

> **ℹ️ Explanation ( Component Specifications ):** While the `format` in a schema is enough information to deserialize the component data, it does not give humans enough information to understand how it should be used in an application. For example, two different components might have exactly the same `format` containing a single `String` type, even though one is mean to be an email address and the other is meant to be a name. It is the specification that distinguishes them from each other and provides guidance on how applications are meant to use the data.

> **ℹ️ Explanation ( Documenting Specifications ):** [`Schema`]s, [`EncryptionAlgorithm`]s, and [`KeyResolver`]s all use an [`EntitySnapshot`] to document their `specification`s. This means that the documentation itself is described by the [`Component`]s in that [`EntitySnapshot`].
>
> The simplest form of documentation would be to add a single [ASCII Component](#the-ascii-schema) to the [`EntitySnapshot`], containing a human explanation of the specification. Alternatives could include using a `Markdown` component or an `HTML` component. This is intentionally flexible, and may even include WASM modules if useful. ( See the [note](#encryption-wasm) on [`EncryptionAlgorithm`]s. )
>
> Since each [`Component`] used to document a specification must have it's own [`Schema`], with it's own specification, you will always be able to follow the chain of specifications components and their schemas until you get to an [ASCII component](#the-ascii-schema).

#### Bootstrapping Schemas

Because [`Schema`] `specification`s are [`EntitySnapshot`]s that, in turn, contains [`Component`]s with their own [`Schema`]s, and all of them are linked by digest, it is impossible to create a [`Schema`] that uses itself in it's specification documentation. In other words, [`Schema`]s and `specifications` create a Directed Acyclic Graph ( DAG ).

This situation means that the first schema that is ever created must have a `specification` that is set to an empty [`EntitySnapshot`]. This is called an [_unspecified schema_](#unspecified-schemas).

Each specified schema, must eventually, down the chain of components and their schemas, be documented by an _unspecified_ schema. This is not ideal, so we define one special case of unspecified schema, the [Ascii Schema](#the-ascii-schema).

#### The ASCII Schema

When all the following are true of a [`Schema`], it describes the `ASCII` schema:

- The `name` is set to `ASCII`
- The `specification` is set to the ID of the empty [`EntitySnapshot`]
- The `format` is set to [`BorshSchema::String`][`BorshSchema`]

#### Unspecified Schemas

When any schema that is **not** the [ASCII Schema](#the-ascii-schema) has a `specification` set to the empty [`EntitySnapshot`] is is called an _unspecified schema_.

Unspecified schemas are generally discouraged, because although the `format` will describe the data layout of a component with the schema, it does not give any indication how that data is meant to be interpreted by apps or humans. This makes unspecified schemas ambiguous, and one app may interpret an unspecified schema in a different way than another app.

Still, nothing prevents the creation of unspecified schemas, so they are allowed to exist and be used on [`Entity`]s.

[`Schema`]: #schemas
[`SchemaId`]: #SchemaId
[ASCII]: https://en.wikipedia.org/wiki/ASCII

### Borsh Schemas

[`BorshSchema`]s are used to describe the binary format of [`Component`] `data`. [`BorshSchema`]s are themselves serialized with [Borsh] according to the following format:

```rust
enum BorshSchema {
    Null,
    U8,
    U16,
    U32,
    U64,
    U128,
    I8,
    I16,
    I32,
    I64,
    I128,
    F32,
    F64,
    String,
    Option {
        schema: BorshSchema
    },
    Array {
        schema: BorshSchema,
        len: u32,
    },
    Tuple {
        fields: Vec<BorshSchema>,
    },
    Struct {
        fields: Vec<String, BorshSchema>,
    },
    Vector {
        BorshSchema
    },
    Map {
        key: BorshSchema,
        value: BorshSchema,
    },
    Set {
        schema: Borshchema
    },
    Link {
        namespace: KeyResolverKind,
        subspace: KeyResolverKind,
        path: Vec<PathComponent>,
        snapshot: Option<EntitySnapshotId>
    }
}

enum KeyResolverKind {
    Inline([u8; 32]),
    Custom {
        id: KeyResolverId,
        data: Vec<u8>,
    },
}
```

The [`BorshSchema`] allows us to represent the [Borsh] data model so that we can deserialize component data with it. Beyond the standard Borsh data model, we add one extension: the [`Link`] type.

### Links

A [`Link`] is a reference to an entity [`Entity`]. Links allow us to build expressive graphs with our entities.

The `path` in the [`Link`] specifies the path to the entity in the `namespace` and `subspace`.

The optional `snapshot` allows you to put the [`EntitySnapshotId`] in the link, so that even if the entity is changed, moved, or deleted you can still load the data of the entity, at the time that the link was made.

The `namespace` and `subspace` specify the `KeyResolverKind` used to lookup the public keys that identify the spaces. The simplest `KeyResolverKind` is the `Inline` variant, which lets you hard-code the key. The `Custom` variant, allows you to use any [`KeyResolver`] and `data` input to the [`KeyResolver`].

[`Link`]: #links
[`BorshSchema`]: #borsh-schemas

### Key Resolvers

A [`KeyResolver`] is a specification that describes a way to resolve some data to a [`NamespaceId`] or a [`SubspaceId`]. Key resolvers allow [`Link`]s to have a level of indirection, possibly using DNS or other mechanisms such as DIDs to lookup a key, instead of hardcoding it.

The [`PayloadDigest`] of a [`KeyResolver`] is called a <a id="KeyResolverId" href="#KeyResolverId">`KeyResolverId`</a>.

Key resolvers contain a `specification`, similar to a [`Schema`], that documents how to implement the key-resolver. Each app must decide which key resolvers to implement. 

[`KeyResolver`]s are stored in a content addressed store, and their data is [Borsh] serialized data matching the following format:

```rust
struct KeyResolver {
    name: String,
    specification: EntitySnapshotId,
}
```

The `name` of the [`KeyResolver`] is a human readable name for documentation purposes.

The `specification` is the ID of an [`EntitySnapshot`] documenting the key resolution process. The specification is usually **human documentation** that preferably includes all of the information necessary to implement the key resolver.

[`KeyResolver`]: #key-resolvers

### Encryption Algorithms

An [`EncryptionAlgorithm`] is specification describing how data may be encrypted and decrypted.

The [`PayloadDigest`] of an [`EncryptionAlgorithm`] is called an <a id="EncryptionAlgorithmId" href="#EncryptionAlgorithmId">`EncryptionAlgorithmId`</a>.

The data of an [`EncryptionAlgorithm`] is [Borsh] serialized data matching the following format:

```rust
struct EncryptionAlgorithm {
    name: String,
    specification: EntitySnapshotId,
}
```

The `name` is a human readable name for documentation purposes.

The `specification` is the ID of an [`EntitySnapshot`] that documents the encryption algorithm. The specification is usually **human documentation** that preferably includes all of the information necessary to encrypt and decrypt data with the encryption algorithm.

> **Note:** <a id="encryption-wasm"></a> It is an interesting consideration that while the `specification` for an [`EncryptionAlgorithm`] should always include human documentation describing the algorithm, it might also contain additional machine-readable [`Component`]s, such as a [WASM] module that can be used to actually perform the encryption and decryption.
>
> If a standardized interface for encryption modules was developed, it might be possible to allow clients to automatically download and execute compatible encryption modules automatically.
>
> This kind of standard is allowed to develop on top of the Leaf protocol independently. Details on how this might be done is out of scope for this specification.

[WASM]: https://webassembly.org/
[`EncryptionAlgorithm`]: #encryption-algorithms

## Notes

The Leaf Protocol specifies a data format on top of Willow storage, and not much else. All of the Willow features such as the Meadowcap capability system can work with Leaf seamlessly.

The goal of Leaf is simply to provide a more expressive format for storing rich data that can be incrementally understood by different applications.
