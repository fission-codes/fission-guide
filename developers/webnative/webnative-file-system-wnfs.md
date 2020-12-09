# Webnative File System \(WNFS\)

The Webnative File System, or WNFS for short, is the live file system that every Fission user account has.

As a developer, it's also where your app stores files and data.

### Top Level File System Roots

WNFS comes with three separate file systems "roots": public, pretty, and private.

#### Public

Not encrypted. Full metadata support. Starts with `/public`. 

#### Pretty

Not encrypted. No metadata. Represented simply as `/p` to be nice and short when creating public URLs like `/p/path/to/file.img`. It does not support versioning, use the Public or Private trees for that.

#### Private

Encrypted. Structured so that file metadata as well as contents is obscured. Starts with `/private`

### Default folders

Currently we initialize WNFS with a set of default _private_ folders, which should be familiar to people from working with desktop operating systems.

{% hint style="warning" %}
TODO: We'll be documenting and versioning these default folders in the webnative Github repo.
{% endhint %}

Additionally, in apps like Drive or in file pickers, the user sees a top level Public folder, which maps to the Public system root.

{% hint style="info" %}
You can dive deeper by [reading the whitepaper »](https://whitepaper.fission.codes/file-system/sections/root)
{% endhint %}

