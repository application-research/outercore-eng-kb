# Proposal: EstuaryV2 / WhyPFS

| Author | Alvin Reyes |
| --- | --- |
| Status | Draft |
| Revision |  |

## Proposal/Overview

This is a proposal to completely redesign Estuary to simplify the core functions and de-couple the resource intensive parts. This also defines the use of flatfs instead of the existing lmdb database.

For this proposal, we want to use a light ipfs node to store the contents and build the service around this node to manage cids and make filecoin deals for the defined CIDs. 

### WhyPFS Node

- WhyPFS-Core
    - We should let this IPFS node take care of get, put, manage the data store
    - This will also take care of the peering and libp2p host configuration
- Storage Provider Functionality
    - Miner and Miner Selection
    - Filecoin Deal (filclient)
- Users Functionality
    - Users, Admin
    - Upload, Download / Retrieval
    - Collections
    - File/Directory upload
    - All existing Estuary V1 endpoints will be available
- Content Manager Queue
    - Redesign the **bucket** functionality that stages the CIDs using boltDB
    - Content Manager queue checks each buckets of CIDs
    - Buckets are created and a garbage collector removes empty or processed buckets
    - Commitment Piece computation
    - Miner selection and deal maker
- Auto Retrieve Queue
- Gateway
- Tooling
    - S3 driver
    - Barge?
- WebUI - file manager / metrics

The new Estuary node will be a single node model where the actors can either be an API node, Core Node or Both (API and Core Node).

- API nodes will have a gateway and a web ui.
- Core Node can process Contents/CIDs and push them to Filecoin. It will also come with a web ui to manage the shuttle.

## Technical Design

### WhyPFS Core

WhyPFS core - the core node that will be use to peer with other WhyPFS nodes. This is an importable module that has IPLD Dag Service built in. It’s also the main component that manages blockstore, datastore.

### WhyPFS Node

- Holds all the functionality from Estuary
    - Content  and Directories
    - Collections
    - Stats and Metrics
    - WebUI / Explorer
    - Storage Providers
    - Content Manager using WhyPFS Queue
        - Deal Making using Filc

### WhyPFS Queue

This is a batch job framework built it on the WhyPFS node. This will run several pre-configured or customer configured jobs within the lifespan of the running node including Content Management Queue, Pinning requests / Auto Pinning, and Garbage collection

### Custom Queue Job: Bucket / Queuing Jobs for Processing Content and Deals.

There will be 2 types of bucket

- Dedicated Bucket
    - User can create their own buckets and the number of files it can hold. When the user chose to a bucket, the cid will be associated to that bucket. The bucket then will be processed by the global timer (blocktime).
- Global Bucket
    - A global bucket are for cids that doesn’t have a bucket defined upon upload.

### Bucket Components

- Buckets Creator
    - creates bucket objects
        - Note: this is a bucket table with specs
            - Threshold size
            - Schedule (run)
    - check unassigned cids
    - If there unassigned cids, create new bucket
    - Assign cid to bucket objects
    - Notes:
        - We will need a table to store the bucket information
        - Content will have a new column “bucket-uuid” to indicate what bucket the content is assigned.
- Bucket Processor
    - check buckets in init status
    - Check if buckets either have cids over size threshold or time is more than 2 days old.
    - If both or one of conditions met, then submit bucket for deal creation
    - Create deals for each content on the bucket. Lookup cid.
    - Set status of each content (6 deals)
    - Set the status of bucket to complete
- Bucket checker
    - checks existing completed buckets to ensure that al cids have deals or have 6 deals
    - Create deals if one of the cid doesn’t have deals yet.
- Bucket GC
    - Clean up buckets that are more than 3 months old
    - Clean up cids that are more than 3 months old and still failing

### WhyPFS WebUI

This the webui interface for WhyPFS. This will be a dashboard of stats for each node. The stat includes:

- CIDs
- Uptime
- Performance
- Peers
- File Manager

### WhyPFS - Service

Proxifier - load balancer layer

Authorization - decoupled the authorization that nodes can opt-in. 

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  SQL File to add the new table
- [ ]  Swagger documentation changes
- [ ]  Documentation changes
-