# Getting Started with webnative SDK

This is the extended version of the [README of the webnative SDK](https://github.com/fission-suite/webnative). Detailed code documentation is [available in the source code](https://github.com/fission-suite/webnative/tree/master/docs).

You are welcome to post to the [Fission Developer forum](https://talk.fission.codes) and join the [Fission chat server](https://fission.codes/discord) to ask questions.

The webnative SDK offers tools for:

* authenticating through an **authentication lobby** \(a lobby is where you can make a Fission account or link an account\)
* managing your web native **file system**  \(this is where a user's data lives\)
* tools for building DIDs and UCANs.
* interacting with the users apps via the **platform APIs**

\*\*\*\*

### **Include the webnative  SDK:**

```typescript
// ES6
import * as wn from 'webnative'

// Browser/UMD build
const wn = self.webnative
```

## Authentication

```typescript
const { prerequisites, scenario, state } = await wn.initialise({
  // Will ask the user permission to store
  // your apps data in `private/Apps/Nullsoft/Winamp`
  app: {
    name: "Winamp",
    creator: "Nullsoft"
  },

  // Ask the user permission for additional filesystem paths
  fs: {
    privatePaths: [ "Music" ],
    publicPaths: [ "Mixtapes" ]
  }
})

if (scenario.authCancelled) {
  // User was redirected to lobby,
  // but cancelled the authorisation.

} else if (scenario.authSucceeded || scenario.continuation) {
  // State:
  // state.authenticated    -  Will always be `true` in these scenarios
  // state.newUser          -  If the user is new to Fission
  // state.throughLobby     -  If the user authenticated through the lobby, or just came back.
  // state.username         -  The user's username.
  //
  // ☞ We can now interact with our file system (more on that later)
  state.fs

} else if (scenario.notAuthorised) {
  wn.redirectToLobby(prerequisites)

}
```

`redirectToLobby` will redirect you to [auth.fission.codes](https://auth.fission.codes) our authentication lobby, where you'll be able to make a Fission account and link with another account that's on another device or browser.

The function takes an optional parameter, the url that the lobby should redirect back to \(the default is `location.href`\).

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

The Web Native File System \(WNFS\) is built on top of the InterPlanetary File System \(IPFS\). It's structured and functions similarly to a Unix-style file system, with one notable exception: it's a Directed Acyclic Graph \(DAG\), meaning that a given child can have more than one parent \(think symlinks but without the "sym"\).

Each file system has a public tree and a private tree, much like your MacOS, Windows, or Linux desktop file system. The public tree is "live" and publically accessible on the Internet. The private tree is encrypted so that only the owner can see the contents.

All information \(links, data, metadata, etc\) in the private tree is encrypted. Decryption keys are stored in such a manner that access to a given folder grants access to all of its subfolders.

```typescript
// After initialising …
const fs = state.fs
const appPath = fs.appPath()

// List the user's private files that belong to this app
if (await fs.exists(appPath)) {
  await fs.ls(appPath)

// The user is new to the app, lets create the app-data directory.
} else {
  await fs.mkdir(appPath)
  await fs.publicise()

}

// Create a sub directory
await fs.mkdir(fs.appPath([ "Sub Directory" ]))
await fs.publicise()
```

### Basics

WNFS exposes a familiar POSIX-style interface:

* `add`: add a file
* `cat`: retrieve a file
* `exists`: check if a file or directory exists
* `ls`: list a directory
* `mkdir`: create a directory
* `mv`: move a file or directory
* `read`: alias for `cat`
* `rm`: remove a file or directory
* `write`: alias for `add`

### Publicise

The `publicise` function synchronises your file system with the Fission API and IPFS. We don't do this automatically because if you add a large set of data, you only want to do this after everything is added. Otherwise it would be too slow and we would have too many network requests to the API.

### Permissions

Every file system action checks if you received the sufficient permissions from the user. Permissions are given to the app by the auth lobby. The permissions to ask the user are determined by the "prerequisites" you give to `wn.initialise`, such as `app`.

The initialise function will indicate the `notAuthorised` scenario if one of the necessary tokens will expire in one day, to minimise the likelihood of receiving this error message. But to be safe, you should account for this error:

```typescript
try {
  await fs.mkdir(...)
} catch (err) {
  if (err instanceOf wn.errors.NoPermissionError) {
    // Redirect the user back to the auth page to get the permissions
    wn.redirectToLobby(prerequisites)
  }
}
```

### API

#### Methods

Methods for interacting with the filesystem all use **absolute** paths.

**add**

Adds some file content at a given path

Params:

* path: `string` **required**
* content: `FileContent` \(`object | string | Blob | Buffer`\) **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const content = "hello world"
const updatedCID = await wnfs.add("public/some/path/to/a/file", content)
// creates a file called "file" at "public/some/path/to/a"
```

**cat**

Retrieves some file content at a given path

Params:

* path: `string` **required**

Returns: `FileContent` \(`object | string | Blob | Buffer`\)

Example:

```typescript
const content = await wnfs.cat("public/some/path/to/a/file")
```

**exists**

Checks if there is anything located at a given path

Params:

* path: `string` **required**

Returns: `boolean`

Example:

```typescript
const bool = await wnfs.exists("private/path/to/file")
```

**get**

Retrieves the node at the given path, either a `File` or `Tree` object

Params:

* path: `string` **required**

Returns: `Tree | File | null`

Example:

```typescript
const node = await wnfs.get("public/some/path")
```

**ls**

Returns a list of links at a given directory path

Params:

* path: `string` **required**

Returns: `{ [name: string]: Link }` Object with the file name as the key and its `Link` as the value.

Example:

```typescript
linksObject = await wnfs.ls("public/some/directory/path") // public
linksObject = await wnfs.ls("private/some/directory/path") // private

// convert to list
links = Object.entries(linksObject)

// working with links
data = await Promise.all(links.map(([name, _]) => {
  return fs.cat(`private/some/directory/path/${name}`)
}))
```

**mkdir**

Creates a directory at the given path

Params:

* path: `string` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const updatedCID = await wnfs.mkdir("public/some/directory/path")
// creates a directory called "path" at "public/some/directory"
```

**mv**

Move a directory or file from one path to another

Params:

* pathA: `string` **required**
* pathB: `string` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const updatedCID = await wnfs.mv("public/doc.md", "private/Documents/notes.md")
```

**rm**

Removes a file or directory at a given path

Params:

* path: `string` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const updatedCID = await wnfs.rm("private/some/path/to/a/file")
```

**write**

Alias for `add`.

Params:

* path: `string` **required**
* content: `FileContent` \(`object | string | Blob | Buffer`\) **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const content = "hello world"
const updatedCID = await wnfs.write("public/some/path/to/a/file", content)
```

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

## Customisation

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

## Apps API

webnative also exposes methods to interact with the apps associated with the user. This API must be prefixed with `apps`.

* `apps.index`: A list of all of your apps and their associated domain names
* `apps.create`: Creates a new app, assigns an initial subdomain, and sets an asset placeholder
* `apps.deleteByURL`: Destroy app by any associated URL

### API

**apps.index**

A list of all of your apps and their associated domain names

Params:

Returns: `{ RandomKey : [ subdomain ] }` a map of subdomains

Example:

```typescript
const index = await wn.apps.index()
// { `SqlBackendKey {unSqlBackendKey = 216} `: ['your-fission-deployment.fission.app'] }
```

**apps.create**

Creates a new app, assigns an initial subdomain, and sets an asset placeholder

Params:

* subdomain: `string` **optional**

Returns: `subdomain` the newly created subdomain

Example:

```typescript
const newApp = await wn.apps.create()
// 'your-fission-deployment.fission.app'
```

**apps.deleteByURL**

Destroy app by any associated URL

Params:

* url: `string` **required**

Returns:

Example:

```typescript
const deletedApp = await wn.apps.deleteByURL('your-fission-deployment.fission.app')
//
```

## Building Blocks

**Warning: Here be 🐉! Only use lower level utilities if you know what you're doing.**

This library is built on top of [js-ipfs](https://github.com/ipfs/js-ipfs) and [keystore-idb](https://github.com/fission-suite/keystore-idb). If you have already integrated an ipfs daemon or keystore-idb into your web application, you probably don't want to have two instances floating around.

You can use one instance for your whole application by doing the following:

```typescript
import ipfs from 'webnative/ipfs'

// get the ipfs instance that the Fission SDK is using
const ipfsInstance = await ipfs.get()

// OR set the ipfs to an instance that you already have
await ipfs.set(ipfsInstance)
```

```typescript
import keystore from 'webnative/keystore'

// get the keystore instance that the Fission SDK is using
const keystoreInstance = await keystore.get()

// OR set the keystore to an instance that you already have
await keystore.set(keystoreInstance)
```

