# Twitter DID

The Twitter DID method is an implementation of the [DID Core Specification](https://w3c.github.io/did-core/) that relies upon [Twitter](https://twitter.com/) as a means of partial trust and resolution.

Twitter DIDs are distinguishable from many other DID methods as they tie into an existing social media account, and often contain human-readable identifiers.

## CLI

A CLI is exposed to facilitate simple generation, update, and deactivation of `did:twitter` documents. The CLI may further be expanded to hook into the [Twitter API](https://developer.twitter.com/en/docs/tweets/post-and-engage/overview) for DID management and resolution.