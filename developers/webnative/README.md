# Webnative SDK

This is the extended version of the [README of the webnative SDK](https://github.com/fission-suite/webnative).  
If you're looking for the API reference that can be found at [webnative.fission.app](https://webnative.fission.app/).

You are welcome to post to the [Fission Developer forum](https://talk.fission.codes) and join the [Fission chat server](https://fission.codes/discord) to ask questions.

The webnative SDK offers tools for:

* authenticating through an **authentication lobby** \(a lobby is where you can make a Fission account or link an account\)
* managing your web native **file system**  \(this is where a user's data lives\)
* tools for building DIDs and UCANs.
* interacting with the users apps via the **platform APIs**

### **Include the webnative SDK:**

```typescript
// ES6
import * as wn from 'webnative'

// Browser/UMD build
const wn = self.webnative
```

## Authentication

```typescript
const state = await wn.initialise({
  permissions: {
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
  }
})


switch (state.scenario) {

  case wn.Scenario.AuthCancelled:
    // User was redirected to lobby,
    // but cancelled the authorisation
    break;

  case wn.Scenario.AuthSucceeded:
  case wn.Scenario.Continuation:
    // State:
    // state.authenticated    -  Will always be `true` in these scenarios
    // state.newUser          -  If the user is new to Fission
    // state.throughLobby     -  If the user authenticated through the lobby, or just came back.
    // state.username         -  The user's username.
    //
    // ☞ We can now interact with our file system (more on that later)
    state.fs
    break;

  case wn.Scenario.NotAuthorised:
    wn.redirectToLobby(state.permissions)
    break;

}
```

`redirectToLobby` will redirect you to our authentication lobby at [auth.fission.codes](https://auth.fission.codes/), where you'll be able to make a Fission account and link your account on another device or in another browser. This function takes an optional parameter, the URL that the lobby should redirect back to \(the default is `location.href`\).

Apps request `permissions` to store user data in a default app storage directory and other public and private directories. Webnative creates these directories for your app if they do not already exist.

## File System

The Web Native File System \(WNFS\) is built on top of the InterPlanetary File System \(IPFS\). It's structured and functions similarly to a Unix-style file system, with one notable exception: it's a Directed Acyclic Graph \(DAG\), meaning that a given child can have more than one parent \(think symlinks but without the "sym"\).

Each file system has a public tree and a private tree, much like your MacOS, Windows, or Linux desktop file system. The public tree is "live" and publically accessible on the Internet. The private tree is encrypted so that only the owner can see the contents.

All information \(links, data, metadata, etc\) in the private tree is encrypted. Decryption keys are stored in such a manner that access to a given folder grants access to all of its subfolders.

```typescript
// After initialising …
const fs = state.fs
const appPath = fs.appPath()

// List the user's private files that belong to this app
await fs.ls(appPath)

// Create a sub directory
await fs.mkdir(fs.appPath([ "Sub Directory" ]))
await fs.publish()
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

### Publish <a id="publicise"></a>

The `publish` function synchronises your file system with the Fission API and IPFS. We don't do this automatically because if you add a large set of data, you only want to do this after everything is added. Otherwise it would be too slow and we would have too many network requests to the API.

Returns: `CID` the updated _root_ CID for the file system

### Versioning

Each file and directory has a `history` property, which you can use to get an earlier version of that item. We use the `delta` variable as the order index. Primarily because the timestamps can be slightly out of sequence, due to device inconsistencies.

```typescript
const file = await fs.get("private/Blog Posts/article.md")

file.history.list()
// { delta: -1, timestamp: 1606236743 }
// { delta: -2, timestamp: 1606236532 }

// Get the previous version
file.history.back()

// Go back two versions
const delta = -2
file.history.back(delta)

// Get the first version (ie. delta -2)
// by providing a timestamp
file.history.prior(1606236743)
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
const updatedCID = await fs.add("public/some/path/to/a/file", content)
// creates a file called "file" at "public/some/path/to/a"
```

**cat**

Retrieves some file content at a given path

Params:

* path: `string` **required**

Returns: `FileContent` \(`object | string | Blob | Buffer`\)

Example:

```typescript
const content = await fs.cat("public/some/path/to/a/file")
```

**exists**

Checks if there is anything located at a given path

Params:

* path: `string` **required**

Returns: `boolean`

Example:

```typescript
const bool = await fs.exists("private/path/to/file")
```

**get**

Retrieves the node at the given path, either a `File` or `Tree` object

Params:

* path: `string` **required**

Returns: `Tree | File | null`

Example:

```typescript
const node = await fs.get("public/some/path")
```

**ls**

Returns a list of links at a given directory path

Params:

* path: `string` **required**

Returns: `{ [name: string]: Link }` Object with the file name as the key and its `Link` as the value.

Example:

```typescript
linksObject = await fs.ls("public/some/directory/path") // public
linksObject = await fs.ls("private/some/directory/path") // private

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
const updatedCID = await fs.mkdir("public/some/directory/path")
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
const updatedCID = await fs.mv("public/doc.md", "private/Documents/notes.md")
```

**rm**

Removes a file or directory at a given path

Params:

* path: `string` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const updatedCID = await fs.rm("private/some/path/to/a/file")
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
const updatedCID = await fs.write("public/some/path/to/a/file", content)
```

