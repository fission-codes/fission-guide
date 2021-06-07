---
description: Fission Platform APIs
---

# Platform APIs

The SDK also exposes methods to interact with the apps associated with the user. This API must be prefixed with `apps`

* `apps.index`: A list of all of your apps and their associated domain names
* `apps.create`: Creates a new app, assigns an initial subdomain, and sets an asset placeholder
* `apps.publish`: Publish a new app version
* `apps.deleteByDomain`: Destroy an app identified by domain

## Permissions

To use the platform APIs, your app needs to ask the user for permission to access the user's apps. This is handled via the parameters to `webnative.initialise` and `webnative.redirectToLobby`:

```javascript
const permissions = {
  platform: {
    apps: "*",
  },
  // ... other permissions
}

// Initialise webnative with expected permissions
webnative.initialise({ permissions }).then(state => {
  if (!state.authenticated) {
    // We don't have the permissions yet. Let's send the user to auth.fission.codes and ask for them:
    webnative.redirectToLobby(permissions)
  } else {
    // we're all set up! 🎉
  }
})
```

The value at `permissions.platform.apps` can be

* `"*"`: Complete app management access for all of the user's apps
* An array of domain names, e.g. `[ "an-app.fission.app", "another-app.fission.app" ]`: Permission to manage particular apps. Those apps can be published to or deleted.

## API

**apps.index**

A list of all of your apps and their associated domain names.

Needs these permissions: `{ platform: { apps: "*" } }`.

Params:

Returns: `{ domain: string }[]` an array of app domains

Example:

```typescript
const index = await sdk.apps.index()
// [ { domain: 'your-fission-deployment.fission.app' } ]
```

**apps.create**

Creates a new app, assigns an initial subdomain, and sets an asset placeholder.

Needs these permissions: `{ platform: { apps: "*" } }`.

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

Needs either permissions for the particular app domain or full app management permissions. See [Permissions](platform.md#permissions).

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

Destroy app by domain.

Needs either permissions for the particular app domain or full app management permissions. See [Permissions](platform.md#permissions).

Params:

* url: `string` **required**

Returns:

Example:

```typescript
await sdk.apps.deleteByDomain('your-fission-deployment.fission.app')
```

