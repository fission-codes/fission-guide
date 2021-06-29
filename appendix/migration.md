---
description: Migration guides for new versions of the CLI and webnative
---

# Migration

## Webnative

#### Version 0.24

The way paths are used throughout the webnative and filesystem APIs has changed.

In earlier versions of webnative, API calls expected UNIX style paths.

```text
const bool = await fs.exists("private/some/file")
const updatedCID = await fs.mkdir("public/some/directory")
```

In webnative 0.24, paths are created by [path helper functions](../developers/webnative/#paths) for files and directories.

```text
const bool = await fs.exists(wn.path.file("private", "some", "file"))
const updatedCID = await fs.mkdir(wn.path.directory("public", "some", "directory"))
```

The docs for the [previous API](https://guide.fission.codes/v/en-2.9.0-0.23/developers/webnative) remain available for reference.

#### Version 0.26

The URL for loading webnative from UNPKG has changed. In previous versions, webnative was available as `index.umd.js`.

```markup
<script src="https://unpkg.com/webnative@latest/dist/index.umd.js"></script>
```

In webnative 0.26, webnative is available as `index.min.js`.

```markup
<script src="https://unpkg.com/webnative@0.26.0/dist/index.min.js"></script>
```

