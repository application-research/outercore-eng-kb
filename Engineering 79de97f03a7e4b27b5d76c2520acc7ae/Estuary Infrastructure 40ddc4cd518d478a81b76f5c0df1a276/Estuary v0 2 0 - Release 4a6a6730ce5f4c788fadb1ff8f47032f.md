# Estuary v0.2.0 - Release

## Overview

This document is to consolidate all considerations and action items to be done before, during and after the release.

## Preparation (Before)

Anything we need to do before the actual deployment (QA, triple check configurations, where to deploy, impact, announcements).

| Consideration | Steps |
| --- | --- |
| Considerations

For AR any flags we need to set?  | ‣ ‘s recommended flag settings
--indexer-advertisement-interval=15m attempt to advertise new contents up to every 15 minutes (the default is 1 minute, but don’t want to fight for bandwidth)

—indexer-url does not need to be set because it defaults to cid.contact

AR publisher note: if the publisher fails, it will not affect the operation of the rest of Estuary main node |
| Announcement of v0.2.0 | Announced on #ecosystem-dev and twitter. Make sure to inform users/customers and add action steps for them. |
| Prepare the tag | checkout
git checkout dev
git tag v0.2.0
git push origin v0.2.0 |

## During

Things we need to do or need to do during the deployment.

| Consideration | Steps |
| --- | --- |
| Anyone needs to be online during the deployment? | ‣ ‣ ‣ ‣ ‣ ‣  |
| Deploy to API node | ssh root@api.estuary.tech

create the build
```
cd estuary
git fetch —tags
git checkout v0.2.0
make all
```

edit the systemctl settings to add the flag for AR.
systemctl edit estuary.service —full
add
--indexer-advertisement-interval=15m

Restart
```
systemctl stop estuary.service
cp estuary /mnt/bin
systemctl restart estuary.service
``` |
| Deploy to shuttle 1 | ssh ubuntu@shuttle-1.estuary.tech |
| Deploy to shuttle 2 | ssh ubuntu@shuttle-2.estuary.tech |
| Update/Deploy to shuttle 4 | ssh estuary@shuttle-4.estuary.tech

create the build
```
cd estuary
git fetch —tags
git checkout v0.2.0
make all
```

edit the systemctl settings to add the flag for AR.
systemctl edit estuary.service —full
add
--indexer-advertisement-interval=15m

```
systemctl stop estuary.service
cp estuary /usr/local/bin
systemctl restart estuary.service
``` |
| Update/Deploy to shuttle 4 | ssh ubuntu@shuttle-5.estuary.tech

create the build
```
cd estuary
git fetch —tags
git checkout v0.2.0
make all
```

edit the systemctl settings to add the flag for AR.
systemctl edit estuary.service —full
add
--indexer-advertisement-interval=15m

```
systemctl stop estuary.service
cp estuary /usr/local/bin
systemctl restart estuary.service
``` |
| Deploy to shuttle 6 | ssh root@shuttle-6.estuary.tech

create the build
```
cd estuary
git fetch —tags
git checkout v0.2.0
make all
```

edit the systemctl settings to add the flag for AR.
systemctl edit estuary.service —full
add
--indexer-advertisement-interval=15m

set the LMDB config (to test migration)
—blockstore=

```
systemctl stop estuary.service
cp estuary /usr/local/bin
systemctl restart estuary.service
``` |
| Deploy to shuttle 7 | ssh root@shuttle-7.estuary.tech

create the build
```
cd estuary
git fetch —tags
git checkout v0.2.0
make all
```

edit the systemctl settings to add the flag for AR.
systemctl edit estuary.service —full
add
--indexer-advertisement-interval=15m

```
systemctl stop estuary.service
cp estuary /usr/local/bin
systemctl restart estuary.service
``` |
| Deploy to shuttle 8 | ssh root@shuttle-8.estuary.tech

create the build
```
cd estuary
git fetch —tags
git checkout v0.2.0
make all
```

edit the systemctl settings to add the flag for AR.
systemctl edit estuary.service —full
add
--indexer-advertisement-interval=15m

```
systemctl stop estuary.service
cp estuary /usr/local/bin
systemctl restart estuary.service
``` |

## Post Deployment (After)

| Consideration | Steps |
| --- | --- |
| Anyone needs to be online during the deployment? QA / Testing - What test should we perform and how? | ‣ needs to run the registration script on the autoretrieve deployment, and test autoretrieve download performance and proper advertisement creation. This does not need to be immediate. |
| Announcement | Announced on #ecosystem-dev and twitter. Make sure to inform users/customers and add action steps for them. |