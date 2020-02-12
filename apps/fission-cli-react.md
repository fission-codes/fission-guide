# Using the Fission CLI to publish React sites

Fission works with any static or client side sites and apps, whether you have a single html file, or you're using a static site generator like [Gatsby](https://www.gatsbyjs.org/) or [Jekyll](https://jekyllrb.com/).

Here we'll walk through how to build and deploy a static site using Gatsby, but the process will be the same for any static site.

First, choose a project, and `cd` into the project directory. For this guide, we'll use [reactjs.org](https://github.com/reactjs/reactjs.org) so that anyone can follow along, but if you have another site in mind, use that!

```bash
$ git clone https://github.com/reactjs/reactjs.org
$ cd reactjs.org
```

Install dependencies:

```bash
$ yarn
```

Build the site:

```bash
$ yarn build
```

`cd` into the build folder

```bash
$ cd public
```

Go interplanetary!

```bash
$ fission up
🚀 Now live on the network
👌 QmTurDD2LNBmmrxP2czBmL15415KFoxEXQ8nvJGhLrgJvU
📝 DNS Updated. Check out your site at:
🔗 21ebedd5d4070a521f83.demo.runfission.com
```

Now copy that link from you terminal to your browser to see your site served live from the decentralized web!

