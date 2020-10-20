---
description: The Fission CLI account management commands
---

# Managing Your Account

Use the `fission user` command and its subcommands to set up your account and display your username.

## Register a new user

The `fission user register` command registers your account with Fission or links to your existing Fission account.

When you register a new account, you will be prompted for a username and an email. Fission will send you an email to confirm your account and complete your registration.

The `fission user register` command will create a global `config.yaml` file in your `~/.config/fission` directory. See the [Global Fission YAML](https://guide.fission.codes/v/2.9.0/developers/cli/fission-yaml#global-fission-yaml) guide for more information about the `config.yaml` file.

{% hint style="info" %}
The `fission setup` command is a shortcut for `fission user register`.
{% endhint %}

The registration process will also create a [Fission Drive](https://guide.fission.codes/v/2.9.0/drive/preview) for you automatically at the URL `USERNAME.fission.name` using your Fission username.

## Display user information

Use `fission user whoami` to display your username.

