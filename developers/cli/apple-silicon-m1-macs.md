# Apple Silicon \(M1\) Macs

A native `arm64` version of the Fission CLI is not yet available. When support is ready in the Haskell compiler, we will ship native binaries. In the meantime, the Fission CLI runs under "Rosetta 2" emulation. To get this working:

Ensure Rosetta 2 is installed: 

```text
softwareupdate --install-rosetta
```

Install the `x86_64` version of homebrew: 

```text
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

This will install a separate version of homebrew in `/usr/local` for x86\_64 tools.  
  
Finally, Install the Fission CLI via: 

```text
arch -x86_64 /usr/local/bin/brew install fission-suite/fission/fission-cli
```

The Fission CLI will now be available at `/usr/local/bin/fission`

{% hint style="info" %}
For convenience you may want to add something like `alias ibrew='arch -x86_64 /usr/local/bin/brew'` to your shell configuration for managing Intel formulae using `ibrew COMMAND` \(e.g. `ibrew install fission-suite/fission/fission-cli`\).
{% endhint %}



