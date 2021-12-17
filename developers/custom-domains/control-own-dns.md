---
description: Use a custom domain with Fission and keep control of your DNS
---

# Control your own DNS

By adding a TXT record for your app and a CNAME pointing at our gateway, you can run your DNS elsewhere but still have a custom domain for your Fission app.

### Add a CNAME pointing to ipfs.runfission.com

For your domain, `YOURAPPDOMAIN.com`, go into your DNS provider control panel and add a CNAME pointing to `ipfs.runfission.com`.

|  Record  |  Type |         Value         |
| :------: | :---: | :-------------------: |
| @ (root) | CNAME | `ipfs.runfission.com` |

{% hint style="info" %}
@ usually represents the "root" or domain apex. Some DNS providers don't support a CNAME for your root, so you'll need to add a CNAME for `www` instead.
{% endhint %}

The process for a subdomain is the same `SUB.YOURAPPDOMAIN.com`, but looks a little different. Go into your DNS provider control panel and add a CNAME pointing to `ipfs.runfission.com`.

| Record |  Type |         Value         |
| :----: | :---: | :-------------------: |
|   SUB  | CNAME | `ipfs.runfission.com` |

### Add a TXT record with dnslink

Next, add a TXT record pointing to your app subdomain. Your APPNAME is created when you run `fission app register`, and will look something like `junior-angular-tulip` . We're creating a link to say that the content from your app should be served up your domain with a special dnslink record:

|   Record  | Type |                 Value                |
| :-------: | :--: | :----------------------------------: |
| \_dnslink |  TXT |  `dnslink=/ipns/APPNAME.fission.app` |

Subdomains function the same way, with the \_dnslink subdomain, where you're adding the TXT record being "above" the SUB.YOURDOMAIN.com record, which looks like this:

|     Record    | Type |                 Value                |
| :-----------: | :--: | :----------------------------------: |
| \_dnslink.SUB |  TXT |  `dnslink=/ipns/APPNAME.fission.app` |

### Get in touch to complete the process

This process is not fully automated right now, so you'll need to send us an email `support@fission.codes` or drop by our [support page](https://fission.codes/support) to send us a chat, and we can get things setup on our end.
