---
description: How to share private data with the Webnative SDK
---

# Sharing Data

You can share a private file or directory with another Fission user:

![](<../../.gitbook/assets/Screenshot 2022-05-09 at 15.28.35.png>)

![](<../../.gitbook/assets/Screenshot 2022-05-09 at 15.24.20.png>) ![](<../../.gitbook/assets/Screenshot 2022-05-09 at 15.24.41.png>)

_Here you see a photo being shared from_ [_Fission Drive_](https://drive.fission.app) _and being accepted by another user in the_ [_auth lobby_](https://auth.fission.codes)_._

#### Exchange keys

Private shared data uses public keys, which we call exchange keys, that live in a user's public filesystem under `.well-known/exchange/`. Exchange keys are unique per domain because webnative generates a key-pair per domain, so we need to add an exchange key to each domain that will accept shares. We can add an exchange key to a filesystem using:

```typescript
// given filesystem `fs`
if (fs.hasPublicExchangeKey() === false) {
  fs.addPublicExchangeKey()
}
```

The auth lobby automatically does this when we create our account, link a device, or authorise an application.

#### Creating a share

When the receiver of the share has their exchange key(s) set up, we're ready to create a share:

```typescript
const shareDetails = fs.sharePrivate(
  [ webnative.path.file() ],
  { shareWith: "username" } // alternative: list of usernames, or exchange DID(s)
)
```

_That will do an `fs.publish()` and consequently a data root update, so you'll want to wait until those are complete before accepting a share._

#### Resolving the share

The `shareDetails` contain a `shareId` used to accept or load a share.

```typescript
fs.acceptShare({
  shareId: shareDetails.shareId,
  sharedBy: shareDetails.sharedBy.username
})
```

By accepting a share, you create a symbolic link (aka. soft link) to the shared data in the `/private/Shared with me/SENDER_USERNAME/` directory. In other words, accepting a share is `fs.loadShare()` plus copying the soft link from the share:

```typescript
const share = await fs.loadShare({ shareId, sharedBy })

// Add soft links to the directory
await fs.add(
  webnative.path.directory(
    webnative.path.Branch.Private,
    "Shared with me",
    sharedBy
  ),
  await share.ls([])
)
```

If you'd like to use the auth lobby to accept, for ease of use like Fission Drive does, then you can create a share link with the share details we got earlier:

```typescript
const link = webnative.lobby.shareLink(shareDetails)
// "https://auth.fission.codes/#/share/sender-username/1/"
```

#### Soft links

When sharing a file or directory with someone, they will receive the updates to those items due to how soft links work. Most filesystem methods work out of the box with soft links, such as `get` and `cat`. But sometimes, you need to resolve them manually, which you can do with `fs.resolveSymlink()`. If you want to create a soft link manually, you can do that with:

```typescript
await fs.symlink({
  at: webnative.path.directory("private", "Documents"),
  referringTo: webnative.path.directory("private", "Photos"),
  name: "Pictures",
  username: "test"
})
```

_⚠️ For now soft links are read-only._
