---
description: Install the Fission command line tools to start publishing from your desktop
---

# Installation

We build on top of decentralized web technologies like the InterPlanetary File System \(IPFS\) to publish directly from your local development environment without having to learn complicated deployment or DevOps techniques.

Right now you need to install IPFS as well as the Fission command line tool to get up and running, but it should just take a couple of minutes if you follow these instructions.

## Installing IPFS

### ipfs-desktop

[ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop) is a great option for a graphical interface, including options to run ipfs as a service on system start, as well as easy access to start and stop the ipfs daemon.

You can download the release from their Github page or use your favorite package manager:

* Homebrew \(macOS\): `brew cask install ipfs` 
* Chocolatey \(Windows\): `choco install ipfs-desktop` 
* Snap \(Linux\): `snap install ipfs-desktop` 

{% hint style="info" %}
The command line ipfs daemon is also included as part of this install.
{% endhint %}

### Command Line IPFS

#### MacOS

If you're not running ipfs-desktop, install ipfs via brew:

```bash
brew install ipfs
```

To run ipfs as a background service:

```bash
brew services start ipfs
```

#### Linux and Windows / WSL

Download the Linux binary from the [IPFS distributions page](https://dist.ipfs.io/#go-ipfs).

Untar the archive and run the `./install.sh` script \(which just moves the binary to a local bin path\).

```bash
$ tar xvfz go-ipfs.tar.gz
$ cd go-ipfs
$ ./install.sh
```

### All Systems

For all systems, IPFS should now be installed. Initialize your IPFS repo:

```bash
ipfs init
```

By default the config and files are stored in your home directory in the`.ipfs` directory.

ipfs-desktop can be turned on and off graphically. For Linux systems, run the daemon in the background:

```bash
ipfs daemon &
```

If you would like to be able to easily start and stop the ipfs daemon, see the [Troubleshooting page](../appendix/troubleshooting.md#initd).

## Installing the Fission CLI

The Fission command line interface \(CLI\) is the main way to interact with Fission's services.

{% hint style="info" %}
For Windows users, we currently recommend using the Windows Subsystem for Linux \(WSL\). The Linux instructions below apply equally to WSL, except where noted.
{% endhint %}

#### MacOS

We have a brew recipe to get you up and running quickly on MacOS:

```bash
$ brew tap fission-suite/fission
$ brew install fission-cli
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

_**\(Linux Only\)**_  
****Linux requires an additional dependency:

```bash
$ sudo apt update
$ sudo apt install libpq-dev
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

