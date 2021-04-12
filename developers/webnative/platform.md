---
description: Fission Platform APIs
---

# Platform APIs

The SDK also exposes methods to interact with the apps associated with the user. This API must be prefixed with `apps`

* `apps.index`: A list of all of your apps and their associated domain names
* `apps.create`: Creates a new app, assigns an initial subdomain, and sets an asset placeholder
* `apps.deleteByURL`: Destroy app by any associated URL

### API

**apps.index**

A list of all of your apps and their associated domain names

Params:

Returns: `{ domain: string }[]` an array of app domains

Example:

```typescript
const index = await sdk.apps.index()
// [ { domain: 'your-fission-deployment.fission.app' } ]
```

**apps.create**

Creates a new app, assigns an initial subdomain, and sets an asset placeholder

Params:

* subdomain: `string` **optional**

Returns: `subdomain` the newly created subdomain

Example:

```typescript
const newApp = await sdk.apps.create()
// 'your-fission-deployment.fission.app'
```

**apps.publish**

Publishes a new app version by IPFS CID. If the app doesn't exist yet, it has to be created with `apps.create` first.

Params:

* domain: `string` **required**
* cid: `string` **required**

Example:

```typescript
await sdk.apps.publish('your-fission-deployment.fission.app', 'QmRVvvMeMEPi1zerpXYH9df3ATdzuB63R1wf3Mz5NS5HQN')
```

Getting a CID can be tricky. Here's a way to turn a WNFS public subdirectory into a CID:
```typescript
const appPath = "Apps/your-fission-deployment/Published` // If you've put app files here
const ipfs = await webnative.ipfs.get()
const rootCid = await fs.root.put()
const stats = await ipfs.files.stat(`/ipfs/${rootCid}/p/${appPath}/`)
const cid = stats.cid.toBaseEncodedString()
// This is the CID you can use for publish:
await sdk.apps.publish('your-fission-deployment.fission.app', cid)
```

**apps.deleteByDomain**

Destroy app by domain

Params:

* url: `string` **required**

Returns:

Example:

```typescript
await sdk.apps.deleteByDomain('your-fission-deployment.fission.app')
```

## 

