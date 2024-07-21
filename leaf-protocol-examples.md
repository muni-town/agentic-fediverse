# How to use Leaf?

This is a set of ideas for how to make use of this web of data. None of these are specifications / normative at the time of writing.

## Well known Namespace

Use a default communal Namespace for discovering things. e.g. If doing DID resolution of a generative method like `did:key` that only provides a key for the user, use the well known Namespace, and the generated key as the subspace.

The well known Namespace recommended is `00000000000000000000000000006c656166` (That's ending in the ascii for `leaf`).
Note this has the property required in the current [earthstar spec](https://github.com/earthstar-project/earthstar/blob/willow/src/identifiers/share.ts#L165-L167) that the last bit is a 0. (And iroh will likely be following this)

## DID-based Link KeyResolvers for subspaces

Using the `Inline` `KeyResolver` for a `Link` is simple - but has some significant disadvantages when considering longer term usage.

If you hardcode Links to subspace IDs it's immutable, but if someone's key is lost or compromised then they can't recover from this situation, except by starting an entirely new identity.

Instead consider using a `KeyResolve` that supports [W3C DIDs](https://www.w3.org/TR/did-core/). This defers the choice of how to manage a long lived identity to the user (albeit with the caveat of applications needing to support the desired methods).

You should probably support `did:key` and `did:web` at a minimum. `did:web` allows people to make a trade off between complete self-sovereignty of identity, and being able to recover from their one key being lost. (Domain names being something where there is a wide range of compatibile providers, and legal backing for recovering them)

## Profiles at the root

Use the willow Path `[[]]` - a single empty PathComponent - to store your profile Entity.

This could have these sorts of Components (and of course many more):
- NameDescription
- DateOfBirth
- Image

When considering how to link in data that's being provided by an identity, prefer Components on the root Entity, rather than conventional paths.

e.g. For a user's cooking recipes, don't do "find their space, and look at `/cooking`. Insted do "look up the Recipes Component on their profile, which will have a Link to some subtree".

## Subtrees for different audiences.

From a read perspective - if you follow the `Recipes` component to `/cooking`, you can then read every entry you find under it - that you can decrypt.

From a write perspective - put some stuff in `/cooking/public/<item>`, and other stuff in `/cooking/family-only/`. You can encrypt stuff in family-only, and only give the keys to your family. But they don't need to know about this, they can just read everything they find as one big recipe database.
Also generalises to say, giving one person additional access to one recipe, by giving them the key.

Note - you can also do this by meadowcap grants on the subtrees. Whether you do permissioning by cap, encryption or both is up to you. Using capabilities is great for purely p2p - but if you want to use hosted stores you may also want to use encryption, so the host you're using can't steal Grandma's cookie recipe.

## Social media feeds

If you want to use the protocol for blogging or micro-blogging, add a `Feed` component to your profile. This points to a subtree that other people can then crawl for all posts.

As above - you could put stuff in `/social/feed/public`, and `/social/feed/followers-only`

Possibly use multiple `Feed` components if you want to have distinct options people can follow?

## Notes about other DIDs

Add a `DIDInfo` Component to your Profile, pointing to a subtree of info about other DIDs.

Why? (Or, what info?)

These entities might have Components like:
- `Link` (to the profile, using DID resolution)
- `Following` - for use in SSB-style building up a set of people to sync content from and build your social media feed (beyond just the people you're following directly)
- `Blocked` or `Muted`? If you want to publicly announce that you're not seeing their stuff. (This is possibly relevant from a perspective of, if the follow graph looks like `A->B->C->D`, B could mark D as blocked, even though their following C. And then A won't see D's stuff either, unless they have a different path.). Lots of interesting options in this space.
