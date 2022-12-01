---
description: Working with the Webnative File System (WNFS)
---

# File System (WNFS)

The Web Native File System (WNFS) is a file system built on top of the InterPlanetary File System (IPFS). Each Fission user has their own WNFS, and apps store user files and data in it when granted permission.

Each file system has a public tree and a private tree, much like your macOS, Windows, or Linux desktop file system. The public tree is "live" and publicly accessible on the Internet. The private tree is encrypted so that only the owner can see the contents.

All information (links, data, metadata, etc.) in the private tree is encrypted. Decryption keys are stored so that access to a given directory grants access to all of its subdirectories.

WNFS is structured and functions similarly to a Unix-style file system, with one notable exception: it's a Directed Acyclic Graph (DAG), meaning that a given child can have more than one parent (think symlinks but without the "sym").

## Permissions

Every file system action checks if an app has received sufficient permissions from the user. Apps request `permissions` when they initialize webnative. The [Fission Auth Lobby grants authorization](auth.md#authorization).

Apps request permission for app storage, additional private and public directories, and user apps published with the Platform APIs. For example, a notes app might request these permissions.

```javascript
permissions: {
  app: {
    name: "Notes",
    creator: "Fission"
  },
  fs: {
    private: [ wn.path.directory("Notes") ],
    public: [ wn.path.directory("Notes") ]
  },
  platform: {
    apps: []
  }
}
```

The app would have access to its dedicated app storage and public and private Notes directories.

Apps request `permissions.app` to store user data in a default app storage directory and other public and private directories. Webnative creates these directories for your app if they do not already exist.

{% hint style="info" %}
**Platform permissions.** The platform permissions could be left out of this example because this app will not need them. See the [Platform API guide](platform.md#permissions) for more information on working with user apps.
{% endhint %}

The initialize function will return a `NotAuthorised` scenario if one of the UCAN will expire in one day to minimize the likelihood of receiving an expired permissions error. But to be safe, apps should also account for this error.

## Paths

WNFS uses directory and file paths built from path segments by path functions.

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

All WNFS operations expect paths created by path functions. See the [path API documentation](https://webnative.fission.app/modules/path.html) for more path utility functions.

{% hint style="info" %}
**Path Objects.** The path functions create objects like `{ directory: ["public", "some", "directory"] }` or `{ file: ["public", "some", "file"] }`. We recommend you use path functions because they validate paths to make sure they are well-formed.
{% endhint %}

## File System Interface

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

### Publish

The `publish` function synchronizes your file system with the Fission API and IPFS. WNFS does not publish changes automatically because it is more practical to batch changes in some cases. For example, a large data set is better published once than over multiple calls to `publish`.

Returns: `CID` the updated _root_ CID for the file system.

{% hint style="warning" %}
**Remember to publish!** If you do not call `publish` after making changes, user data will not be persisted to WNFS.
{% endhint %}

### API Summary

#### Methods

Methods for interacting with the filesystem all use **absolute** paths.

Paths created by [path functions](broken-reference) have a `FilePath` or `DirectoryPath` type. Methods with a `DistinctivePath` param accept either a `FilePath` or a `DirectoryPath`.

The `FileContent`that WNFS can store includes `FileContentRaw`, `Blob`, `string`, `number`, and `boolean`. `FileContentRaw` is `Uint8Array`. In addition, the private file system can store `Object`s.

**add**

Adds file content at a given path**.**

Params:

* path: `FilePath` **required**
* content: `FileContent`**required**

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

Retrieves some file content at a given path**.**

Params:

* path: `FilePath` **required**

Returns: `FileContent`

Example:

```typescript
const content = await fs.cat(wn.path.file("public", "some", "file"))
```

**exists**

Checks if there is anything located at a given path.

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
    wn.path.file("private", "some", "directory", `${name}`)
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

Move a directory or file from one path to another.

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

Removes a file or directory at a given path.

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
* content: `FileContent` **required**

Returns: `CID` the updated _root_ CID for the file system

Example:

```typescript
const content = "hello world"
const updatedCID = await fs.write(
  wn.path.file("private", "some", "file"), 
  content
)
```

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
