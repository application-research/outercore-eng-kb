# Estuary - Auto Retrieve

## Overview

**This is from AR Pull Request (https://github.com/application-research/estuary/pull/159)*

This PR finishes adding the support for [autoretrieve](https://github.com/application-research/autoretrieve) on estuary. It allows Estuary to send advertisements to [indexers](https://github.com/filecoin-project/storetheindex) on behalf of autoretrieve servers, so that bitswap users looking for CIDs that are on Estuary will think autoretrieve servers are the ones with these CIDs. The clients then go to the autoretrieve servers and ask for the CIDs. Autoretrieve knows the CIDs belong to Estuary, queries and serves them back to the clients through bitswap.

The flow for this on Estuary's end is:

1. Estuary receive an HTTP POST request on `/admin/autoretrieve/init` to register a new autoretrieve server (this can be done by running the following command on autoretrieve)

```
./autoretrieve register-estuary <http://api.estuary.tech/> your-admin-api-key
```

1. Estuary receives regular heartbeats from the registered autoretrieve server on `/autoretrieve/heartbeat`
2. Estuary regularly announces to indexers saying every autoretrieve server that has submitted a heartbeat recently has its CIDs

Related: [https://github.com/application-research/autoretrieve/issues/35](https://github.com/application-research/autoretrieve/issues/35)

### Testing/Running

Checkout and build the branch

```
git checkout tell-indexer
make estuary

```

Run estuary (the `--indexer-url` is optional, it'll point to `https://cid.contact` by default, use it if you want to point to `https://dev.cid.contact` instead). `--indexer-tick-interval` is important because it sets how many minutes estuary waits until it advertises again (and the default is 12h so be sure to set it to a small value when testing).

```
./estuary --logging --indexer-tick-interval=1 --indexer-url=https://dev.cid.contact

```

Finally, register and run an autoretrieve instance (this is important as it'll heartbeat to the estuary instance. If no one is heartbeating then estuary will not issue advertisements). The code to register is not yet merged to autoretrieve master, so we need to checkout the `feat/estuary-registration`.

```
# clone and build autoretrieve
cd ..
git clone git@github.com:application-research/autoretrieve.git
cd autoretrieve
git checkout feat/estuary-registration
go build

# register autoretrieve on estuary
./autoretrieve register-estuary <http://localhost:3004> <ESTUARY-API-KEY>

# run!
./autoretrieve

```

Every new CID uploaded to Estuary should now show up on `https://cid.contact` or `https://dev.cid.contact` (the whole process generally takes around 5mins after the CID is uploaded on estuary).

### Verification

Adding content:

![https://user-images.githubusercontent.com/8129788/181833746-10e72a49-f045-4641-b060-fa373a450127.png](https://user-images.githubusercontent.com/8129788/181833746-10e72a49-f045-4641-b060-fa373a450127.png)

Listing pins:

![https://user-images.githubusercontent.com/8129788/181833830-e0f82266-6fbc-4489-a74c-6ff949782e0a.png](https://user-images.githubusercontent.com/8129788/181833830-e0f82266-6fbc-4489-a74c-6ff949782e0a.png)

Content stats:

![https://user-images.githubusercontent.com/8129788/181833893-92b4684f-8b62-4a2a-976c-f56b907e3066.png](https://user-images.githubusercontent.com/8129788/181833893-92b4684f-8b62-4a2a-976c-f56b907e3066.png)

## Enabling AR

We only need to enable AR on shuttle nodes and not the API node.

To disable AR.

As of 0.1.9 - `--enable-auto-retrieve=false`

For all > 0.1.9 - `--disable-auto-retrieve=true`

Code

NewAutoRetrieveEngine

RegisterMultihashLister