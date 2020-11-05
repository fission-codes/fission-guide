# Publishing an elm-pages app

Fission makes it easy to build and host web apps using only frontend and local development tools with no effort put into servers or deployment workflows.

In this guide, we will use the Fission CLI to publish the `elm-pages-starter` to the web.

### Install Fission and IPFS

We will start by installing the Fission CLI and IPFS.

If you are on macOS, you can install the CLI with `brew`.

```text
brew install fission-suite/fission/fission-cli
```

On Linux, you can download Fission from our [releases](https://github.com/fission-suite/fission/releases) page and move the file to your PATH.

```text
sudo mv ./fission-cli-exe /usr/local/bin/fission
```

On apt-based distros a couple of additional libraries are needed.

```text
sudo apt install libpq-dev libtinfo5
```

The easiest way to install IPFS is with the IPFS Graphical Desktop. You can download it from the IPFS Desktop [releases page](https://github.com/ipfs-shipyard/ipfs-desktop/releases) or use `brew` on macOS.

```text
brew cask install ipfs
```

More options for installing IPFS are described in our [installation guide](https://guide.fission.codes/hosting/installation/ipfs).

With the Fission CLI and IPFS installed we have all the tooling we need. The last step is setting up Fission on your machine. Pick a username and an email you would like to register with Fission and run the `fission setup` command.

```text
$ fission setup
Username: YOURNAME
Email: yourname@example.com
✅ Registration successful!
```

### Publish the elm-pages-starter

Clone the `elm-pages-starter` to your local development environment.

```text
git clone git@github.com:dillonkearns/elm-pages-starter.git
```

Install and build the app.

```text
cd elm-pages-starter
npm install
npm run build
```

Once the `elm-pages-starter` is built, we are ready to publish it with Fission.

Initialize the app from the `dist` directory.

```text
cd dist
fission app-init
```

You should see a confirmation message with a URL where your app will be viewable on the web.

```text
$ fission app-init
✅ App initialized as tasty-young-oval-fairy.fission.app
⏯️  Next run fission up or fission watch to sync data
💁 It may take DNS time to propogate this initial setup globally. In this case, you can always view your app at https://ipfs.runfission.com/ipns/tasty-young-oval-fairy.fission.app
```

Start the IPFS Graphical Desktop and publish the app with `fission up`.

```text
$ fission up
🚀 Now live on the network
👌 QmYjA3wc3AjvNvZ2ocxZ2p9tVJgxoSd66R82AcUjUrXsmb
📝 DNS updated! Check out your site at: 
🔗 tasty-young-oval-fairy.fission.app
```

Copy the URL into your browser and you will see the `elm-pages-starter` live on the web!

You can continuously update the hosted version of an app by running `fission watch`. Fission will watch the files in `dist` and run `fission up` when changes occur.

You can also view apps on `localhost`. Take the hash reported by `fission up` and append it to `localhost:8080/ipfs/`. This link will redirect you to a subdomain where your app is running locally. Taking the hash from the example output above, the link would look like `localhost:8080/ipfs/QmYjA3wc3AjvNvZ2ocxZ2p9tVJgxoSd66R82AcUjUrXsmb`.

> **Where is my app?** An app published with Fission runs locally and on the web on top of the InterPlanetary File System \(IPFS\). When you run `fission up` you are saving your app to IPFS on your machine and syncing it with a greater file system distributed across the web. It's like a part of your file system merges with the whole and your app becomes available to everyone with a browser!

### Learn more

We have put the `elm-pages-starter` out on the web without setting up a DevOps environment or going through a complex deployment workflow!

At this point your mind might be swimming with a thousand questions. Here are a couple of resources that explain how this all works:

* [A Universal "Hostless" Substrate for a Post-Serverless Future](https://www.youtube.com/watch?v=1NBZoJ5fnjM) talk at Øredev 2019
* [Fission Whitepaper](https://whitepaper.fission.codes/)

In the next section, we will add Fission auth to the `elm-pages-starter`.

