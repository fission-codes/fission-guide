# Publishing an elm-pages app

Fission makes it easy to build and host web apps using only frontend and local development tools with no effort put into servers or deployment workflows.

In this guide, we will use the Fission CLI to publish the `elm-pages-starter` to the web.

{% hint style="warning" %}
**Install Fission**. Please follow the steps in the [Installation guide](https://guide.fission.codes/hosting/installation) to install and set up Fission on your machine before proceeding.
{% endhint %}

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

Initialize the project as a Fission app with `fission app register`. You will be prompted for a build directory.

```text
 $ fission app register
👷 Build directory (./dist): 
```

Accept `dist` as your build directory, and `fission` will initialize your app.

```text
✅ App initialized as long-skinny-stone-monkey.fission.app
⏯️  Next run fission app publish or fission app publish --watch to sync data
💁 It may take DNS time to propagate this initial setup globally. In this case, 
you can always view your app at 
https://ipfs.runfission.com/ipns/long-skinny-stone-monkey.fission.app
```

Fission has created a `fission.yaml` file with your app configuration.

{% code title="fission.yaml" %}
```yaml
ignore: []                                                                      
url: long-skinny-stone-monkey.fission.app
build: ./dist
```
{% endcode %}

The `url` is where the app will be on the web after it has been published, `build` tells the fission which directory to publish, and `ignore` is a list of files and directories to ignore. See the [Fission YAML guide ](https://guide.fission.codes/hosting/cli/fission-yaml)for more details.

Run `fission app publish` to publish the webpage.

```yaml
$ fission app publish
🚀 Now live on the network
📝 DNS updated! Check out your site at: 
🔗 long-skinny-stone-monkey.fission.app
```

Copy the URL into your browser and you will see the `elm-pages-starter` live on the web!

{% hint style="warning" %}
It may take a few minutes for DNS to propagate. If you don't see the app live at the URL, come back and try again in a bit. You can also take the URL from the CLI output and view the app at`https://ipfs.runfission.com/ipfs/{URL}`right away.
{% endhint %}

You can continuously update the hosted version of the app by running `fission app publish --watch`. Fission will watch the files in `dist` and publish when changes occur.

{% hint style="info" %}
**Where is my app?** An app published with Fission runs locally and on the web on top of the InterPlanetary File System \(IPFS\). When you run `fission up` you are saving your app to IPFS on your machine and syncing it with a greater file system distributed across the web. It's like a part of your file system merges with the whole and your app becomes available to everyone with a browser!
{% endhint %}

### Learn more

We have put the `elm-pages-starter` out on the web without setting up a DevOps environment or going through a complex deployment workflow!

At this point your mind might be swimming with a thousand questions. Here are a couple of resources that explain how this all works:

* [A Universal "Hostless" Substrate for a Post-Serverless Future](https://www.youtube.com/watch?v=1NBZoJ5fnjM) talk at Øredev 2019
* [Fission Whitepaper](https://whitepaper.fission.codes/)

In the next section, we will add Fission auth to the `elm-pages-starter`.

