---
description: Fission Platform APIs
---

# Platform APIs

The SDK also exposes methods to interact with the apps associated with the user. This API must be prefixed with `apps`

* `apps.index`: A list of all of your apps and their associated domain names
* `apps.create`: Creates a new app, assigns an initial subdomain, and sets an asset placeholder
* `apps.deleteByURL`: Destroy app by any associated URL

### API

**apps.index**

A list of all of your apps and their associated domain names

Params:

Returns: `{ RandomKey : [ subdomain ] }` a map of subdomains

Example:

```typescript
const index = await sdk.apps.index()
// { `SqlBackendKey {unSqlBackendKey = 216} `: ['your-fission-deployment.fission.app'] }
```

**apps.create**

Creates a new app, assigns an initial subdomain, and sets an asset placeholder

Params:

* subdomain: `string` **optional**

Returns: `subdomain` the newly created subdomain

Example:

```typescript
const newApp = await sdk.apps.create()
// 'your-fission-deployment.fission.app'
```

**apps.deleteByURL**

Destroy app by any associated URL

Params:

* url: `string` **required**

Returns:

Example:

```typescript
const deletedApp = await sdk.apps.deleteByURL('your-fission-deployment.fission.app')
//
```

## 

