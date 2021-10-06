---
description: Install the Fission command line tools to start publishing from your desktop.
---

# Installation

## Installing the Fission CLI

The Fission command line interface \(CLI\) is the most common way for developers to interact with Fission services.

#### macOS with Homebrew

Use the Homebrew recipe to install the CLI on macOS. This taps and installs in one command:

```bash
brew install fission-suite/fission/fission-cli
```

You can also follow the next section for a manual install on macOS.

#### Linux and Manual Installation

{% hint style="info" %}
For Windows users, we currently recommend using Windows Subsystem for Linux 2 \(WSL2\). WSL1 is not supported. Run these Linux / Manual install instructions in your WSL2 environment.
{% endhint %}

Head over to our [releases](https://github.com/fission-suite/fission/releases) page on Github and download the latest release for your operating system.

Grant execute permissions and move the binary onto to your PATH. For example, on Ubuntu 20.04:

```bash
chmod a+x ./fission-cli-ubuntu-20.04
sudo mv ./fission-cli-ubuntu-20.04 /usr/local/bin/fission
```

That's it! Double check that it's installed correctly.

```bash
$ fission --help
Fission makes developing, deploying, updating, and iterating on web apps quick
and easy.

Usage: fission (SHORTCUT | COMMAND | --version) [-v|--verbose]
  CLI to interact with Fission services
```

If you run into any problems, check that you have `libssl1.1` \(installed with OpenSSL\) and `libtinfo5` \(or `libtinfo6`\). Most recent Linux distributions will already have these libraries installed.

On macOS, you will also need `libcrypto1.1` installed and linked on your system, which should be available from OpenSSL.

##  Upgrading the CLI

Run `fission --version` to check if you are using an old version of the CLI.

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

On Linux, repeat the [installation steps](installation.md#installing-the-fission-cli) listed above, leaving out the installation of the additional libraries.

