---
description: >-
  Installation instructions for IPFS and Fission on Windows, including Windows
  Subsystem for Linux and native Windows exe
---

# Windows

## Native Windows Executable

{% hint style="success" %}
We are currently in the alpha stage of development and expect to have a native executable ready shortly.  

In the meantime, continue reading to learn how to access Fission CLI using WSL today!
{% endhint %}

## Fission on Windows Subsystem for Linux \(WSL\)

WSL on Windows 10 offers an easy way to get started with Fission CLI.

{% hint style="warning" %}
To differentiate between operating system commands:

* `>` indicates Windows
* `$` indicates Linux
{% endhint %}

### Enabling WSL

WSL is disabled by default so if you have never used it before, the first step will be enabling the optional feature.  Open PowerShell as Administrator and enter the following:

```bash
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

After restarting your computer, you'll be ready for the next step.

### Adding a Linux Distribution

Before using WSL, you need to add a Linux distribution.  Run PowerShell as Administrator and enter the following command: 

```bash
> choco install wsl-ubuntu-1804
> wsl --list
```

If the installation was successful then Ubuntu should be listed.

{% hint style="info" %}
If you have multiple distributions installed, the above command \(`wsl --list`\) will display your default distribution. It's important to know which one you're working in because **each distribution is completely isolated.**
{% endhint %}

### Installing IPFS

We recommend installing IPFS from a prebuilt package. First, [download the correct **Linux** package for your platform](https://dist.ipfs.io/#go-ipfs).

Enter WSL, untar the archive, and run the `./install.sh` script \(which just moves the binary to your local Linux bin path\).

```bash
> wsl
$ tar xvfz go-ipfs.tar.gz
$ cd go-ipfs
$ ./install.sh
```

#### Test it out

```bash
$ ipfs help
USAGE:

    ipfs - Global p2p merkle-dag filesystem.
...
```

For more installation options, check out the [Install IPFS guide](https://docs.ipfs.io/guides/guides/install/).

### Installing Fission

Head over to our [releases](https://github.com/fission-suite/cli/releases) page on Github and download the latest release for your operating system.

Unzip the file into your Windows Downloads folder then open PowerShell as Administrator and run the following commands:

```bash
> cd ~\Downloads
> wsl
$ pwd
```

The output of `pwd` should show`/mnt/c/Users/<username>/Downloads` which is the mounting point of your Windows Downloads folder in Linux.  Now you can move the binary to your Linux PATH:

```bash
$ mv ./fission-cli-exe /usr/local/bin/fission
```

Linux requires an additional dependency:

```bash
$ apt update
$ apt install libpq-dev
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

