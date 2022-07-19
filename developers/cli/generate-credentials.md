---
description: The Fission CLI commands for generating credentials
---

# Generate Credentials

Use `fission generate credentials` to generate key pairs and DIDs.

## Generate Credentials

The `fission generate credentials` command generates a new Ed25519 key pair and an associated DID.

```
$ fission generate credentials
✅ Generated an Ed25519 key pair and associated DID
🗝️  Private key: <private-key>
🔑 Public key: <public-key>
🆔 DID: <DID>
```

See the [W3C did:key Method specification](https://w3c-ccg.github.io/did-method-key/#format) for details on the derivation of DIDs from public keys.

{% hint style="danger" %}
Generated key pairs are not stored anywhere and are not associated with your Fission user account. Be careful to store a copy somewhere safe.
{% endhint %}
