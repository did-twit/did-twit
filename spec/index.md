# Twit DID method specification

The `twit` method intends to make working with DIDs very simple at the cost of trusting [twitter.com](https://twitter.com) for assisting in resolving DID Documents.

Twitter is a highly used and influential social media network that lacks decentralization and higher levels of trust (i.e. signed messages). The `did:twit` specification makes an attempt to increase trust in Twitter interactions.

The method is similar to [did:key](https://w3c-ccg.github.io/did-method-key) in the sense that it is uses a `did` to wrap a single public key.

The objective of Twitter DID, similar to that of the [GitHub DID Method](https://github.com/decentralized-identity/github-did),  is to encourage use of the [DID Spec](https://w3c-ccg.github.io/did-spec/), by lowering the barrier to entry for use of the technology, and promote higher trust interactions.

## Method syntax

The name-string identifying this did method is `twit`.

A DID that uses this method MUST begin with the following prefix: `did:twit`. This string MUST be in lowercase.

The remainder of a DID after the prefix — the unique DID suffix — MUST be a valid Twitter username.

Example: `did:twit:god`

## DID Twit Format

The format for the `did:twit` is based off that of the [did:key format](https://w3c-ccg.github.io/did-method-key/#format). It consists of the `did:twit` prefix, followed by a Twitter username (e.g. `did:twit:god`), followed by a [Multibase](https://github.com/multiformats/multibase) base58-btc encoded value (`z`) that is a concatenation of the [Multicodec](https://github.com/multiformats/multicodec) identifier for the public key type and the raw bytes associated with the public key format.

For now, the only supported Multicodec value is for an Ed25519 Public key or `0xed`.

The encoding rules can also be thought of as the application of a series of transformation functions on the raw public key bytes:

```
did-twit-format := did:twit:MULTIBASE(base58-btc, MULTICODEC(public-key-type, raw-public-key-bytes))
```

An example `did:twit` DID follows:
```
did:twit:god:z2DcvXXvnnG9aSrAga9U47eqzr95dRn7hux4FnoWcPTaKi9
```

## CRUD Operations

### Create

In order to create a Twit DID, a Twitter user MUST:
- Generate a valid DID Twit document using the aforementioned `did:twit` format
- Publish a Tweet containing the `did:twit` DID. 

A template message is provided below:
```
I've created a did:twit method with the the identifier did:twit:god:z2DcvXXvnnG9aSrAga9U47eqzr95dRn7hux4FnoWcPTaKi9! I will be using this identitiy to verify Tweets, and other information originating from this account. Find out more at didtwit.io.
```

It is important to highlight that the create method is the _binding_ step, in which one is uniting their Twitter identity and a public key that can be used for a multitude of cryptographic applications. The `timestamp` of the Tweet is taken as the `creation` moment for the identity.

It is recommended to pin, or otherwise highlight this creation Tweet so that it is easily discoverable.

### Read

In order to resolve a `did:twit:USERNAME`, you MUST read the Creation Tweet on the Twitter feed of a given user.

Users may opt to make links to their DID Tweet highly discoverable (i.e. a pinned twitter or link in a user's bio).

### Update

Traditional updating is not supported as `did:twit` identifiers only support one key at a time. The only supported update is a _rotation_ which changes the key used in the `did:twit` identifier.

In order to update a DID Document, a Tweeter MUST:
- Publish a new Tweet that contains the new `did:twit` value with a proof verifying ownership of key(s) listed in a previous version of the DID, that wraps the new document.

A template message is provided below:
```
I am updating my did:twit identifier from did:twit:god:z2DcvXXvnnG9aSrAga9U47eqzr95dRn7hux4FnoWcPTaKi9 to did:twit:god:z2DaoLTGdnbPLfUUq8nJDSsSqePobCmmoFG7ibApaWDfK9t.7bmm2bS9NsRPDddDPdAMCPtJqAWHzVrQMm4jthEjWMXvJErJ3nNyy2EJpPpfZm7uDzXBLRmidphwQhU1LZ13KQWU4QPZavk2bYrwS6RX5zNtA99gjhtobU5GTnyyR1XhxTpzbwL2639xH1H3xGy4FuqaDbwFGxrpabfax36UbeC2UhjBs2BeREt2CyK5czxwg5DDGHCCVE4Mik6QkDQ1EkHBSGFRYBNjtj11XQ2YBL431tN8ft88ZAVE1KYPMhy5rHChoAXH8jTx6RQtZs5V8ruwSU6gnDHGjL8C5B4cuoGv978363wG4iB7xFrtjxpAQCoMANs2zfwFUBxgS1AoKnksN2Es84wHNCyCMe3gEct6An73RQR4JXfduXRD6hmj9CdWx6DkGUxUj34TQVenSHjmjkJNdiy75ZrW3HX6dD5GZjRWAort
``` 

For historical purposes a user may choose to maintain their original `did:twit`, but, after update, no new messages should be signed with the corresponding private key of the previous identifier.

###  Deactivation & Deletion

For the `did:twit` method, deactivation and deletion are one in the same.

In order to delete a DID Document, a Twitter MUST delete all Tweets pertaining to their `did:twitter` Document.

A message may be posted alerting a user's followers to this fact.

## Generating Tweets

A utility shall be provided to assist with generating Tweets using the `did:twit` method. The format for a Tweet is:

```
<tweet text>.<Base58 encoding of a Proof>
```

where the `Proof` is a [Linked Data Signature](https://w3c-ccg.github.io/ld-proofs/) of the form

```
type Proof struct {
	Type               string `json:"type"`
	Created            string `json:"created"`
	VerificationMethod string `json:"verificationMethod"`
	Challenge          string `json:"challenge,omitempty"`
	SignatureValue     string `json:"signatureValue,omitempty"`
}
```

A sample is provided below:

```
hello world.7bmm2bS9NsRPDddDPdAMCPtJqAWHzVrQMm4jthEjWMXvJErJ3nNyy2EJpPpfZm7uDzXBLRmidphwQhui2FLDsZ2KuZN1xX26S2BXNdFTh6eHvK5UwF6pDuteuBa7pqGwszoxyYMakw719EV4oo5DQpCiUQ1WXDJaTZKgXvxZfKeS6E8bZwRiYzKizGszwJUvZ1vWmvxqYzg73156ZgFN2KVUDZ3SPGLNmEKM7QPEAxKDCbgDrhkHaWTQwAEBQ6ZyNmnzELvwrjEYjLpbrFPhs3MUHaioUpfghLedGaZSrXH79VBZFLCUsKAs4WJJW3DNdR9fQN4Ds6Yj2xtNL1PydURyUJkgvSt8mPMq9DCpnvNQq5zkWfn5DWkWbfWBpaisL4Ah64nLxwwDUULYvrU6gonmbn6hcJFeiHWDokPiHLSaXJgtYmH6
```

## Twitter Concerns

Twitters character limits make using this method a pretty terrible user experience. 
- Create Tweets have an approximate length of 850 characters
- Tweets of length 240 have proofs of around 600 characters

This means for each _unsigned_ tweet its _signed_ counterpart is often over 3x the size. Fortunately the provided mechanism scales linearlly. In testing we've found that a 1 character `did:twit` Tweet is around 438 characters, or 2 tweets. A 240 character Tweet is around 677 characters, or 3 Tweets. 

A potential workaround is to use 3rd party storage for recording Tweet signatures and periodically signing over a set of Tweet identifiers to prove ownership. Of course, out-of-band challenges for Tweet authenticity can be issued at any time.


## Security and privacy considerations

This method relies on trusting Twitter for authenticating updates to a DID Document. Users MUST use Linked Data Signatures via the `proof` field of their DID Document for strong verifiable cryptographic proofing.

Users interacting with `did:twit` users should challenge DID Holders for authenticity frequently.
