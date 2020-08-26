---
description: 'O SDK webnative da Fission, e uma introdução aos aplicativos nativos da web'
---

# Introdução ao Web Native 

"Web native software" is what we call software designed to run on the web, taking advantage of all the capabilities your browser has to offer, as well as native, local capabilities like computation, storage, and identity.

That includes web browsers on mobile devices, which increasingly are the primary and only computing device for the majority of people on the planet.

**webnative** is the name of the software development kit \(SDK\) we have built for the Fission platform, which also functions an app framework for building powerful web native software.

Developers can visit the GitHub code repository at [fission-suite/webnative](https://github.com/fission-suite/webnative), find it on NPM as [webnative](https://www.npmjs.com/package/webnative), or skip this introduction and go straight to Getting Started.

Read on for more of our thinking on web native.

## A web native app works more like native mobile or desktop apps

Compute, storage, and identity are all under control of the person using the software.

A web native app:

* Works in all browsers, including mobile
* Doesn't need browser plug-ins
* Makes use of web APIs in the browser, including on mobile
* Has user owned data and storage
* Gives users control over app permissions
* Can function local-first, and in many cases, offline

Like the original vision of the Internet, much more of the communication is peer-to-peer. Some of the peers are more powerful, or online and backed up more often, but they have the same functions as the local web native app. This is in contrast to the web app client / server model that we see with so many cloud services today, where you can't run the local web interface without the server at all.

## Further Reading

We are inspired by the work of the [Ink & Switch](https://www.inkandswitch.com/) research lab, especially their[ April 2019 publication on Local First Software](https://www.inkandswitch.com/local-first.html), which includes their **Seven Ideals for local-first Software**:

1. No spinners: your work at your fingertips
2. Your work is not trapped on one device
3. The network is optional
4. Seamless collaboration with your colleagues
5. The Long Now
6. Security and privacy by default
7. You retain ultimate ownership and control



