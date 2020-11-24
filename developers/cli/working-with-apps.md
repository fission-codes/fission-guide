---
description: The Fission CLI commands to work with apps
---

# Working with Apps

Use the `fission app` command and its subcommands to work with apps. The main operations are registering an app, publishing an app, and displaying information about an app.

## Register an app

The `fission app register` command initializes a new app and links it to your Fission account.

```text
$ fission app register
👷 Build directory (./dist): 
✅ App initialized as big-narrow-fuchsia-elf.fission.app
⏯️  Next run fission app publish or fission app publish --watch to sync data
💁 It may take DNS time to propagate this initial setup globally. In this case, 
you can always view your app at 
https://ipfs.runfission.com/ipns/big-narrow-fuchsia-elf.fission.app
```

You will be prompted for a build directory. The Fission CLI will publish your app from the build directory you select. 

If you are using a common build directory, the Fission CLI will detect it when you run `fission app register` and suggest it. You accept the suggestion or enter another build directory as a relative path from the `fission.yaml` file.

The Fission CLI will create a `fission.yaml` configuration file with a list of files to ignore, a URL where your app will be viewable after it is published, and a build directory. See the [Fission YAML guide ](fission-yaml.md#fission-yaml-for-apps)for more information about the `fission.yaml` file.

{% hint style="info" %}
You can also use a custom domain name for your app. See the [Custom Domains](../custom-domains/) guide to set up a custom domain name.
{% endhint %}

The `fission app register` command has advanced options:

```text
$ fission app register --help
Usage: fission app register [--ipfs-bin BIN_PATH] [--ipfs-timeout SECONDS] 
                            [-v|--verbose] [-a|--app-dir PATH] 
                            [-b|--build-dir PATH] [-n|--name NAME]
  Initialize an existing app

Available options:
  --ipfs-bin BIN_PATH      Path to IPFS binary (default: `which ipfs`)
  --ipfs-timeout SECONDS   IPFS timeout (default: 300)
  -v,--verbose             Detailed output
  -a,--app-dir PATH        The file path to initialize the app in (app config,
                           etc) (default: ".")
  -b,--build-dir PATH      The file path of the assets or directory to sync
  -n,--name NAME           Optional app name
  -h,--help                Show this help text
```

## Publish an app

Use the `fission app publish` command to publish your app to the web. Run this command from the directory that contains your app's `fission.yaml` configuration file.

The `fission app publish` command publishes your app and associates it with the URL in the `fission.yaml` file. After your app is published, the Fission CLI will output a success message and the URL for your app.

```text
$ fission app publish
🚀 Now live on the network
📝 DNS updated! Check out your site at: 
🔗 big-narrow-fuchsia-elf.fission.app
```

{% hint style="info" %}
The `fission up` command is a shortcut for `fission app publish`.
{% endhint %}

The `fission app publish` command has advanced options:

```text
$ fission app publish --help
Usage: fission app publish [--ipfs-bin BIN_PATH] [--ipfs-timeout SECONDS] 
                           [-v|--verbose] [--update-data ARG] [--update-dns ARG]
                           [-w|--watch] [PATH]
  Upload the working directory

Available options:
  --ipfs-bin BIN_PATH      Path to IPFS binary (default: `which ipfs`)
  --ipfs-timeout SECONDS   IPFS timeout (default: 300)
  -v,--verbose             Detailed output
  --update-data ARG        Upload the data (default: True)
  --update-dns ARG         Update DNS (default: True)
  -w,--watch               Watch for changes & automatically trigger upload
  PATH                     The file path of the assets or directory to
                           sync (default: "./")
  -h,--help                Show this help text
```

### Continuously Update an App with Watch

You can continuously publish your app by adding the `--watch` option. The Fission CLI will watch your build directory and publish whenever it detects a change.

```text
$ fission app publish --watch
🚀 Now live on the network
📝 DNS updated! Check out your site at: 
🔗 big-narrow-fuchsia-elf.fission.app
```

This means that as you work in your local development environment, changes are continuously streamed online as you save. Note that some development environments have different code and output options than "production", but this will allow you to quickly and easily share a live online version with other people. 

## Display information about an app

Use `fission app info` to display the URL where your app is viewable.

```text
$ fission app info
✅ App available at big-narrow-fuchsia-elf.fission.app
```

## 



