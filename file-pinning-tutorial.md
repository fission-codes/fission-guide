---
description: An introduction to the Fission Web API with some example requests
---

# Fission Web API

The Fission Web API provides an interface between HTTP and IPFS. You can interact with it at [https://runfission.com/](https://runfission.com/).  We'll go through a few interactions that you might find helpful, but for the full API, check out the [Swagger docs](https://runfission.com/docs/).

## Get Content From IPFS

Getting content from IPFS is a simple `GET` request to `https://runfission.com/ipfs/{cid}`  
No authentication needed!

For example:

```text
$ curl https://runfission.com/ipfs/QmWATWQ7fVPP2EFGu71UkfnqhYXDYH566qy47CnJDgvs8u
```

If you want to do this programmatically, use your favorite REST client or the [Fission TypeScript Client](https://github.com/fission-suite/typescript-client)

```javascript
import { content } from '@fission-suite/client'
const helloWorld = await content("QmWATWQ7fVPP2EFGu71UkfnqhYXDYH566qy47CnJDgvs8u")
```

## Upload Content To IPFS

You can upload content to IPFS by sending a `POST` request to `https://runfission.com/ipfs`   
For this, you'll need a BasicAuth header with your Fission username & password, as well as a `content-type: application/octet-stream` header

With JSON:

```text
$ curl -i \
     -X POST \
     -H "Content-Type: application/octet-stream" \
     -u 'USERNAME:PASSWORD' \
     --data '{
       "test": 123
     }' \
     https://runfission.com/ipfs
```

With a file:

```text
$ curl -i \
     -X POST \
     -H "Content-Type: application/octet-stream" \
     -u 'USERNAME:PASSWORD' \
     --data-binary @/path/to/file \
     https://runfission.com/ipfs
```

If you want to do this programmatically, use your favorite REST client or the [Fission TypeScript Client](https://github.com/fission-suite/typescript-client)

```javascript
import { add } from '@fission-suite/client'
const auth = { username: "username", password: "password" }
const content = {
  key1: 123,
  key2: 456
}
const cid = await add(content, auth)
```

## Pin Content To IPFS

Fission also offer's a pinning service, if you already have content on IPFS and want the remote Fission node to help keep it online. To pin something, send a `PUT` request to `https://runfission.com/ipfs/{cid}`   
For this, you'll need a BasicAuth header

For example:

```text
curl -i \
     -X PUT \
     -u 'b55cf27ef01025e3c761:Gnu$N+OTlHrauuBg-q_6W1OrjrA_6z0eVhfBvc9sC2LNy,H' \
     https://runfission.com/ipfs/QmWATWQ7fVPP2EFGu71UkfnqhYXDYH566qy47CnJDgvs8u
```

If you want to do this programmatically, use your favorite REST client or the [Fission TypeScript Client](https://github.com/fission-suite/typescript-client)

```javascript
import { pin } from '@fission-suite/client'
const auth = { username: "username", password: "password" }
await pin("QmWATWQ7fVPP2EFGu71UkfnqhYXDYH566qy47CnJDgvs8u", auth)
```

