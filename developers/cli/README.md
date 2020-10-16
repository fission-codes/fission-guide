---
description: The Fission Command Line Interface (CLI)
---

# Fission CLI

The Fission CLI has the following commands:

* `setup`
* `whoami`
* `up`
* `down`
* `app-init`
* `watch`

Details on all of these commands can be seen at any time by running `fission --help`

```bash
CLI to interact with Fission services

Usage: fission [--version] [--help] COMMAND
  Fission makes developing, deploying, updating, and iterating on web apps quick
  and easy.

Available options:
  --version                Show version
  --help                   Show this help text

Available commands:
  setup                    Setup Fission on your machine
  whoami                   Check the current user
  up                       Keep your current working directory up
  down                     Pull data down to your system
  app-init                 Initialize a new Fission app in an existing directory
  watch                    Keep your working directory in sync with the IPFS
                           network
```

### setup

Setup is used to get Fission installed and linked to your machine. This is both for new registrations, and for linking an existing account to a new machine.

You'll be asked to pick a unique username and enter your email to use the Fission services. Your username also gives you a `USERNAME.fission.name` web address where your Fission Drive is available.

### whoami

Use this to see what username account is connected to your current user.

Mostly used for debugging, now that multi-app support is included per account.

### up

This is the only command you need to get your content hosted on the decentralized web!

To use `up`, make sure that you:

* have an IPFS daemon running in the background \(through [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop), or in another terminal\) 
* are currently in the directory containing the assets you want to deploy

A few things happen when you run `fission up`:

* the directory is recursively added to IPFS through your local node  \(the equivalent of running `ipfs add -r ./`\)
* Your local node connects to a remote Fission node  \(the equivalent of running `ipfs swarm connect [peerId]`\)
* A Pin Request is sent to our server which tells the remote Fission node to get and store the requested content directly from your local node
* a request is sent to our server to update the Domain associated with you account to point to the new content using dnslink ℹ 
  * This points `[appname].fission.app` at our IPFS gateway \([ipfs.runfission.com](https://ipfs.runfission.com/ipfs/Qmaisz6NMhDB51cCvNWa1GMS7LU1pAxdF4Ld6Ft9kZEP2a)\)
  * And `_dnslink.[appname].fission.app` at your uploaded content
  * _Note: It may take some time for DNS to propagate. So give it a minute or two if it doesn't load immediately._

But from your perspective, it's just success messages, emojis, and a link to your hosted website 🚀

### down

Enter in an IPFS hash to download the files to your local machine.

### app-init

Running `fission app-init` will create a new app linked to your account. We will generate a name for the app, which also gives you an `APPNAME.fission.app` subdomain.

A `fission.yaml` is created in the local folder which lists the app name and the URL where it is available. You can also add [Custom Domains](../custom-domains/).

Running fission `up` or `watch` will publish the files to the app listed in the local YAML file.

You can create as many apps as you like.

### watch

If you're currently developing your website and want continuous updates to the hosted version, use `watch` instead of `up`.

This command does the same thing as `up` but after uploading your content, it continues watching the current directory for changes. Every time you change a file, it adds the new content to IPFS, pins it to the remote Fission node, and updates DNS.

## Additional Settings

There are a few advanced settings and configuration that can be set by your local environment.

### DEBUG

To get more details on what's happening, you can turn DEBUG to true like so:

```text
DEBUG=true fission up
```

### IPFS\_PATH

By default, the fission-cli assumes that IPFS is installed at `/usr/local/bin`. You can call the CLI with a custom setting or permanently set a shell variable:

```text
IPFS_PATH=/Snap/ipfs fission up
```

