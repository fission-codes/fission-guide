# Webnative SDK

This guide is an extended version of the [webnative SDK README](https://github.com/fission-suite/webnative). You can find the complete API reference at [webnative.fission.app](https://webnative.fission.app/).

{% hint style="info" %}
**Have questions?** Come join the [Fission Discord server](https://fission.codes/discord) or post your question to the [Fission Developer forum](https://talk.fission.codes/c/developers/7/none). We are here to help!
{% endhint %}

The webnative SDK offers tools for:

* **Auth.** Users make an account and link their account across devices in the [Fission auth lobby](https://auth.fission.codes).
* **Storage.** The webnative file system \(WNFS\) stores user data for apps.
* **UCAN and DID tools** for working with UCANs and distributed identities
* **Platform APIs** for interacting with user apps

## Installation

You can add the webnative SDK to a project by loading it from a CDN or installing it with a package manager.

### CDN

For prototyping or experimentation, you can use the latest version of the SDK.

```markup
<script src="https://unpkg.com/webnative@latest/dist/index.umd.js"></script>
```

We recommend linking to a specific version in production to avoid unexpected changes.

### Package Manager

We recommend installing with a package manager for larger projects that use build tools or bundlers.

Install the SDK with `npm` or your preferred package manager.

```bash
npm install webnative
```

Import the SDK in your application.

```typescript
// ES6
import * as wn from 'webnative'

// Browser/UMD build
const wn = self.webnative
```

## Paths

Webnative uses directory and file paths built from path segments by path functions.

```javascript
// creates a directory path equivalent to "public/some/directory/"
const publicDirectoryPath = wn.path.directory("public", "some", "directory")

// creates a directory path equivalent to "private/some/directory/"
const privateDirectoryPath = wn.path.directory("private", "some", "directory")

// creates a file path equivalent to "public/some/file"
const publicFilePath = wn.path.file("public", "some", "file")

// creates a file path equivalent to "private/some/file"
const privateFilePath = wn.path.file("private", "some", "file")

// wn.path.Branch.Private is an alias for "private"
const privateHelperPath = wn.path.file(wn.path.Branch.Private, "some", "file")

// wn.path.Branch.Public is an alias for "public"
const publicHelperPath = wn.path.file(wn.path.Branch.Public, "some", "file")
```

All webnative SDK methods expect paths created by path functions. See the [path API documentation](https://webnative.fission.app/modules/path.html) for more path utility functions.

{% hint style="info" %}
**Path Objects.** The path functions create objects like `{ directory: ["public", "some", "directory"] }` or `{ file: ["public", "some", "file"] }`. We recommend you use path functions because they validate paths to make sure they are well-formed.
{% endhint %}

## Auth

```javascript
const state = await wn.initialise({
  permissions: {
    // Will ask the user permission to store
    // your apps data in `private/Apps/Nullsoft/Winamp`
    app: {
      name: "Winamp",
      creator: "Nullsoft"
    },

    // Ask the user permission to additional filesystem paths
    fs: {
      private: [ wn.path.directory("Audio", "Music") ],
      public: [ wn.path.directory("Audio", "Mixtapes") ]
    }
  }

}).catch(err => {
  switch (err) {
    case wn.InitialisationError.InsecureContext:
      // We need a secure context to do cryptography
      // Usually this means we need HTTPS or localhost

    case wn.InitialisationError.UnsupportedBrowser:
      // Browser not supported.
      // Example: Firefox private mode can't use indexedDB.
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

`redirectToLobby` redirects users to our auth lobby at [auth.fission.codes](https://auth.fission.codes/), where they can make a Fission account and link their account on another device or in another browser. This function takes an optional parameter, the URL that the lobby should redirect back to \(the default is `location.href`\).

Apps request `permissions` to store user data in a default app storage directory and other public and private directories. Webnative creates these directories for your app if they do not already exist.

## File System

The Web Native File System \(WNFS\) is built on top of the InterPlanetary File System \(IPFS\). WNFS is structured and functions similarly to a Unix-style file system, with one notable exception: it's a Directed Acyclic Graph \(DAG\), meaning that a given child can have more than one parent \(think symlinks but without the "sym"\).

Each file system has a public tree and a private tree, much like your macOS, Windows, or Linux desktop file system. The public tree is "live" and publicly accessible on the Internet. The private tree is encrypted so that only the owner can see the contents.

All information \(links, data, metadata, etc.\) in the private tree is encrypted. Decryption keys are stored so that access to a given folder grants access to all of its subfolders.

```typescript
// After initialising …
const fs = state.fs

// List the user's private files that belong to this app
await fs.ls(fs.appPath())

// Create a sub directory and add some content
await fs.write(
  fs.appPath(wn.path.file("Sub Directory", "hello.txt")),
  "👋"
)

// Announce the changes to the server
await fs.publish()
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
* `write`: alias for `add`

### Publish <a id="publicise"></a>

The `publish` function synchronises your file system with the Fission API and IPFS. WNFS does not publish changes automatically because it is more practical to batch changes in some cases. For example, a large data set is better published once than over multiple calls to `publish`.

Returns: `CID` the updated _root_ CID for the file system

{% hint style="warning" %}
**Remember to publish!** If you do not call `publish` after making changes, user data will not be persisted to WNFS.
{% endhint %}

### Versioning

Each file and directory has a `history` property, which you can use to get an earlier version of that item. We use the `delta` variable as the order index, primarily because the timestamps can be slightly out of sequence due to device inconsistencies.

```typescript
const articlePath = wn.path.file("private", "Blog Posts", "article.md")
const file = await fs.get(articlePath)

file.history.list()
// { delta: -1, timestamp: 1606236743 }
// { delta: -2, timestamp: 1606236532 }

// List more than (by default) 5 versions
file.history.list(10)

// Get the previous version
file.history.back()

// Go back two versions
const delta = -2
file.history.back(delta)

// Get a version strictly before a timestamp
// The first version (delta -2) is prior to
// the second version (delta -1) timestamp
file.history.prior(1606236743)
```

{% hint style="warning" %}
Requesting many versions with `file.history.list` can be slow. The acceptable delay will depend on your application. 
{% endhint %}

### API

#### Methods

Methods for interacting with the filesystem all use **absolute** paths.

Paths created by [path functions](./#paths) have a `FilePath` or `DirectoryPath` type. Methods with a `DistinctivePath` param accept either a `FilePath` or a `DirectoryPath`.

**add**

Adds some file content at a given path

Params:

* path: `FilePath` **required**
* content: `FileContent` \(`object | string | Blob | Buffer`\) **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const content = "hello world"

// create a file called "file" at "public/path/to/a/"
await fs.add(
  wn.path.file("public", "path", "to", "a", "file"),
  content
)
```

**cat**

Retrieves some file content at a given path

Params:

* path: `FilePath` **required**

Returns: `FileContent` \(`object | string | Blob | Buffer`\)

Example:

```typescript
const content = await fs.cat(wn.path.file("public", "some", "file"))
```

**exists**

Checks if there is anything located at a given path

Params:

* path: `DistinctivePath` **required**

Returns: `boolean`

Example:

```typescript
const bool = await fs.exists(wn.path.file("private", "some", "file"))
```

**get**

Retrieves the node at the given path, either a `File` or `Tree` object

Params:

* path: `DistinctivePath` **required**

Returns: `Tree | File | null`

Example:

```typescript
const node = await fs.get(wn.path.directory("public", "some", "directory"))
```

**ls**

Returns a list of links at a given directory path

Params:

* path: `DirectoryPath` **required**

Returns: `{ [name: string]: Link }` Object with the file name as the key and its `Link` as the value.

Example:

```typescript
// public directory
const publicPath = wn.path.directory("public", "some", "directory")
const publicLinksObject = await fs.ls(publicPath)

// private directory
const privatePath = wn.path.directory("private", "some", "directory")
const privateLinksObject = await fs.ls(privatePath)

// convert private links object to a list
const links = Object.entries(privateLinksObject)

// working with links
const data = await Promise.all(links.map(([name, _]) => {
  return fs.cat(
    wn.path.file("private", "some", "directory", `${name}`
  )
}))
```

**mkdir**

Creates a directory at the given path

Params:

* path: `DirectoryPath` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
// create a directory called "directory" at "public/some/"
const updatedCID = await fs.mkdir(wn.path.directory("public", "some", "directory"))
```

**mv**

Move a directory or file from one path to another

Params:

* from: `DistinctivePath` **required**
* to: `DistinctivePath` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const fromPath = wn.path.file("public", "doc.md")
const toPath = wn.path.file("private", "Documents", "notes.md")
const updatedCID = await fs.mv(fromPath, toPath)
```

**rm**

Removes a file or directory at a given path

Params:

* path: `DistinctivePath` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const updatedCID = await fs.rm(wn.path.file("private", "some", "file"))
```

**write**

Alias for `add`.

Params:

* path: `FilePath` **required**
* content: `FileContent` \(`object | string | Blob | Buffer`\) **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const content = "hello world"
const updatedCID = await fs.write(
  wn.path.file("private", "some", "file"), 
  content
)
```

