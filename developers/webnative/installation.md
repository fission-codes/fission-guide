---
description: Installing the Webnative SDK
---

# Installation

You can add Webnative to a project by loading it from a CDN or installing it with a package manager.

## CDN

For prototyping or experimentation, you can use the latest version of the Webnative.

```markup
<script src="https://unpkg.com/webnative@latest/dist/index.umd.min.js"></script>
```

We recommend linking to a specific version in production to avoid unexpected changes.

## Package Manager

We recommend installing with a package manager for larger projects that use build tools or bundlers.

Install Webnative with `npm` or your preferred package manager.

```markup
npm install webnative
```

Import Webnative in your application.

```javascript
// ES6
import * as wn from 'webnative'

// Browser/UMD build
const wn = self.webnative
```
