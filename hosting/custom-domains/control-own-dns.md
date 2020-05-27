---
description: Use a custom domain with Fission and keep control of your DNS
---

# Control your own DNS

By adding a TXT record for your app and a CNAME pointing at our gateway, you can run your DNS elsewhere but still have a custom domain for your Fission app.

### Add a CNAME pointing to ipfs.runfission.com

For your domain, `YOURAPPDOMAIN.com`, go into your DNS provider control panel and add a CNAME pointing to `ipfs.runfission.com`.

| Record | Type | Value |
| :---: | :---: | :---: |
| "\*" \(root\) | CNAME | `ipfs.runfission.com` |

{% hint style="info" %}
Some DNS providers don't support a CNAME for your root, so you'll need to add a CNAME for `www` instead.
{% endhint %}

### Add a TXT record with dnslink info

Now, add a TXT record pointing to your app subdomain

| Record | Type | Value |
| :---: | :---: | :---: |
| \_dnslink | TXT |  `_dnslink="/ipns/APPNAME.fission.app"` |

