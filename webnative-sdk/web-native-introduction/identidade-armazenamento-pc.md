---
description: Fission's approach to providing and working with web native building blocks
---

# Compute, Storage, and Identity Foundations

The foundation of powerful digital applications roughly include three primary building blocks: compute, storage, and identity. Fission's approach is to make use of these building blocks, make them easier to use for developers, and to develop strong default capabilities in each of these areas.

The [Fission White Paper](https://whitepaper.fission.codes/) can be read for a more technical, specification-style version of our vision and roadmap, including direct links to various sections:

* [WinFS File System](https://whitepaper.fission.codes/file-system)
* [DID-based identity, UCAN + JWTs](https://whitepaper.fission.codes/identity)

Additional reading and presentations are also available:

* User Controlled Authorization Network \(UCAN\), [Auth without a Backend blog post](https://blog.fission.codes/auth-without-backend/)
* Presentation to the Berlin Functional Programming Meetup June 2020, [A Universal Hostless Substrate: Full Stack Web Apps Without a Backend, and More!](https://talk.fission.codes/t/berlin-functional-programming-group-a-universal-hostless-substrate-full-stack-web-apps-without-a-backend-and-more/617)

## Compute

Web browsers run on your local desktop, laptop, or mobile device, and use local computer processing unit \(CPU\) to do work -- to compute things. Your machine provides the work to run apps in your browser. Simple web pages use the browser's built in HTML and CSS capabilities. But many web pages are full blown "apps" that are highly interactive, with a rich user interface \(UI\). These are typically powered by JavaScript, a programming language that runs in your browser, and uses your local compute.

Web Assembly is a fourth compute environment has been standardized for browsers. It can compute apps that are originally written in many different programming languages, not just JavaScript. We are just starting to see powerful Web Assembly apps being built, like Figma, which provides a PhotoShop-like experience in your web browser.

Fission intends to support Web Assembly, and make it easier to build, host, and scale web native apps that include Web Assembly.

## Storage

When people think about owning their data, this often reveals that they feel comfortable when they can see and browse the files that an app uses or creates, rather than having to request an export, or not even have access to their data at all.

At Fission, we started by designing a file system that is available to both developers who want to build and host apps, and the people who use these apps.

We have designed what we call **Web Native File System**, pronounced and written as **WinFS**. It is built on top of other open protocols, including the InterPlanetary File System \(IPFS\), which you can [learn more about in the appendix](../../appendix/ipfs.md).

We intend to design and share the WinFS specification in the open, and contribute it as an open standard, with open source implementations that anyone can use.

WinFS is designed to provide an experience like an open source iCloud: available and synchronized across all devices and provide both public and private, encrypted files.

## Identity

Implementing a login, authentication, and authorization system is challenging to do securely, especially while respecting user privacy, providing end to end encryption, and otherwise balancing ease of use for both developers and customers.

For developers, we wanted to provide a built in way to include logins and authentication without having to struggle to implement it.

For everyone, we wanted it to be easy to login securely, without having to remember passwords, and make that login available on all of their devices.

We built on top of the emerging Decentralized Identifiers \(DIDs\), which is a [working draft of the W3C standards body](https://www.w3.org/TR/did-core/).

We then designed the User Controlled Authorization Network \(UCAN\): a way of doing authorization where users are fully in control. 

With UCAN, we based it on top of the developer friendly JSON Web Tokens \(JWTs\) standard, plus Google's Macaroons research paper for more distributed systems.

We combine all of this in the webnative SDK for developers and the Fission platform for app users. In browsers, we use the [Web Cryptography API, a W3C standard](https://www.w3.org/TR/WebCryptoAPI/). The [MDN Web Docs include further reading »](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)

We intend to work with existing and emerging authorization and identity standards bodies to contribute our work as open standards.

