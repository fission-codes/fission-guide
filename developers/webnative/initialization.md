---
description: Creating a Webnative program
---

# Initialization

You can use the Webnative SDK by creating a Webnative program. Here is a minimal example

```typescript
const program = await wn.program({
  // Can also be a string, used as an identifier for caches.
  // If you're developing multiple apps on the same localhost port,
  // make sure these differ.
  namespace: { creator: "Nullsoft", name: "Winamp" }

}).catch(error => {
  switch (error) {
    case webnative.ProgramError.InsecureContext:
      // Webnative requires HTTPS
      break;
    case webnative.ProgramError.UnsupportedBrowser:
      // Browsers must support IndexedDB
      break;
  }

})
```

## Program

A Webnative program is a set of components and utilities for building a distributed web application.

```typescript
export type Program = ShortHands & {
  auth: AuthenticationStrategy
  capabilities: {
    collect: () => Promise<Maybe<string>> // returns username
    request: () => Promise<void>
    session: (username: string) => Promise<Maybe<Session>>
  }
  components: Components
  session: Maybe<Session>
}
```

We'll cover each part of a program in upcoming sections. As a quick summary

* **Session.** A session including a username and a filesystem instance.
* **Authentication Strategy.** A set of functions for registering users and linking devices.
* **Capabilities.** A set of functions for requesting authorization from another app.
* **Components.** Component implementations for authentication strategy, capabilities, crypto, storage, depot, manners, and references.
* **Short Hands.** Helper functions provided for easy access.

## Configuration

You can configure a program with a `namespace` parameter and few optional parameters

```typescript
export type Configuration = {
  namespace: string | AppInfo
  debug?: boolean
  fileSystem?: {
    loadImmediately?: boolean
    version?: string
  }
  permissions?: Permissions
  userMessages?: UserMessages
}
```

### Namespace

We saw a namespace earlier in our minimal example

```typescript
namespace: { creator: "Nullsoft", name: "Winamp" }
```

A namespace isolates an application in browser storage. This means you can work on multiple apps on `localhost` at the same port and not worry about them interacting or conflicting with each other. Provide each app a namespace and they will exist independently.

The namespace can also be set as a string

```typescript
namespace: "nullsoft-winamp"
```

### Debug

The `debug` flag determines whether Webnative should log detailed debugging information to the console. The default value is `false`.

### Filesystem options

The `loadImmediately` flag determines whether the filesystem should be loaded on initialization. The default value is `true`.

The filesystem can be loaded after initialization using the `program.loadFileSystem` or `program.loadRootFileSystem` short hand functions. In most cases, you will want to load the filesystem immediately, but deferring can sometimes be useful. For example, you may want to [load the filesystem in a Worker](additional-info.md#web-worker) after initialization.

The `version` sets the filesystem version. The `version` defaults to the latest stable version of WNFS.

### Permissions

Permissions are the capabilities to be requested from another application. We'll cover permissions in more detail in the [Requesting Capabilities](requesting-capabilities.md) section.

### User messages

User messages are [alerts](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert) that report to the user when their filesystem is not supported by the current version of Webnative. You can update these messages to provide users with your support information.

## Components

A Webnative program can be deeply customized by providing component implementations. We'll discuss custom components in the [Components](initialization.md#components) section.



