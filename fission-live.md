---
description: An in-depth look at Fission Live and the accompanying cli tool
---

# Fission Live

Fission Live is the first experience offered through the Fission CLI tool. It makes deploying decentralized websites a breeze and takes care of annoying things like updating DNS and ensuring your content is pinned, so you don't have to!

Check out the [Getting Started](getting-started.md) section to see how the simple interface can help you accomplish some pretty extraordinary things.

## Fission CLI

The Fission CLI has 4 commands:

* `login`
* `register`
* `up`
* `watch`

Details on all of these commands can be seen at any time by running `fission --help`

### Login & Register

Before using Fission services, you need to login. `login` and `register` prompt you for user credentials and store them in `~/.fission.yaml`. 

If you ever need to log into a different account, just delete `~/.fission.yaml` and login again.

### Up

This is the only command you need to get your content hosted on the decentralized web! 

To use `up`, make sure that you:

* have an IPFS daemon running in the background \(through [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop), or in another terminal\) 
* are currently in the directory containing the assets you want to deploy

A few things happen when you run `fission up`:

* Your current directly is recursively added to IPFS through your local node  \(the equivalent of running `ipfs add -r ./`\)
* Your local node connects to a remote Fission node  \(the equivalent of running `ipfs swarm connect [peerId]`\)
* A Pin request is sent to our server which tells the remote Fission node to get and store the requested content directly from your local node
* a request is sent to our server to update the Domain associated with you account to point to the new content using [dnslink](https://docs.ipfs.io/guides/concepts/dnslink/)
  * This points `[username].demo.runfission.com` at our IPFS gateway \([ipfs.runfission.com](https://ipfs.runfission.com/ipfs/Qmaisz6NMhDB51cCvNWa1GMS7LU1pAxdF4Ld6Ft9kZEP2a)\)
  * And `_dnslink.[username].demo.runfission.com` at your uploaded content
  * _Note: It may take some time for DNS to propagate. So give it a minute or two if it doesn't load immediately._

But from your perspective, it's just success messages,  emojis, and a link to your hosted website 🚀

### Watch

If you're currently developing your website and want continuous updates to the hosted version, use `watch` instead of `up`. 

This command does the same thing as `up` but after uploading your content, it continues watching the current directory for changes. Every time you change a file, it adds the new content to IPFS, pins it to the remote Fission node, and updates DNS.

## Using with Gatsby

Fission Live works with any static site, whether you have a single html file, or you're using a static site generator like [Gatsby](https://www.gatsbyjs.org/) or [Jekyll](https://jekyllrb.com/).

Here we'll walk through how to build and deploy a static site using Gatsby, but the process will be the same for any static site.

First, choose a project, and `cd` into the project directory. For this guide, we'll use [reactjs.org](https://github.com/reactjs/reactjs.org) so that anyone can follow along, but if you have another site in mind, use that!

```bash
$ git clone https://github.com/reactjs/reactjs.org
$ cd reactjs.org
```

Install dependencies:

```bash
$ yarn
```

Build the site:

```bash
$ yarn build
```

`cd` into the build folder

```bash
$ cd public
```

Go interplanetary!

```bash
$ fission up
🚀 Now live on the network
👌 QmTurDD2LNBmmrxP2czBmL15415KFoxEXQ8nvJGhLrgJvU
📝 DNS Updated. Check out your site at:
🔗 21ebedd5d4070a521f83.demo.runfission.com
```

Now copy that link from you terminal to your browser to see your site served live from the decentralized web!

