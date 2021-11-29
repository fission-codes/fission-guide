---
description: Migrate your file system to get the latest features
---

# Account Migration

Fission will occasionally change how your file system is stored to enable new features, improve performance, and improve security. Some of these changes require a file system migration.

Because Fission has no control over your file system, we cannot migrate your file system for you. To help you with migrations, we've developed a command-line tool that upgrades your file system to the latest version.

### When to migrate your file system

If your file system is outdated, you will see a message like this in the Auth Lobby ([https://auth.fission.codes](https://auth.fission.codes)):

![A popup message indicating that you need to migrate](<../../.gitbook/assets/Bildschirmfoto vom 2021-11-23 10-06-46.png>)

If you decide that your username and data are not worth keeping, go to [https://auth.fission.codes](https://auth.fission.codes) and click the text "remove this device" on the bottom of the page. Keep in mind that you will not be able to access data associated with the account afterward.

After removing a device, you will be able to create a new account. New accounts are always created at the latest file system version.

{% hint style="info" %}
**Multiple devices.** If you have signed in on multiple devices, remove each device and link them again after you have created a new account.
{% endhint %}

### Running a migration

At the moment, running a migration requires familiarity working with the command line on your machine.

#### Step 1: [Install the Fission CLI](../../developers/installation.md#installing-the-fission-cli)

#### Step 2: [Link your web account to your command line](../../developers/cli/managing-your-account.md#linking-from-a-web-account)

The Webnative File System (WNFS) migration tool will use your linked credentials when migrating your file system.

#### Step 3: [Install NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) following the instructions for your operating system

#### Step 4: Install `wnfs-migration`

You can now install the migration CLI tool with npm:

`npm install --global wnfs-migration`

#### Step 5: Run `wnfs-migration`

{% hint style="info" %}
Make sure the output of `npm bin` is on your PATH. If it is not, you can prepend your command with the path returned from `npm bin`, for example: `$` `/home/me/node_modules/bin/wnfs-migration`
{% endhint %}

```shell-session
$ wnfs-migration
Looking up data root for [...]
Data root is bafybei[...]
Loading IPFS...
Connected to local ipfs node, version 0.9.0
Connected to the Fission IPFS Cluster
Looking up your filesystem version (https://[...].files.fission.name/version)
Your filesystem currently is at version 1.0.0
Processing public/.well-known
Processing public/.well-known/exchange
[...]
Processing private/Documents
Finished migration: bafybei[...]
? Are you sure you want to overwrite your filesystem with a migrated version? (Y/n)
```

At this point, `wnfs-migration` has migrated a local copy of your WNFS. It will then ask you to confirm that you want to overwrite the synced version of your file system on the Fission servers.

Enter `Y` to confirm or `n` to decline and abort the migration process. If you confirm, `wnfs-migration` will sync your migrated file system:

```shell-session
Created authorization UCAN. Updating data root... # this will take some time
Migration done!
Shutting down IPFS...
```
