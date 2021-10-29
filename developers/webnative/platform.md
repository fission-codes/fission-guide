---
description: Fission Platform APIs
---

# Platform APIs

The webnative platform API provides methods to work with the apps associated with users.

The platform API makes it possible to manage user apps and, combined with the webnative filesystem methods, create and manage apps entirely from the browser.

{% hint style="success" %}
Yes, you can build an app, that creates apps on behalf of the user! This is a way for developers to make use of the Fission Platform to build their own MicroSaaS business, including custom domains and other features for their users.
{% endhint %}

The API methods are prefixed with `apps`.

* `apps.index`: A list of all of your apps and their associated domain names
* `apps.create`: Creates a new app, assigns an initial subdomain, and sets an asset placeholder
* `apps.publish`: Publish a new app version
* `apps.deleteByDomain`: Destroy an app identified by domain

## Permissions

Apps that use the platform API must request permission to work with a user's apps. Permissions are requested when a user signs in through the Fission Auth Lobby. See the [Auth guide](auth.md) for more on the webnative authentication flow.

Platform API permissions are requested at `permissions.platform.apps` in the initialization object.

```javascript
import * as wn from 'webnative'

const permissions = {
  platform: {
    apps: "*",
  },
  // ... other permissions
}

// Initialise webnative with expected permissions
wn.initialise({ permissions }).then(state => {
  if (!state.authenticated) {
    // We don't have the permissions yet. 
    // Let's send the user to auth.fission.codes and ask for them:
    wn.redirectToLobby(permissions)
  } else {
    // we're all set up! 🎉
  }
})
```

The value at `permissions.platform.apps` can be

* `"*"`: Grant complete app management access for all of the user's apps
* An array of domain names, e.g. `[ "an-app.fission.app", "another-app.fission.app" ]`: Grant permission to manage specific apps. Those apps can be published or deleted.

## API

**apps.index**

Lists all user apps and their associated domain names.

Required permissions: `{ platform: { apps: "*" } } `full app management permissions

Params: No parameters

Returns: `App[]` with the domain names, creation and modification times for each app

Example:

```typescript
const index = await wn.apps.index()
// [ 
//   { domains: ['your-fission-deployment.fission.app'],
//     insertedAt: '<creation-time>',
//     modifiedAt: '<last-modified-time>',
//   } 
// ]
```

**apps.create**

Creates a new app, assigns an initial subdomain, and publishes a placeholder site.

Required permissions: `{ platform: { apps: "*" } } `full app management permissions

Params:

* subdomain: `string` **optional**

Returns: `{ domain: string }` the subdomain of the newly created app

Example:

```typescript
const newApp = await wn.apps.create()
// { domain: 'your-fission-deployment.fission.app' }
```

**apps.publish**

Publishes a new app version by IPFS CID. If the app does not exist yet, create the app first with `apps.create`.

Required permissions: Needs either permission for the app domain or full app management permissions. See [Permissions](platform.md#permissions).

Params:

* domain: `string` **required**
* cid: `string` **required**

Returns: Nothing returned

Example:

```typescript
await sdk.apps.publish('your-fission-deployment.fission.app', 'QmRVvvMeMEPi1zerpXYH9df3ATdzuB63R1wf3Mz5NS5HQN')
```

Retrieving the CID depends on where you have staged the app code. One convenient way to do this is to publish the app's HTML, CSS, JavaScript, and assets to a public directory in WNFS and retrieve the CID of that directory.

```typescript
// The POSIX path where you published your app code in the public filesystem:
const appPath = `Apps/your-fission-deployment/Published`

const ipfs = await wn.ipfs.get()
const rootCid = await fs.root.put()
const stats = await ipfs.files.stat(`/ipfs/${rootCid}/p/${appPath}/`)

// The CID you use to publish with the plaform API:
const cid = stats.cid.toBaseEncodedString()

await wn.apps.publish('your-fission-deployment.fission.app', cid)
```

**apps.deleteByDomain**

Delete an app by domain.

Required permissions: Needs either permission for the app domain or full app management permissions. See [Permissions](platform.md#permissions).

Params:

* url: `string` **required**

Returns: Nothing returned

Example:

```typescript
await wn.apps.deleteByDomain('your-fission-deployment.fission.app')
```
