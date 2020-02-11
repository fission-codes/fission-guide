# Learning IPFS

The main [IPFS docs website](https://docs.ipfs.io/introduction/) is a good place to start to read and learn about IPFS. We've included a few explanations about certain ways that we use core IPFS features.

Other resources:

* [ProtoSchool](https://proto.school/): features a number of interactive web tutorials, as well as the home base for in-person chapters around the world

### DNSLink

From the [IPFS docs](https://docs.ipfs.io/guides/concepts/dnslink/):

> DNSLink uses [DNS TXT](https://en.wikipedia.org/wiki/TXT_record) records to map a domain name \(like `ipfs.io`\) to an IPFS address. Because you can edit your DNS records, you can use them to always point to the latest version of an object in IPFS \(remember that an IPFS object’s address changes if you modify the object\). Because DNSLink uses DNS records, the names it produces are also usually easy to type and read.

Fission Live automagically updates the DNS TXT record for you whenever you make changes.

#### IPNS vs. IPFS in DNSLink

You can use either an IPNS or an IPFS address for DNSLink.

If you use IPNS, you are entering the Peer ID of your local node, and your node will have to be online. However, on the positive side, you don't need to update your TXT record, as your Peer ID is consistent. Note that IPNS records are only cached for 24 hours, so you'll have to refresh at least once a day.

If you use an IPFS address, you are entering the hash representing the DAG of a folder. Whenever you make an edit to any of the files in the folder, the DAG hash changes, and you need to update the DNS TXT record each time. Fission Live does this automatically using `up` or `watch`, and we pin your files so they will always be online.





