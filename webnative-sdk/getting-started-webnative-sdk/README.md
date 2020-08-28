# Getting Started with webnative SDK

This content is from the [README of the webnative SDK](https://github.com/fission-suite/webnative). Extended documentation is [available in the source code](https://github.com/fission-suite/webnative/tree/master/docs).

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
import * as sdk from 'webnative'

// Browser/UMD build
self.webnative
```

## Authenticating

```typescript
const { scenario, state } = await sdk.initialise()

if (scenario.authCancelled) {
  // User was redirected to lobby,
  // but cancelled the authorisation.

} else if (scenario.authSucceeded || scenario.continuum) {
  // State:
  // state.authenticated    -  Will always be `true` in these scenarios
  // state.newUser          -  If the user is new to Fission
  // state.throughLobby     -  If the user authenticated through the lobby, or just came back.
  // state.username         -  The user's username.
  //
  // ☞ We can now interact with our file system (more on that later)
  state.fs

} else if (scenario.notAuthenticated) {
  sdk.redirectToLobby()

}
```

`redirectToLobby` will redirect you to [auth.fission.codes](https://auth.fission.codes) our authentication lobby, where you'll be able to make a Fission account and link with another account that's on another device or browser.

The function takes an optional parameter, the url that the lobby should redirect back to \(the default is `location.href`\).

### Other functions

* `await sdk.deauthenticate()`
* `await sdk.authenticatedUsername()`



## File System

The Web Native File System \(WNFS\) is built on top of the InterPlanetary File System \(IPFS\). It's structured and functions similarly to a Unix-style file system, with one notable exception: it's a Directed Acyclic Graph \(DAG\), meaning that a given child can have more than one parent \(think symlinks but without the "sym"\).

Each file system has a public tree and a private tree, much like your MacOS, Windows, or Linux desktop file system. The public tree is "live" and publically accessible on the Internet. The private tree is encrypted so that only the owner can see the contents.

All information \(links, data, metadata, etc\) in the private tree is encrypted. Decryption keys are stored in such a manner that access to a given folder grants access to all of its subfolders.

```typescript
// After authenticating …
const fs = state.fs

// List the user's private files that belong to this app
const appPath = fs.appPath.private("myApp")

if (await fs.exists(appPath)) {
  await fs.ls(appPath)
} else {
  await fs.mkdir(appPath)
}
```

### Basics

WNFS exposes a POSIX-style interface:

* `add`: add a file
* `cat`: retrieve a file
* `exists`: check if a file or directory exists
* `ls`: list a directory
* `mkdir`: create a directory
* `mv`: move a file or directory
* `read`: alias for `cat`
* `rm`: remove a file or directory
* `write`: write to a file

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



