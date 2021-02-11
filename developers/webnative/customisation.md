# Customisation

Customisation can be done using the `setup` module.  
Run these before anything else you do with the SDK.

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

