---
description: >-
  Instructions for DNS and domain registrar settings to use a custom domain with
  Fission
---

# Custom Domains

Every Fission App gets a free subdomain.

Currently, these look like `YOURUSERNAME.fission.name`, but we're migrating to user accounts and attached apps, where every app has `YOURAPPNAME.fission.app`.

To use a custom domain of your own, like `YOURAPPDOMAIN.com`, you need to make some changes to DNS. There are two methods detailed below.

{% hint style="warning" %}
Note: custom domains currently require manual confirmation by us. This will be integrated directly into the Fission CLI and be fully automated. For now, [contact us on our support page](https://fission.codes/support) if you want to use this feature.
{% endhint %}

## Control your own DNS

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

## Fission Hosted DNS

We can take care of this for you by hosting your DNS for you. This means changing your name servers to the Fission ones.

### Fission Nameservers

* ns1.fission.systems
* ns2.fission.systems
* ns3.fission.systems
* ns4.fission.systems

### Changing Nameservers in Namecheap

Go to the domain you want to host on Fission in your Namecheap admin:

![](../.gitbook/assets/screenshot-2020-02-12-at-11.07.56-am.png)

 Where it says "Namecheap BasicDNS", select "Custom DNS" from the drop down:

![](../.gitbook/assets/screenshot-2020-02-12-at-11.08.39-am.png)

Enter in `ns1.fission.systems` and `ns2.fission.systems` and click the green check mark to save.



