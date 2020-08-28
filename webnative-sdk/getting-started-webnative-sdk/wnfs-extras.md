---
description: Extra information about working with WNFS
---

# WNFS Extras

Extra information about working with WNFS.

### Web Worker

Can I use my file system in a web worker?  
Yes, this only requires a slightly different setup.

```typescript
// UI thread
// `session.fs` will now be `null`
sdk.initialise({ loadFileSystem: false })

// Web Worker
const fs = await sdk.loadFileSystem()
```

### Versions

Since the file system may evolve over time, a "version" is associated with each node in the file system \(tracked with semver\).

Currently two versions exist:

* `1.0.0`: file tree with metadata. Nodes in the file tree are structured as 2 layers where one layer contains "header" information \(metadata, cache, etc\), and the second layer contains data or links. **This is the default version, use this unless you have a good reason not to**.
* `0.0.0`: bare file tree. The public tree consists of [ipfs dag-pg](https://github.com/ipld/js-ipld-dag-pb) nodes. The private tree is encrypted links with no associated metadata. These should really only be used for vanity links to be rendered by a gateway.

