---
description: An introduction to Fission for developers
---

# Concepts

The Fission CLI and Webnative SDK are developer tools for interacting with Fission services. This section will introduce the technologies used in these tools and describe how they work on your machine and in the broader network.

## Network

The Fission CLI and Webnative SDK use the InterPlanetary File System (IPFS) for networking and data transfer. IPFS uses peer-to-peer communication to exchange data between peers.

{% hint style="info" %}
**What is IPFS?** IPFS is a distributed system for storing and accessing files, websites, applications, and data. If you aren't already familiar with IPFS, you may want to read Protocol Lab's [What is IPFS?](https://docs.ipfs.io/concepts/what-is-ipfs/) guide.
{% endhint %}

![](<../.gitbook/assets/Network Peers@2x.png>)

In the browser, the Webnative SDK connects with Fission peers to access and update user data. Likewise, the Fission CLI connects with Fission peers to publish websites. In both cases, you can substitute your own peers by running your own Fission server.

![](<../.gitbook/assets/Network In Browser@2x.png>)

Apps that use Webnative communicate with Fission peers through a [shared worker](https://developer.mozilla.org/en-US/docs/Web/API/SharedWorker) provided by the Fission Auth Lobby. You can pass an instance of `js-ips` to Webnative to [bring your own peers](https://github.com/fission-suite/webnative/blob/6322c840f20e4f3cd6370d2ca5d999cb20b2e837/src/ipfs/config.ts#L10-L12).

{% hint style="info" %}
**More to come.** We are planning to cover more concepts here. Come by our [Discord server](https://fission.codes/discord) if you have questions on what we have covered so far.
{% endhint %}
