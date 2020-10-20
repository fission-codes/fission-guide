---
description: The Fission Command Line Interface (CLI)
---

# Fission CLI

The Fission CLI is a developer tool for working with apps and managing your account on the Fission platform.

## Structure

The Fission CLI uses multipart commands with the structure:

```text
fission <command> <subcommand> [options]
```

Commands group related subcommands into operations on apps or accounts. Options extend functionality or display help information.

Shortcuts combine a command and subcommand into a shortcut command:

```text
fission <shortcut> [options]
```

The options that would apply to the multipart command also apply to the shortcut.

{% hint style="info" %}
This guide covers the most commonly used options. More options are available for you to explore and you can always [join our Discord](https://discord.gg/daDMAjE) to ask us more about them!
{% endhint %}

## Help

The `--help` option displays a quick reference at any command level.

At the top level, `fission --help` displays a high-level summary of all commands and shortcuts.

```text
$ fission --help
Fission makes developing, deploying, updating, and iterating on web apps quick
and easy.

Usage: fission (SHORTCUT | COMMAND | --version)
  CLI to interact with Fission services

Available options:
  --version                Print version
  -h,--help                Show this help text

Shortcuts
  setup                    Initial Fission setup
  up                       Upload the working directory

Command Groups
  app                      User application management
  user                     Fission account & auth management

Visit https://fission.codes for more information and support
```

