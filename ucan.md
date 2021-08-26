# UCAN

Basic example of how to make a UCAN. Note that the `issuer` always has to be your DID, because the UCAN will be signed with your private key.

```typescript
import * as wn from "webnative"

// DIDs
const ourDID = await wn.did.ucan()
const otherDID = "did:key:EXAMPLE"

/**
 * This can be another UCAN which has a bigger, or equal,
 * set of permissions than the UCAN we're building later.
 */
const possibleProof = null // or, other UCAN.

/**
 * The UCAN, encoded as a string.
 */
const ucan = await wn.ucan.build({
  audience: otherDID,
  issuer: ourDID,
  lifetimeInSeconds: 60 * 60 * 24, // UCAN expires in 24 hours
  proof: possibleProof
})
```



