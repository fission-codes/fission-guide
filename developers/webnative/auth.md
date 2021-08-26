---
description: Authentication and Authorization with the Webnative SDK
---

# Auth

## Authentication

Users authenticate once per browser in the [Fission Auth Lobby](https://auth.fission.codes/). If the user is new to Fission, they are prompted to sign up. They may also link an existing account from another browser.

Apps redirect users to the Auth Lobby to sign up and request authorization to use resources. Sign-up only happens once, and each subsequent visit from any app is for authorization only.

{% hint style="info" %}
Auth in webnative looks similar to an OAuth authentication flow but with an important difference. Webnative stores user credentials in the browser, and authentication through a third-party is not necessary. Private credentials are stored as [WebCrypto](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) `CryptoKey`s to keep them secure. See the [Login section](https://whitepaper.fission.codes/accounts/login) of the Fission Whitepaper for more details.
{% endhint %}

## Authorization

The Auth Lobby grants apps authorization to access WNFS. Apps request permission to use app-specific storage and additional public and private directories. The Auth Lobby creates a User Controlled Authorization Networks \(UCAN\) token that reflects the requested permissions.

Webnative checks the UCAN at initialization and returns an auth scenario.

* **AuthSucceded.** The user has just returned from the Auth Lobby, and they granted the requested permissions.
* **Continuation.** The user has already granted permission, and the UCAN has not expired.
* **AuthCancelled.** The user denied the requested permissions.
* **NotAuthorised.** The user has not granted permission yet or the UCAN has expired. 

UCANs expire and users must periodically re-authorize apps through the Auth Lobby. All user data is preserved in WNFS across authorizations, including the data stored in App Storage.

{% hint style="info" %}
**More on UCANs.** Read more about UCANs in our [UCAN: Authorizing Users Without a Back End](https://blog.fission.codes/auth-without-backend/) blog post and in the [Fission Whitepaper](https://whitepaper.fission.codes/access-control/ucan#overview).
{% endhint %}

## Initialization

Apps intialize webnative with [file system](file-system-wnfs.md#permissions) and [platform API](platform.md#permissions) permissions. The  `redirectToLobby` method redirects users to the Auth Lobby, where users grant or deny permission to use their resources. `redirectToLobby` takes an optional parameter, a URL that the lobby should redirect back to \(the default is `location.href`\).

```javascript
import * as wn from 'webnative'

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

## Shared Devices

In most cases, users will not need to log out of an app or a device. Read more about this part of our vision for Fission-enabled apps in the [What does “log in” or “log out” mean for the Fission SDK and apps?](https://talk.fission.codes/t/what-does-log-in-or-log-out-mean-for-the-fission-sdk-and-apps/919) forum post.

One case where logging out is desirable is on a shared device. Logging out on a shared device can be accomplished in two steps:

* Remove the UCAN token from apps that were granted permissions with the webnative`wn.leave` function
* Log out from the Auth Lobby on the reset page \([https://auth.fission.codes/reset/](https://auth.fission.codes/reset/)\)

