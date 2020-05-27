---
description: >-
  Instructions for DNS and domain registrar settings to use a custom domain with
  Fission
---

# Custom Domains

Every Fission App gets a free subdomain.

Currently, these look like `YOURUSERNAME.fission.name`, but we're migrating to user accounts and attached apps, where every app has `YOURAPPNAME.fission.app`.

To use a custom domain of your own, like `YOURAPPDOMAIN.com`, you need to make some changes to DNS. There are two methods detailed in the following pages:

1. [Fission Hosted DNS](fission-hosted-dns.md) - move your nameservers to Fission, we automate everything
2. [Control your own DNS](control-own-dns.md) - create a CNAME and TXT record and point them at Fission services

{% hint style="warning" %}
Note: custom domains currently require manual confirmation by us. This will be integrated directly into the Fission CLI and be fully automated. For now, [contact us on our support page](https://fission.codes/support) if you want to use this feature.
{% endhint %}



