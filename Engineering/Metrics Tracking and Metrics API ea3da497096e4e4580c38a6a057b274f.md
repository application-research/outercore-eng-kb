# Metrics Tracking and Metrics API

| Author | Alvin Reyes |
| --- | --- |
| Status | Completed  |
| Revision |  |
| Github Repo | https://github.com/application-research/estuary-metrics |
| Grafana | https://protocollabs.grafana.net/d/0-0ztE97z/estuary-team-metrics-dashboard?orgId=1&from=now%2Ffy&to=now%2Ffy |
| Github Issue: | https://github.com/application-research/estuary/issues/283 |

# Overview

The purpose of this document is to create a specification of the Estuary Metrics API. 

## Purpose

In order for any consumers to monitor estuary, be it their own node or the Outercore hosted estuary, there needs to be a way to monitor and consume different functional metrics that estuary provides. 

As of today, there’s only one way to monitor metrics for estuary

1 - thru Grafana

2 - thru public/stats endpoint

# Solution: Grafana

Started working on this: [https://protocollabs.grafana.net/d/0-0ztE97z/estuary-team-metrics-dashboard?orgId=1&from=now%2Ffy&to=now%2Ffy](https://protocollabs.grafana.net/d/0-0ztE97z/estuary-team-metrics-dashboard?orgId=1&from=now%2Ffy&to=now%2Ffy)

# Solution: Estuary Metrics API

![Untitled](Metrics%20Tracking%20and%20Metrics%20API%20ea3da497096e4e4580c38a6a057b274f/Untitled.png)

## Tech **Components**

- Go
- Grafana
- Gorm
- Cacher
- Mux
- PQ
- IPFS

## Use cases

For System metrics, in addition to aggregate, we also want breakdown by shuttle / primary node. 

### **System**

- [x]  Total objects pinned (Query) **`select** *count*(***) **from** contents **where pinning**`
- [x]  Total TiBs uploaded (Query)**`select** *sum*(**size**) **from** objects`
- [x]  Total TiBs sealed data on Filecoin **`select** *sum*(**size**) **from** contents **where pinning and active**`
- [x]  Available free space (custom Grafana plugin)
- [x]  Total space capacity (custom Grafana plugin)
- [ ]  Downtime (this is usually notoriously difficult to define) (custom Grafana plugin)
- [ ]  Performance (this needs to be fleshed out)

### **Users**

- [x]  Total number of Storage Providers (Query) select count(*) from storage_miners
- [x]  Total number of users (Query) select count(*) from users
- [ ]  Ongoing user activity — DAUs, WAUs, MAUs etc. Are users coming back? (custom Grafana plugin) - we would need to build a tracking system for this - Persistent layer for Tracking

For Storage/Retrieval deal metrics, in addition to aggregate, we also want the following breakdowns

- [x]  per day breakdown (Query)
- [x]  per week breakdown (Query)
- [x]  per provider breakdown (Query)

### **Storage**

- [x]  Storage Deal Success Rate (Success % / All Deals)
- [x]  Storage Deal Acceptance Rate (Success % / Accepted Deals)
    - [x]  Total number of storage deals proposed  (Total Deals / Proposed)
    - [x]  Total number of storage deal proposals accepted (Total Deals / Accepted Deals)
    - [x]  Total number of storage deal proposals rejected (Total Deals / Rejected Deals)
- [x]  Total number of storage deals attempted
    - [x]  Total number of successful deals
    - [x]  Total number of failed deals
- [x]  Distribution of data size uploaded per user
- [ ]  Performance metrics
    - [ ]  Time to a successful deal
        - [ ]  how does that scale with data size?

### **Retrieval**

- [ ]  Retrieval Deal Success Rate
- [ ]  Retrieval Deal Acceptance Rate
    - [ ]  Total number of retrieval deals proposed
    - [ ]  Total number of retrieval deal proposals accepted
    - [ ]  Total number of retrieval deal proposals rejected
- [ ]  Total number of retrieval deals attempted (per day and per week breakdown)
    - [ ]  total number of successful retrievals
    - [ ]  total number of failed retrievals
- [ ]  Deals Failed Because Of Undialable Miners
- [ ]  Time To First Byte (retrieval deals)

## Implementation

https://github.com/application-research/estuary-metrics