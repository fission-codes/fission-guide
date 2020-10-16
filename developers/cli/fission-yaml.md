---
description: More information on global and app YAML configuration files.
---

# Fission YAML

Fission stores global and app configuration options in YAML files. The global configuration is stored in a `config.yaml` file and app configurations are stored in `fission.yaml` files.

## Global Fission YAML

After you run `fission setup`, you will have a global `config.yaml` in your `~/.config/fission/` folder. You can open `config.yaml` in a text editor or display it at the command line.

```bash
more ~/.fission.yaml
```

The default global `config.yaml` will looks something like this:

```yaml
ignore: []
username: fission
server_did: did:key:zStEZpzSMtTt9k2vszgvCwF4fLQQSyA15W5AQ4z3AR6Bx4eFJ5crJFbuGxKmbma4
peers:
- /dns4/node.runfission.com/tcp/4001/ipfs/QmVLEz2SxoNiFnuyLpbXsH6SvjPTrHNMU88vCQZyhgBzgw
- /ip4/3.226.224.78/tcp/4001/p2p/QmPeeeZZXxMBAPxxba7a6ggjDr5jLuD3RAFnmPPcvJ9fMS
- /ip4/3.226.224.78/udp/4001/quic/p2p/QmPeeeZZXxMBAPxxba7a6ggjDr5jLuD3RAFnmPPcvJ9fMS
signing_key_path: /home/fission/.config/fission/key/machine_id.ed25519
```

{% hint style="warning" %}
In most cases, the only thing you will want to change in this file is the `ignore` section. The other sections are managed by the Fission CLI.
{% endhint %}

### ignore

Ignore is a list of files you don't want `fission app publish` to publish. For example, you might add commonly ignored files and secrets.

```yaml
ignore: [".DS_Store", ".env"]
```

Ignore follows the same conventions used in a `.gitignore` file.

### username

Username is a name you select for yourself during `fission setup`.

### server\_did

Server DID is the identity of the Fission server that authenticates your requests when using the Fission CLI.

### peers

We configure your machine to directly connect to our servers. The peers are the IP addresses and fingerprints of our servers.

{% hint style="info" %}
Eventually, we'll have a list of peers around the world.
{% endhint %}

### signing\_key\_path

The path to the key used to sign requests made when using the Fission CLI.

## Fission YAML for Apps

When you create a new app with `fission app register`, a `fission.yaml` file is created in the directory where you ran the command. The default `fission.yaml` file looks something like:

```yaml
ignore: []
url: junior-angular-tulip.fission.app
build: ./dist
```

### ignore

Ignore works the same as the ignore in the global configuration. You can use it to list files that you do not want to publish.

```yaml
ignore:
 - node_modules
 - *.psd
```

You might use it to ignore a directory like `node_modules` or all files with an extension like Photoshop `.psd` files.

{% hint style="info" %}
YAML can specify lists in a couple of different ways, both like this, or as comma-separated list shown in the global `config.yaml` example above.
{% endhint %}

### url

The URL where your app is viewable after it has been published.

{% hint style="info" %}
Eventually, you will be able to publish different versions of your app -- for example, development, testing, production, etc. -- and use custom domains for your app.
{% endhint %}

### build

The build directory is the directory the Fission CLI will publish. Use this when you have a build step that produces a production-ready version of your application.

The build directory is set as a relative path from the location of the `fission.yaml` file.

{% hint style="success" %}
If you are using a "common" build directory, the Fission CLI will attempt to detect it the first time you run `fission app register`. It will prompt you and ask if you would like to use the directory it detects.
{% endhint %}

