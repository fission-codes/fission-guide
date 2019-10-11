---
description: Get your first site up and running on IPFS with this beginner-friendly guide
---

# Getting Started

## Register or Login

Now that you have `fission` installed, you need to register for an account:

```bash
$ fission register
✅ Registered & logged in
```

And you're good to go! Your credentials are stored in `~/.fission.yaml` 

If you already have an account, you can login instead:

```bash
$ fission login
Username: # follow the prompts to provide your username and password
```

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

{% code-tabs %}
{% code-tabs-item title="index.html" %}
```markup
<html>
  <head>
    <title>Hello Universe!</title>
  </head>
  <body>
    <h1>Hello Universe!</h1>
  </body>
</html>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Go interplanetary!

Make sure you're in your project folder \(`hello-universe`\) and run:

```bash
$ fission up
🚀 Now live on the network
👌 QmXGsrriZWi9j24CKKj9FY7NxghRMqAHhhjMgc2peRhx61
📝 DNS Updated. Check out your site at:
🔗 21ebedd5d4070a521f83.demo.runfission.com
```

Copy the url from the last line, and paste it into your web browser to see your new site, hosted on the decentralized web. It's that easy!

_Note: It may take some time for DNS to propagate. So give it a minute or two if it doesn't load immediately._

