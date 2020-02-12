---
description: >-
  Instructions for DNS and domain registrar settings to use a custom domain with
  Fission
---

# Custom Domains

Every Fission App gets a free subdomain.

Currently, these look like `YOURUSERNAME.fission.name`, but we're migrating to user accounts and attached apps, where every app has `YOURAPPNAME.fission.app`.

To use a custom domain of your own, like `YOURAPPDOMAIN.com`, you need to point your domain at our name servers in your domain registrar control panel.

{% hint style="warning" %}
Note: custom domains currently require manual confirmation by us. This will be integrated directly into the Fission CLI and be fully automated. For now, [contact us on our support page](https://fission.codes/support) if you want to use this feature.
{% endhint %}

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



