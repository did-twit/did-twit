# Twitter DID method specification

The `twitter` method is meant to make working with DIDs very simple at the cost of trusting Twitter.com for assisting in resolving DID Documents.

Many developers are familar with Github, and its 2 supported public key cryptosystems, GPG and SSH.

Linked Data Signatures are difficult to work with when operating a server or running a local node of some distributed system / blockchain is a requirement.

The objective of Twitter DID, similar to the [GitHub DID](https://github.com/decentralized-identity/github-did) is to encourage use of the [DID Spec](https://w3c-ccg.github.io/did-spec/) by lowering the barrier to entry for use of the technology.

## Method syntax

The namestring identifying this did method is `twitter`

A DID that uses this method MUST begin with the following prefix: `did:twitter`. This string MUST be in lowercase.

The remainder of a DID after the prefix – the did unique suffix - MUST be a valid Twitter username.

Example: `did:twitter:god`

## CRUD Operations

### Create

In order to create a Twitter DID, a Twitter user MUST:
- Generate a valid DID Twitter document using our provided CLI
- Publish a Tweet cointaining a [Base64](https://en.wikipedia.org/wiki/Base64) encoded version of the DID Document with an attached proof.

### Read

In order to resolve a `did:twitter:USERNAME`, you MUST read the Tweet on the Twitter feed of a given user.

Users may opt to make links to their DID Tweet highly discoverable (i.e. link in Twitter bio).

### Update

In order to update a DID Document, a Twitter MUST publish a new Tweet that contains a proof verifying ownership of key(s) listed in a previous version of the Document, that wraps the new document. The providing CLI can help with this as well.

### Delete

In order to delete a DID Document, a Twitter MUST delete all Tweets pertaining to their `did:twitter` Document. For historical purposes, a user may wish to _deactivate_ but not _delete_. Deactivation is a form up update that removes all keys from a document leaving it in a terminal state.

## Security and privacy considerations

This method relies on trusting Twitter for authenticating updates to a DID Document. Users MUST use Linked Data Signatures via the `proof` field of their DID Document for strong verifiable cryptographic proofing.
