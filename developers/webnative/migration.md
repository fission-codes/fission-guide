---
description: Migration guides for new versions of the webnative SDK
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

In webnative 0.24, paths are created by [path helper functions](./#paths) for files and directories.

```text
const bool = await fs.exists(wn.path.file("private", "some", "file"))
const updatedCID = await fs.mkdir(wn.path.directory("public", "some", "directory"))
```

The docs for the [previous API](https://guide.fission.codes/v/en-2.9.0-0.23/developers/webnative) remain available for reference.

#### Version 0.26.0

The URL for loading webnative from UNPKG has changed. In previous versions, webnative was available as `index.umd.js`.

```markup
<script src="https://unpkg.com/webnative@latest/dist/index.umd.js"></script>
```

In webnative 0.26, webnative is available as `index.min.js`.

```markup
<script src="https://unpkg.com/webnative@0.26.0/dist/index.min.js"></script>
```

#### Version 0.26.1

The URL for loading webnative from UNPKG has changed. In webnative 0.26.0, webnative was available as `index.min.js`.

```markup
<script src="https://unpkg.com/webnative@0.26.0/dist/index.min.js"></script>
```

We found that some projects needed the UMD build and brought it back in webnative 0.26.1. We have replaced `index.min.js` with `index.umd.min.js`.

```markup
<script src="https://unpkg.com/webnative@0.26.1/dist/index.umd.min.js"></script>
```

#### Version 0.27.0

The type of `App` returned `app.index` has changed. Previously, the return type was

```javascript
export type App = {
  domain: string
}
```

In webnative 0.27.0, the the return type is

```javascript
export type App = {
  domains: string[]
  insertedAt: string
  modifiedAt: string
}
```

The domain for the app is now in the `domains` array.

