---
description: The Fission Command Line Interface (CLI)
---

# Fission CLI

The Fission CLI has the following commands:

* `login`
* `logout`
* `register`
* `reset-password`
* `up`
* `down`
* `watch`
* `whoami`

Details on all of these commands can be seen at any time by running `fission --help`

```bash
Usage: fission [--version] [--help] COMMAND
  Fission makes developing, deploying, updating and iterating on web
  applications quick and easy.

Available options:
  --version                Show version
  --help                   Show this help text

Available commands:
  login                    Add your Fission credentials
  logout                   Logout of your Fission account
  register                 Register for Fission and login
  reset-password           Reset Fission Password
  up                       Keep your current working directory up
  down                     Pull a ipfs or ipns object down to your system
  watch                    Keep your working directory in sync with the IPFS
                           network
  whoami                   Check the current user
```

### Login & Register

Before using Fission services, you need to login. `login` and `register` prompt you for user credentials and store them in `~/.fission.yaml`. 

If you ever need to log into a different account, just delete `~/.fission.yaml` and login again.

### Up

This is the only command you need to get your content hosted on the decentralized web! 

To use `up`, make sure that you:

* have an IPFS daemon running in the background \(through [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop), or in another terminal\) 
* are currently in the directory containing the assets you want to deploy

A few things happen when you run `fission up`:

* the directory is recursively added to IPFS through your local node  \(the equivalent of running `ipfs add -r ./`\)
* Your local node connects to a remote Fission node  \(the equivalent of running `ipfs swarm connect [peerId]`\)
* A Pin Request is sent to our server which tells the remote Fission node to get and store the requested content directly from your local node
* a request is sent to our server to update the Domain associated with you account to point to the new content using dnslink ℹ 
  * This points `[username].demo.runfission.com` at our IPFS gateway \([ipfs.runfission.com](https://ipfs.runfission.com/ipfs/Qmaisz6NMhDB51cCvNWa1GMS7LU1pAxdF4Ld6Ft9kZEP2a)\)
  * And `_dnslink.[username].demo.runfission.com` at your uploaded content
  * _Note: It may take some time for DNS to propagate. So give it a minute or two if it doesn't load immediately._

But from your perspective, it's just success messages,  emojis, and a link to your hosted website 🚀

### Down



### Watch

If you're currently developing your website and want continuous updates to the hosted version, use `watch` instead of `up`. 

This command does the same thing as `up` but after uploading your content, it continues watching the current directory for changes. Every time you change a file, it adds the new content to IPFS, pins it to the remote Fission node, and updates DNS.

