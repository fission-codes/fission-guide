---
description: Interplanetary Fission IPFS - Fission Web API as an Heroku Add-on
---

# Heroku Add-on

The Fission Web API is available as a Heroku Add-on. F[ind it in the Heroku Add-Ons Marketplace](https://elements.heroku.com/addons/interplanetary-fission).

### Deploy to Heroku

A particularly good use case for using our Fission IPFS add-on is to combine it with setting up your application in a "Deploy to Heroku" mode.

This means adding an `app.json` file and a few other settings that tell Heroku what add-ons and environment variables your application needs. You can find out more [how to set this up in Heroku's documentation](https://devcenter.heroku.com/articles/heroku-button).

### Example Deploy to Heroku Apps

#### Ghost Blogging on Heroku with IPFS

We built an [IPFS Storage Adapter for the Ghost blogging platform](https://github.com/fission-suite/ghost-storage-adapter-ipfs), and bundled it together with Deploy to Heroku and the Fission Heroku Add On. You can get started on the hobby tier by clicking the Deploy to Heroku button on Github:

* [https://github.com/fission-suite/heroku-ipfs-ghost](https://github.com/fission-suite/heroku-ipfs-ghost)

