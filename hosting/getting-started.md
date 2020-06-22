---
description: Get your first site up and running with this beginner-friendly guide
---

# Getting Started

## Setup

Now that you have `fission` installed, you need to set it up for your local machine. If this is your first time using fission or you are setting up a new machine, you'll want to run setup:

```bash
$ fission setup
Username: YOURNAME
Email: yourname@example.com
✅ Registration successful!
```

And you're good to go! A private key has been generated for your machine, and placed in your local `~/config/fission` folder. This secures communication with the Fission services, and works similarly to how using an SSH key to connect to GitHub works.

{% hint style="info" %}
Your first web address is based on your username, and looks like `USERNAME.fission.name`. You can add custom apps at `YOURAPP.fission.app`, and also add custom domains.
{% endhint %}

### Account Linking

If you already have an account, you'll want to link it to your local system. Just enter in your current username and [follow the linking flow](account-linking.md).

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
    <p>This is a Fission powered page.</p>
    <p>Assisted by <a href="https://fission.codes">Fission</a>.</p>
  </body>
</html>
```
{% endcode %}

## Run IPFS

You need IPFS running in order to use `fission` 

If you're using [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop), ensure that the application is running.

If you're running `ipfs` from the command line, run the following in a separate terminal:

```bash
$ ipfs daemon
```

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

