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

Head over to our [releases](https://github.com/fission-suite/fission/releases) page on Github and download the latest release for your operating system.

Unzip the file and move the file to your PATH:

```bash
$ sudo mv ./fission-cli-exe /usr/local/bin/fission
```

We currently depend on two additional libraries `libpq.so.5` and `libtinfo.so.5`. For apt-based Linux distros, you'll want to run the following:

```bash
$ apt install libpq-dev libtinfo5
```

Depending on your OS version, you may need to run `apt-get update` first, and you may need to run these commands with `sudo`.

That's it! Double check that it's installed correctly:

```bash
$ fission --help
Fission makes developing, deploying, updating, and iterating on web apps quick
and easy.

Usage: fission (SHORTCUT | COMMAND | --version)
  CLI to interact with Fission services
...
```

## Upgrading the CLI

To upgrade the CLI on macOS, `brew uninstall` and `brew untap` to reset `brew`.

```text
brew uninstall fission-cli
brew untap fission-suite/fission
```

Reinstall with `brew tap` and `brew install`.

```text
brew tap fission-suite/fission
brew install fission-cli
```

On Linux, repeat the [installation steps](installation.md#installing-the-fission-cli) listed above, leaving out the installation of `libpq.so.5` and `libtinfo.so.5`.

