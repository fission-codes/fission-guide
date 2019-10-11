---
description: Install IPFS + Fission and make your machine interplanetary!
---

# Installation

Fission is your toolkit for easily interfacing with [IPFS](https://ipfs.io/). To get started, you'll need to install IPFS as well as the Fission CLI tool.

## Installing IPFS

### ipfs-desktop

If you don't want to mess around with running an IPFS daemon in the command line, [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop) is a great option. You can down load the release from their Github page or use your favorite package manager:

* Homebrew \(macOS\): `brew cask install ipfs` 
* Chocolatey \(Windows\): `choco install ipfs-desktop` 
* Snap \(Linux\): `snap install ipfs-desktop` 

### ipfs binary

We recommend installing IPFS from a prebuilt package. First, [download the correct package for your platform](https://dist.ipfs.io/#go-ipfs).

#### macOS and Linux

Untar the archive and run the `./install.sh` script \(which just 

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

Head over to our [releases](https://github.com/fission-suite/web-api/releases) page on Github and download the latest release for your operating system or grab the latest one here:

* Mac: [v1.9.0](https://github.com/fission-suite/web-api/releases/download/1.9.0/macos-cli)
* Linux: [v.1.9.0](https://github.com/fission-suite/web-api/releases/download/1.9.0/deb-cli)

Then make the binary executable:

```bash
$ cd ~/Downloads
$ sudo chmod +x ./macos-cli
```

And move the file to your PATH:

```bash
$ sudo mv ./macos-cli /usr/local/bin/fission
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

