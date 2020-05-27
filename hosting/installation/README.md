---
description: Install the Fission command line tools to start publishing from your desktop
---

# Installation

You can sign up for Fission and get setup to start using it directly from your command line.

## Installing the Fission CLI

The Fission command line interface \(CLI\) is the main way to interact with Fission's services.

{% hint style="info" %}
For Windows users, we currently recommend using the Windows Subsystem for Linux \(WSL\). The Linux instructions below apply equally to WSL, except where noted.
{% endhint %}

#### MacOS

We have a brew recipe to get you up and running quickly on MacOS:

```bash
$ brew install fission-suite/fission/fission-cli
```

#### Linux \(and manual MacOS\)

Head over to our [releases](https://github.com/fission-suite/cli/releases) page on Github and download the latest release for your operating system.

{% hint style="warning" %}
Note: Ubuntu 19+ is currently not supported due to [a build issue \#51](https://github.com/fission-suite/cli/issues/51)
{% endhint %}

Unzip the file and move the file to your PATH:

```bash
$ sudo mv ./fission-cli-exe /usr/local/bin/fission
```

That's it! Double check that it's installed correctly:

```bash
$ fission --help
CLI to interact with Fission services

Usage: fission [--version] [--help] COMMAND
  Fission makes developing, deploying, updating and iterating on web
  applications quick and easy.
...
```

