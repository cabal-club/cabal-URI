# Cabal-URI
Specification for Cabal URIs (aka cabal keys, or identified by `cabal://`)

Cabal's basic URI scheme is composed of the `cabal://` protocol identifier, followed by a 64 character long `base16` identifier. **See the _Basic_  scheme below.**

**Basic**: `cabal://[0-9A-Fa-f]{64}`

As part of the work we are doing with [adding a subjective moderation system](https://opencollective.com/cabal-club/updates/cabal-x-subjective-moderation-mozilla-grant) to Cabal, and funded by [Mozilla's Fix The Internet Spring Lab](https://builders.mozilla.community/springlab/index.html), we decided on an extended scheme (inspired by & compatible with HTTP's [query string](https://en.wikipedia.org/wiki/Query_string)). **See the _Extended_ scheme below**.

**Extended**: `cabal://[0-9A-Fa-f]{64}?<key1>=<value>&<key2>=<value>`   
for any amount of key-value pairs (i.e. parity with HTTP)

Two examples of Cabal URIs:
1. cabal://1eef9ad64e284691b7c6f6310e39204b5f92765e36102046caaa6a7ff8c02d74
2. cabal://1eef9ad64e284691b7c6f6310e39204b5f92765e36102046caaa6a7ff8c02d74?admin=1211400f1aa2e3076a3f17f4521b2cc41e258c446cdaa44742afe6e1b9fd5f82

Example 1 is a simple cabal key without any parameters. Example 2 extends example 1 with an `admin` key, identifying the public key of the subjective administrator to be applied upon joining.


Additionally, there is support in [Cabal clients](https://github.com/cabal-club/cabal-client/blob/21fed68727b522bda612466a76c5d24d89ab5008/src/client.js#L53-L63) for a DNS shortname system.
In these cases, the section following the protocol identifier is a domain name where the cabal key may be fetched. This is accomplished either by quering a well-known record at the domain name, i.e. the file `<valid-domain.tld>/.well-known/cabal`, or by querying a TXT record on the domain. The TXT record is identified by `cabalkey`.
The first line of the `.well-known/cabal` file contains the cabal key, as described by the _Basic_ and _Extended_ schemes above. The second line contains a [TTL](https://en.wikipedia.org/wiki/Time_to_live) in milliseconds, in the format of `ttl=<milliseconds>`.

**DNS**: `cabal://<valid-domain.tld>`

Thus, Cabal's URIs are either identified by the _Extended_ scheme above (which is a superset of the _Basic_ scheme) or the _DNS_ scheme, just explained.
