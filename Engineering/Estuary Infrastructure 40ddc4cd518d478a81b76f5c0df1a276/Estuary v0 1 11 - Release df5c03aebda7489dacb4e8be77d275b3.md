# Estuary v0.1.11 - Release

## Overview

This document is to consolidate all considerations and action items to be done before, during and after the release.

## Preparation (Before)

Anything we need to do before the actual deployment (QA, triple check configurations, where to deploy, impact, announcements).

| Consideration | Steps |
| --- | --- |
| Announcement of v0.1.11 | Announced on #ecosystem-dev and twitter. Make sure to inform users/customers and add action steps for them. |
| smoke test on dev branch. |  |
| For AR any flags we need to set?  | ‣ ‘s recommended flag settings
--indexer-advertisement-interval=15m attempt to advertise new contents up to every 15 minutes (the default is 1 minute, but don’t want to fight for bandwidth)

—indexer-url does not need to be set because it defaults to cid.contact

AR publisher note: if the publisher fails, it will not affect the operation of the rest of Estuary main node |
| Do we need to set any new flags for API node and Shuttle?  |  |
| Any DB / SQL scripts we need to change or add before? |  |

## During

Things we need to do or need to do during the deployment.

| Consideration | Steps |
| --- | --- |
| Anyone needs to be online during the deployment? |  |

## Post Deployment (After)

| Consideration | Steps |
| --- | --- |
| Anyone needs to be online during the deployment? QA / Testing - What test should we perform and how? | ‣ needs to run the registration script on the autoretrieve deployment, and test autoretrieve download performance and proper advertisement creation. This does not need to be immediate. |
| Rest APIs - any specific endpoint we need to test and what is the result? |  |
| Are there any DB changes check to confirm? |  |
| Announcement |  |