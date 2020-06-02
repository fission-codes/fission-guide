---
description: Get your first site up and running on IPFS with this beginner-friendly guide
---

# Getting Started

## Register or Login

Now that you have `fission` installed, you need to register for an account:

```bash
$ fission setup
✅ Registered & logged in
```

And you're good to go! Your credentials are stored in `~/.fission.yaml` 

{% hint style="info" %}
Our default web addresses are based on your username, and look like `USERNAME.fission.name`. In the future, you'll be able to add custom domains.
{% endhint %}

## Run IPFS

You need IPFS running in order to use `fission` 

If you're using [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop), ensure that the application is running.

If you're running `ipfs` from the command line, run the following in a separate terminal:

```bash
$ ipfs daemon
```

## Create a simple webpage

Let's create a simple website and make it interplanetary!

Create a project directory and add an `index.html` file:

```bash
$ mkdir hello-universe
$ cd hello-universe
$ touch index.html
```

Add some content to `index.html`:

{% code title="index.html" %}
```markup
<html>
  <head>
    <title>Hello Universe!</title>
  </head>
  <body>
    <h1>Hello Universe!</h1>
    <p>This is on the InterPlanetary File System.</p>
    <p>Assisted by <a href="https://fission.codes">Fission</a>.</p>
  </body>
</html>
```
{% endcode %}

## Go interplanetary!

Make sure you're in your project folder \(`hello-universe`\) and run:

```bash
$ fission up
🚀 Now live on the network
👌 QmPmZDd6esqzsc2R1i8t5DcRGeRKrNomMhqj232Cz6heNW
📝 DNS Updated. Check out your site at:
🔗 diffusiondemo.fission.name
```

Copy the url from the last line, and paste it into your web browser to see your new site, hosted on the decentralized web. It's that easy!

_Note: It may take some time for DNS to propagate. So give it a minute or two if it doesn't load immediately. If you're impatient you can also take the `content-id` from the CLI output and view it here at `https://ipfs.runfission.com/ipfs/{YOUR_CONTENT_ID}`_

{% hint style="success" %}
Fun IPFS Fact: If everyone follows this tutorial exactly you will end up with identical Content ID's. This means you can instantly pull the demo website from your neighbour if they're a step ahead of you.
{% endhint %}

