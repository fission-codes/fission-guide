---
description: More information on the fission.yaml file and options
---

# Fission YAML

Fission stores some configuration options on your local machine by using a file named `.fission.yaml`. One is in your home folder and is for global defaults for all apps on your machine.

## Global Fission YAML

After you run `fission setup`, you'll have a global `.fission.yaml` file in your home folder. If you type the following command, it will display the contents of your global file:

```text
more ~/.fission.yaml
```

The default global file will looks something like this:

```text
ignore: [".DS_Store", ".env", ".fission.yaml"]
peers:
- /ip4/3.215.160.238/tcp/4001/ipfs/QmVLEz2SxoNiFnuyLpbXsH6SvjPTrHNMU88vCQZyhgBzgw
- /ip4/3.226.224.78/tcp/4001/ipfs/QmXab6bcjmWUQZryEtmZfxS5hGoJAguw8bhLdUN5ZFQ2e5
```

### ignore

Ignore is used to list files you don't want `fission up` to upload. We include a couple of defaults to make sure that common files don't get uploaded.

This functions similarly to a `.gitignore` file.

### peers

We make sure that your local computer can directly connect to our servers. These are the IP addresses and fingerprints of our servers.

{% hint style="info" %}
Eventually, we'll have a list of peers around the world.
{% endhint %}

## Fission YAML for Apps

When you create a new app using `fission app-init`, a new `.fission.yaml` file is created in the directory where you ran the command. Here's an example:

```text
app_url: junior-angular-tulip.fission.app
ignore:
 - *.psd
 - _ignore
```

### app\_url

This is the URL where your app is available.

{% hint style="info" %}
Eventually, you'll be able to specific different versions -- for example, development, testing, production, etc. -- as well as list custom domains here.
{% endhint %}

### ignore

As you can see, this can also include a local ignore directive, that just applies to the local app directory. There won't be an ignore section by default, but you can edit the file to add whatever you like here.

YAML can specify arrays in a couple of different ways, both like this, and comma separated as in the global example above.

The `*.psd` means, ignore and don't upload any Photoshop files with a `.psd` extension.

The `_ignore` is just a convention, to make a local directory with that name where you can keep all your files together, but know that files in that folder won't get uploaded.

### 



