---
description: Get your first site up and running with this beginner-friendly guide
---

# Getting Started

## Setup

With `fission` installed, we are ready to set it up on your local machine. If this is your first time using `fission`, you will be prompted for a username and an email to confirm your new account.

```text
$ fission setup
🌱 Setting up environment
🪐 Downloading managed IPFS for Linux
🎛️  Configuring managed IPFS
generating ED25519 keypair...done
peer identity: 12D3KooWHZ3C1tDmzUhgi3A6LYmgBxvv1E1obtG7S2ZkYM1MN83
initializing IPFS node at /home/yourname/.config/fission/ipfs
🔑 Creating keys
🏠 Do you have an existing account? [Y/n] n
Username: YOURNAME
Email: yourname@example.com
✅ Registration successful! Head over to your email to confirm your account.
🎛️  Initializing user config file
✅ Done! Welcome to Fission, YOURNAME ✨.                                                                                             
```

A private key has been generated for your machine and saved in the `~/.config/fission/key/` directory. This key secures your communication with Fission services and works like using an SSH key to connect to GitHub.

{% hint style="warning" %}
The current version of the fission-cli automatically downloads and runs the ipfs peer-to-peer software that makes for fast publishing. We have a known issue where if you already have ipfs running on your system, there is a conflict. We have a fix in an upcoming release, for now please turn off your local ipfs services with `brew services stop ipfs` , quit IPFS desktop, or otherwise follow your local OS instructions.
{% endhint %}

### Account Linking

If you already have an account created in a web browser, you can link it on your local machine. See the [Fission CLI linking guide](cli/managing-your-account.md#linking-an-existing-user) to link your account.

## Create a simple webpage

Let's publish a simple webpage with Fission! We will write a simple webpage, register it as a Fission app, and publish it to the web.

Create a project directory and an `index.html` file.

```bash
mkdir hello-universe
cd hello-universe
touch index.html
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

Run `fission app register` command to initialize the webpage as a Fission app. You will be prompted for a build directory.

```text
$ fission app register
👷 Build directory (.):
```

Press enter to set `hello-universe` as your build directory, and `fission` will initialize your app.

```text
✅ App initialized as big-narrow-fuchsia-elf.fission.app
⏯️  Next run fission app publish or fission app publish --watch to sync data
💁 It may take DNS time to propagate this initial setup globally. 
In this case, you can always view your app at 
https://ipfs.runfission.com/ipns/big-narrow-fuchsia-elf.fission.app
```

Fission will create a `fission.yaml` file with your app configuration.

{% code title="fission.yaml" %}
```yaml
ignore: []
url: big-narrow-fuchsia-elf.fission.app
build: .
```
{% endcode %}

The `url` is where your webpage will be on the web after it has been published, `build` tells the fission which directory to publish, and `ignore` is a list of files and directories to ignore. See the [Fission YAML guide ](https://guide.fission.codes/hosting/cli/fission-yaml)for more details.

Run `fission app publish` to publish the webpage.

```text
$ fission app publish
🚀 Now live on the network
📝 DNS updated! Check out your site at: 
🔗 big-narrow-fuchsia-elf.fission.app
```

Copy the URL from the last line and paste it into your web browser to view the webpage live on the web. Nice and simple!

{% hint style="warning" %}
It may take a few minutes for DNS to propagate. If you don't see the webpage live at the URL, come back and try again in a bit. You can also take the URL from the CLI output and view the webpage at`https://ipfs.runfission.com/ipfs/{URL}`right away.
{% endhint %}

