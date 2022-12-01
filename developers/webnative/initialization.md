---
description: Creating a Webnative program
---

# Initialization

You can use the Webnative SDK by creating a Webnative program. Here is a minimal example:

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

A Webnative program is an assembly of components that make up a distributed web application.

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

We'll cover each part of a program in upcoming sections. As a quick summary:

* **Session.** A session including a username and a filesystem instance.
* **Authentication Strategy.** A set of functions for registering users and linking devices.
* **Capabilities.** A set of functions for requesting authorization from another app.
* **Components.** Component implementations for authentication strategy, capabilities, crypto, storage, depot, manners, and references.
* **Short Hands.** Helper functions provided for easy access.

## Configuration

You can configure a program with a `namespace` parameter and few optional parameters:

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

We saw a namespace earlier in our minimal example:

```typescript
namespace: { creator: "Nullsoft", name: "Winamp" }
```

A namespace isolates an application's stored resources

### Debug

When `debug` is set to `true`, Webnative will log debugging information to the console. `debug` defaults to `false`.

### Filesystem options

When `loadImmediately` is set to `true`, the filesystem will load immediately on initialization. Loading can be deferred by setting `loadImmediately` to `false`.

In most cases, the filesystem should be loaded immediately. If you would like to load the filesytem in a Worker, you should defer loading at initialization on the main thread.

