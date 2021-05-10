---
description: The Fission CLI account management commands
---

# Managing Your Account

Use `fission setup` and`fission user whoami` to set up your account and display your username.

## Register a new user

The `fission setup` command registers your account with Fission or links to your existing Fission account.

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

When you register a new account, you will be prompted for a username and an email. Fission will send you an email to confirm your account and complete your registration.

The `fission setup` command will create a global `config.yaml` file in your `~/.config/fission` directory. See the [Global Fission YAML](fission-yaml.md#global-fission-yaml) guide for more information about the `config.yaml` file.

The registration process will also create a [Fission Drive](../../drive/preview.md) for you automatically at the URL `YOURNAME.fission.name` using your Fission username.

## Linking from a web account

If you are signed into a Fission account in a web browser, you can link your account and use it at the command line.

Open the [Fission auth lobby](https://auth.fission.codes/)  in a browser window.

![](../../.gitbook/assets/lobby.png)

Use `fission setup` to create and link a CLI user at your command line.

```text
$ fission setup
🌱 Setting up environment
🪐 Downloading managed IPFS for Linux
🎛️  Configuring managed IPFS
generating ED25519 keypair...done
peer identity: 12D3KooWHZ3C1tDmzUhgi3A6LYmgBxvv1E1obtG7S2ZkYM1MN83
initializing IPFS node at /home/thuselem/.config/fission/ipfs
🔑 Creating keys
🏠 Do you have an existing account? [Y/n] Y
🔗 Please open auth.fission.codes on a signed-in device
📛 Please enter your username: yeti
```

When prompted, enter `Y` to confirm you have an account and enter your username from the browser window. The auth lobby and CLI will display a confirmation code.

![](../../.gitbook/assets/confirmation.png)

When you have confirmed that the codes match, approve in the browser, and the CLI will complete the setup process and link your account. 

```text
$ fission setup
🌱 Setting up environment
🪐 Downloading managed IPFS for Linux
🎛️  Configuring managed IPFS
generating ED25519 keypair...done
peer identity: 12D3KooWHZ3C1tDmzUhgi3A6LYmgBxvv1E1obtG7S2ZkYM1MN83
initializing IPFS node at /home/thuselem/.config/fission/ipfs
🔑 Creating keys
🏠 Do you have an existing account? [Y/n] Y
🔗 Please open auth.fission.codes on a signed-in device
📛 Please enter your username: yeti
🔢 Confirmation code: [6, 0, 9, 1, 2, 3]
🎛️  Initializing user config file
```

## Display user information

Use `fission user whoami` to display your username.

```text
$ fission user whoami
💻 Currently logged in as: fission
```

