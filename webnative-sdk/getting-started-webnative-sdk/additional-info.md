---
description: "Become a webnative jedi \U0001F9D8"
---

# Additional information

## Lobby

### Authorisation

The auth lobby is responsible for authorisation as well, it'll give us a UCAN \(or token if you will\) with various scopes based on the values we gave to `wn.initialise`. Important to note here is that if one of those tokens, that we got from a previous session, is expired, the scenario will be `notAuthorised`.

### Shared devices

Our vision for "fission-enabled apps" is that users don't really need to sign out, unless they are on a shared device \(a device they normally don't use\). You can read more about our vision on this on [our forum](https://talk.fission.codes/t/what-does-log-in-or-log-out-mean-for-the-fission-sdk-and-apps/919).

Signing out on shared devices would be two-fold: 1. Remove any authorisation tokens from the current domain \(ie. for your app\) 2. Sign out of the auth lobby

This function will do that first part, and then redirect you to the auth lobby:

```javascript
wn.leave()
```

## File System

### Permissions

Every file system action checks if you received the sufficient permissions from the user. Permissions are given to the app by the auth lobby. The permissions to ask the user are determined by the "prerequisites" you give to `wn.initialise`, such as `app`.

The initialise function will indicate the `notAuthorised` scenario if one of the necessary tokens will expire in one day, to minimise the likelihood of receiving this error message. But to be safe, you should account for this error:

### Web Worker

Can I use my file system in a web worker?  
Yes, this only requires a slightly different setup.

```typescript
// UI thread
// `state.fs` will now be `null`
const { prerequisites } = wn.initialise({ loadFileSystem: false })
worker.postMessage({ tag: "LOAD_FS", prerequisites })

// Web Worker
let fs

self.onMessage = async event => {
  switch (event.data.tag) {
    case "LOAD_FS":
      fs = await wn.loadFileSystem(event.data.prerequisites)
      break;
  }
}
```

### Versions

Since the file system may evolve over time, a "version" is associated with each node in the file system \(tracked with semver\).

Currently two versions exist:

* `1.0.0`: file tree with metadata. Nodes in the file tree are structured as 2 layers where one layer contains "header" information \(metadata, cache, etc\), and the second layer contains data or links. **This is the default version, use this unless you have a good reason not to**.
* `0.0.0`: bare file tree. The public tree consists of [ipfs dag-pg](https://github.com/ipld/js-ipld-dag-pb) nodes. The private tree is encrypted links with no associated metadata. These should really only be used for vanity links to be rendered by a gateway.

