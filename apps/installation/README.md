---
description: Install the Fission command line tools to start publishing from your desktop
---

# Installation

We build on top of decentralized web technologies like the InterPlanetary File System \(IPFS\) to publish directly from your local development environment without having to learn complicated deployment or DevOps techniques.

Right now you need to install IPFS as well as the Fission command line tool to get up and running, but it should just take a couple of minutes if you follow these instructions.

## Installing IPFS

### ipfs-desktop

If you don't want to mess around with running an IPFS daemon in the command line, [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop) is a great option. You can download the release from their Github page or use your favorite package manager:

* Homebrew \(macOS\): `brew cask install ipfs` 
* Chocolatey \(Windows\): `choco install ipfs-desktop` 
* Snap \(Linux\): `snap install ipfs-desktop` 

{% hint style="info" %}
All of these include graphical interfaces as well as the command line IPFS daemon. You can drag and drop files, view your local IPFS files visually, and even integrate a shortcut for taking screenshots and auto-uploading them to IPFS.
{% endhint %}

### ipfs binary

We recommend installing IPFS from a prebuilt package. First, [download the correct package for your platform](https://dist.ipfs.io/#go-ipfs).

If you are a Mac Homebrew user, `brew install ipfs` is even easier.

#### macOS and Linux

Untar the archive and run the `./install.sh` script \(which just moves the binary to a local bin path\).

```bash
$ tar xvfz go-ipfs.tar.gz
$ cd go-ipfs
$ ./install.sh
```

{% hint style="info" %}
`If you don't know your system's architecture, run`**`uname -a`**
{% endhint %}

#### Windows

Unzip the archive and move `ipfs.exe` to your `%PATH%` .

### Test it out

```bash
$ ipfs help
USAGE:

    ipfs - Global p2p merkle-dag filesystem.
...
```

For more installation options, check out the [Install IPFS guide](https://docs.ipfs.io/guides/guides/install/).

## Installing Fission

Skip to the [dedicated Windows page for Fission](windows.md), or follow Mac and Linux instructions below.

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

Unzip the file, and then make the binary executable:

```bash
$ cd ~/Downloads
$ sudo chmod +x ./fission-cli-exe
```

And move the file to your PATH:

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

