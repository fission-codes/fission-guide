---
description: "Become a webnative jedi \U0001F9D8"
---

# Additional information

## Lobby

### Authorization

The auth lobby grants apps authorization to access WNFS. Apps request permission to use App Storage and additional public and private directories. The auth lobby creates a User Controlled Authorization Networks \(UCAN\) token that reflects the requested permissions.

Webnative checks the UCAN at initialization and returns an auth scenario.

* **AuthSucceded.** The user has just returned from the auth lobby, and they granted the requested permissions.
* **Continuation.** The user has already granted permission, and the UCAN has not expired.
* **AuthCancelled.** The user denied the requested permissions.
* **NotAuthorised.** The user has not granted permission yet or the UCAN has expired. 

UCANs expire and users must periodically re-authorize apps through the auth lobby. All user data is preserved in WNFS across authorizations, including the data stored in App Storage.

{% hint style="info" %}
**More on UCANs.** Read more about UCANs in our [UCAN: Authorizing Users Without a Back End](https://blog.fission.codes/auth-without-backend/) blog post and in the [Fission Whitepaper](https://whitepaper.fission.codes/access-control/ucan/ucan-tokens).
{% endhint %}

### Shared devices

In most cases, users will not need to log out of an app or a device. Read more about this part of our vision for Fission-enabled apps in the [What does “log in” or “log out” mean for the Fission SDK and apps?](https://talk.fission.codes/t/what-does-log-in-or-log-out-mean-for-the-fission-sdk-and-apps/919) blog post.

One case where logging out is desirable is on a shared device. Logging out on a shared device can be accomplished in two steps:

* Remove the UCAN token from apps that were granted permissions with the webnative`wn.leave` function
* Log out from the auth lobby on the reset page \([https://auth.fission.codes/reset/](https://auth.fission.codes/reset/)\)

## File System

### Permissions

Every file system action checks if an app has received sufficient permissions from the user. Apps request `permissions` when they initialize webnative, and the [auth lobby grants authorization](additional-info.md#authorization).

Apps request permission for App Storage, additional private and public directories, and user apps published with the Platform APIs. For example, a notes app might request these permissions.

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

The app would have access to its App Storage and public and private Notes directories. 

{% hint style="info" %}
**Platform permissions.** The platform permissions could be left out of this example because this app will not need them. See the [Platform API guide](platform.md#permissions) for more information on working with user apps.
{% endhint %}

The initialize function will return a `NotAuthorised` scenario if one of the UCAN will expire in one day to minimize the likelihood of receiving an expired permissions error. But to be safe, apps should also account for this error.

### Web Worker

Can I use my file system in a web worker?  
Yes, this only requires a slightly different setup.

```typescript
// UI thread
// `state.fs` will now be `null`
const { permissions } = wn.initialise({ loadFileSystem: false })
worker.postMessage({ tag: "LOAD_FS", permissions })

// Web Worker
let fs

self.onMessage = async event => {
  switch (event.data.tag) {
    case "LOAD_FS":
      fs = await wn.loadFileSystem(event.data.permissions)
      break;
  }
}
```

### Versions

Since the file system may evolve over time, a "version" is associated with each node in the file system \(tracked with semver\).

Currently two versions exist:

* `1.0.0`: file tree with metadata. Nodes in the file tree are structured as 2 layers where one layer contains "header" information \(metadata, cache, etc\), and the second layer contains data or links. **This is the default version, use this unless you have a good reason not to**.
* `0.0.0`: legacy bare file tree of an early version.

## Debugging

### Version

Check the webnative version.

```javascript
console.log(wn.VERSION)
```

### Debug Logs

Add the following to your code to enable webnative debug logging.

```javascript
wn.setup.debug({ enabled: true })
```

When webnative makes a change to the user's filesystem, it logs:

```text
📓 Adding to the CID ledger
```

Next, webnative links the change to make it available across the web.

```text
🌊 Updating your DNSLink
```

When linking completes, the change is published and available to other browsers.

```text
🪴 DNSLink updated:
```

