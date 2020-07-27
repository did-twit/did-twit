# Twitter DID method specification

The `twitter` method intends to make working with DIDs very simple at the cost of trusting [twitter.com](https://twitter.com) for assisting in resolving DID Documents.

Twitter is a highly used and influential social media network that lacks decentralization and higher levels of trust (i.e. signed messages). The `did:twitter` specification makes an attempt to increase trust in Twitter interactions.

The method leverages Linked Data Signatures without the use of a decentralized ledger or independently run trust store, and instead, relies upon the existing Twitter platform.

The objective of Twitter DID, similar to that of the [GitHub DID Method](https://github.com/decentralized-identity/github-did),  is to encourage use of the [DID Spec](https://w3c-ccg.github.io/did-spec/), by lowering the barrier to entry for use of the technology, and promote higher trust interactions.

## Method syntax

The name-string identifying this did method is `twitter`.

A DID that uses this method MUST begin with the following prefix: `did:twitter`. This string MUST be in lowercase.

The remainder of a DID after the prefix — the unique DID suffix — MUST be a valid Twitter username.

Example: `did:twitter:god`

## CRUD Operations

### Create

In order to create a Twitter DID, a Twitter user MUST:
- Generate a valid DID Twitter document using our provided CLI
- Publish a Tweet containing a [Base58](https://en.bitcoin.it/wiki/Base58Check_encoding) encoded version of the DID Document with an attached proof of the form `did:twitter:<username>?create=<b58encoding>`

### Read

In order to resolve a `did:twitter:USERNAME`, you MUST read the Tweet on the Twitter feed of a given user.

Users may opt to make links to their DID Tweet highly discoverable (i.e. a pinned twitter or link in a user's bio).

### Update

In order to update a DID Document, a Twitter MUST:
- Publish a new Tweet that contains a proof verifying ownership of key(s) listed in a previous version of the Document, that wraps the new document.
- The updating Tweet must be of the form `did:twitter:<username>?update=<b58encoding>`

###  Deactivation

Deactivation communicates to those who encounter a user's DID that it is not intended to be used for signing or verifying documents. 
- All prior Tweets containing `did:twitter` operations should be _deleted_.
- A new Tweet displaying a document in a terminal state (nothing but the identifier, and optionally a proof and/or timestamp) should be posted of the following form `did:twitter:<username>?deactivate=<b58encoding>`.

### Delete

In order to delete a DID Document, a Twitter MUST delete all Tweets pertaining to their `did:twitter` Document. For historical purposes, a user may wish to _deactivate_ but not _delete_. Deactivation is a form up update that removes all keys from a document leaving it in a terminal state.

## Twitter Concerns

Twitters character limits make using this method a pretty terrible user experience. 
- Create Tweets have an approximate length of 850 characters
- Tweets of length 240 have proofs of around 600 characters

This means for each _unsigned_ tweet its _signed_ counterpart is often over 3x the size.

A potential workaround is to use 3rd party storage for recording Tweet signatures and periodically signing over a set of Tweet identifiers to prove ownership. Of course, out-of-band challenges for Tweet authenticity can be issued at any time. 

## Security and privacy considerations

This method relies on trusting Twitter for authenticating updates to a DID Document. Users MUST use Linked Data Signatures via the `proof` field of their DID Document for strong verifiable cryptographic proofing.

Users interacting with `did:twitter` users should challenge DID Holders for authenticity frequently.
