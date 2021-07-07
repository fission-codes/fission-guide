---
description: Paths in the Webnative SDK
---

# Paths

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

