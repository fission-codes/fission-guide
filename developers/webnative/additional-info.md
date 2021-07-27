---
description: "Become a webnative jedi \U0001F9D8"
---

# Additional information

## Debugging

### Version <a id="version"></a>

Check the webnative version.

```javascript
console.log(wn.VERSION)
```

### Debug Logs <a id="debug-logs"></a>

Add the following to your code to enable webnative debug logging.

```javascript
wn.setup.debug({ enabled: true })
```

When webnative makes a change to the user's filesystem, it logs:

```text
📓 Adding to the CID ledger
```

Next, webnative links the change to make it available across the web.

```text
🌊 Updating your DNSLink
```

When linking completes, the change is published and available to other browsers.

```text
🪴 DNSLink updated:
```

### **Forcing a Logout**

While you are testing an app, you may want to force a logout to check the `NotAuthorised` authentication state. The `wn.leave()` method will log out and completely remove a user from your app. The user will still be authenticated with Fission, and they can sign back into your app to re-authorize.

## **WNFS File System Roots**

WNFS comes with three separate top-level file systems "roots": public, pretty, and private.

#### Public <a id="public"></a>

Not encrypted. Full metadata support. Starts with `/public`.

#### Pretty <a id="pretty"></a>

Not encrypted. No metadata. Represented simply as `/p` to be nice and short when creating public URLs like `/p/path/to/file.img`. It does not support versioning, use the Public or Private trees for that.

#### Private <a id="private"></a>

Encrypted. Structured so that file metadata as well as contents are obscured. Starts with `/private`.

### Default folders <a id="default-folders"></a>

We initialize WNFS with a set of default _private_ folders, which should be familiar to people from working with desktop operating systems.

{% hint style="warning" %}
TODO: We'll be documenting and versioning these default folders in the webnative Github repo.
{% endhint %}

Additionally, in apps like Drive or in file pickers, the user sees a top level Public folder, which maps to the Public system root

{% hint style="info" %}
**More about roots.** Learn more about roots in the [Fission whitepaper](https://whitepaper.fission.codes/file-system/partitions/root).
{% endhint %}



## Web Worker

Can I use my file system in a web worker? Yes, this only requires a slightly different setup.

```typescript
// UI thread
// `state.fs` will now be `null`
const { permissions } = wn.initialise({ loadFileSystem: false })
worker.postMessage({ tag: "LOAD_FS", permissions })

// Web Worker
let fs

self.onMessage = async event => {
  switch (event.data.tag) {
    case "LOAD_FS":
      fs = await wn.loadFileSystem(event.data.permissions)
      break;
  }
}
```

## Versions

Since the file system may evolve over time, a "version" is associated with each node in the file system \(tracked with semver\).

Currently two versions exist:

* `1.0.0`: file tree with metadata. Nodes in the file tree are structured as 2 layers where one layer contains "header" information \(metadata, cache, etc\), and the second layer contains data or links. **This is the default version, use this unless you have a good reason not to**.
* `0.0.0`: legacy bare file tree of an early version.

## Customi**z**ation

Customization can be done using the `setup` module. Run these before anything else you do with the SDK.

```javascript
// custom api, lobby, and/or user domain
// (no need to specify each one)
wn.setup.endpoints({
  api: "https://my.fission.api",
  lobby: "https://my.fission.lobby",
  user: "my.domain"
})

// js-ipfs options
// (see docs in src for more info)
wn.setup.ipfs({ init: { repo: "my-ipfs-repo" } })
```

## Building Blocks

**Warning: Here be 🐉! Only use lower level utilities if you know what you're doing.**

This library is built on top of [js-ipfs](https://github.com/ipfs/js-ipfs) and [keystore-idb](https://github.com/fission-suite/keystore-idb). If you have already integrated an ipfs daemon or keystore-idb into your web application, you probably don't want to have two instances floating around.

You can use one instance for your whole application by doing the following:

```typescript
import * as ipfs from 'webnative/ipfs'

// get the ipfs instance that the Fission SDK is using
const ipfsInstance = await ipfs.get()

// OR set the ipfs to an instance that you already have
await ipfs.set(ipfsInstance)
```

```typescript
import * as keystore from 'webnative/keystore'

// get the keystore instance that the Fission SDK is using
const keystoreInstance = await keystore.get()

// OR set the keystore to an instance that you already have
await keystore.set(keystoreInstance)
```

