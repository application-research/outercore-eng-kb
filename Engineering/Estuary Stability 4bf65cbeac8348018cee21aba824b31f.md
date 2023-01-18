# Estuary Stability

| Author | Alvin Reyes |
| --- | --- |
| Status | Draft |
| Revision |  |

## Overview

These are the things we need to accomplish to get Estuary to the alpha and post alpha stage. I’d like to look at each as `Pillars` with each being built and should perfectly levelled to stablize the Estuary platform.

*This is all Tech. No productization / product lifecycle steps here.*

Github Project: [https://github.com/orgs/application-research/projects/7/views/5](https://github.com/orgs/application-research/projects/7/views/5) 

| Priority | Improvements  Issues | Conversations / Discussions |
| --- | --- | --- |
| 1 | System Errors | All systems error that estuary encounters  |
| 2 | Infrastructure | All the action items we need to do to stablize the infrastructure along with the code changes |
| 3 | Data Clean up | Any stale data we need to remove or clean up |
| 4 | Debugging | All the action items we need to do to debug or provide us more information on how to debug. |
| 5 | Functional | All functional / design / code that needs to be optimized and improved |
| 6 | Support | All the action items I think we need to do ensure we have the proper customer support |

## System Errors (Panics)

All on it’s own page. We need to handle all the panics.

Log file: 

[log_file_from_shuttle6](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/Untitled.json)

[msg":"couldnt decode pid](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/msg%20couldnt%20decode%20pid%20b1c06d9f60704deeb7ea08ae293506f5.md)

[pinning queue error: context canceled\nfailed to walk DAG\nmain.](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/pinning%20queue%20error%20context%20canceled%20nfailed%20to%20wa%20eae17bb03607444cabcf879405a43c19.md)

[failed to handle rpc command: Unable to send restart request: exhausted 5 attempts but failed to open stream to ](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/failed%20to%20handle%20rpc%20command%20Unable%20to%20send%20restar%20ea4351e6923949d78d57bea684acca34.md)

[pinning queue error: context deadline exceeded\nfallback provide failed\nmain](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/pinning%20queue%20error%20context%20deadline%20exceeded%20nfal%2060840bfd739d447ab1f7ed5984829145.md)

[tried to add pin for content we failed to pin previously](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/tried%20to%20add%20pin%20for%20content%20we%20failed%20to%20pin%20prev%20008b15ae50ed4a0badac4079b80c74f0.md)

[failed to handle rpc command: failed to compute commP](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/failed%20to%20handle%20rpc%20command%20failed%20to%20compute%20com%20b5ad787ace884d64b9fc51252d816b40.md)

[failed to handle rpc command](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/failed%20to%20handle%20rpc%20command%2015384024daca414487c15e5905967939.md)

## Infrastructure

- [ ]  Grafana agent on ansible so we can source out the storage of the logs to grafana. We save up some space if we do so. We do have this on shuttles, but not enabled properly. https://filecoinproject.slack.com/archives/C016APFREQK/p1665703251824289
- [ ]  Install / Enable agents on all shuttles - enabling these agents will ensure that logs are stored on grafana only.
- [ ]  Document the release and deployment ([https://www.notion.so/Estuary-Infrastructure-40ddc4cd518d478a81b76f5c0df1a276](Estuary%20Infrastructure%2040ddc4cd518d478a81b76f5c0df1a276.md))
- [ ]  Troubleshooting guide for the infrastructure - I’ll be adding more information on this.
- [ ]  Back up and restore (enable data and blockstore backups) - I’d like to work with infrastructure point of content for this.
- [ ]  Infra improvement: Dockerize all components
- [ ]  Infra improvement: Create a simple kube cluster for POC
- [ ]  E2E Test Env: estuary + lotus + boost

## Data Clean up

[https://filecoinproject.slack.com/archives/C016APFREQK/p1660258369066179](https://filecoinproject.slack.com/archives/C016APFREQK/p1660258369066179)

- [ ]  Write an SQL script to remove the majority of the non-active `pins` (14m plus records) on shuttle-4, e.i Delete all non-active pins.
- [ ]  The negative impact of removing these records is that if some of the pins are on the blockstore then anyone who uses the `/gw` will fail to look up the CID since this gateway relies on the database record.
- [ ]  There might be some failed pins that are yet to be processed by the SP so lost of opportunity there.
- [ ]  Another solution is to create a clean up script on shuttle-4 to traverse thru the blockstore using the CIDs from the `pins` table, identify those that the shuttle can't "walk" - meaning it's in the database but not on the blockstore (using merkledag.Walk), and delete them on the database. It will be like a "estuary shuttle reconciler" tool to match the blockstore CID with the pins table.
- [ ]  Write scripts that can perform backups on specific filters.
- [ ]  Write SQL script to delete the CIDs that doesn’t exist on the blockstore of the local node.

## Debugging

- [ ]  Enable developers that they have the proper debugging tools (GoLand).
- [ ]  Set up dedicated shuttles for each developer (for dev testing)
- [ ]  Enable pprof on all shuttles and api node
- [ ]  Enable grafana agents

## Functional

- [ ]  Revisit the pinning mechanism
    - [ ]  We need to revisit the pinning process, specifically the infinite loops and initialization of workers to pin specific content. The current process right now is causing a build of unnecessary memory allocation on the PinningOperation which contributes to the OOM issue.
    - [ ]  [https://www.notion.so/ecosystem-wg/Pin-Manager-use-a-disk-based-queue-757a79f9fc8d47b09a2f46112d2c423c](https://www.notion.so/Pin-Manager-use-a-disk-based-queue-757a79f9fc8d47b09a2f46112d2c423c)
- [ ]  Revisit the queueing mechanism
    - [ ]  I’d like to explore the possibility of separating the queuing from the main api node. We had discussions on this before and I would like to revisit.
- [ ]  Revisit all the infinite for loops and check if we need to create intervals or optimize them.

![Untitled](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/Untitled.png)

- shuttle.go
    
    [handleShuttleMessages](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/handleShuttleMessages%20ef742bde11904105ac903bd34283f86a.md)
    
- autoretrieve.go
- shuttle/main.go
    
    [RunRpcConnection](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/RunRpcConnection%209528c8e546f24214b0098c8595016db5.md)
    
    [websocket connection handleRpcCmd](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/websocket%20connection%20handleRpcCmd%201937fadff16149a987ec06b43262179e.md)
    
    [websocket.JSON.Send](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/websocket%20JSON%20Send%20e84ba33b9d5d4bc4b816c031aa539b55.md)
    
    [addDatabaseTrackingToContent](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/addDatabaseTrackingToContent%20943a5758fc0f4acf90748462f12b6ea5.md)
    
- handlers.go
    
    [addDatabaseTrackingContent (duplicate code)](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/addDatabaseTrackingContent%20(duplicate%20code)%209df4eea9021c4a9ea74b4614977c75b7.md)
    
    [websocket.JSON (duplicate code)](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/websocket%20JSON%20(duplicate%20code)%2023188a77bd574951b6eae37240bd2e28.md)
    
    [handleShuttleConnection](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/handleShuttleConnection%207300c63791b647d7a2784390cac8e7de.md)
    
- pinmgr.go
    
    [Run(workers int)](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/Run(workers%20int)%200b4095a4ad8d4bfbb8d59598c7e56d60.md)
    
- replication.go
    
    [runStagingBucketWorker](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/runStagingBucketWorker%20e4463aeb691b4492b35baea7a2bbaac8.md)
    
    [runDealWorker](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/runDealWorker%20ae9ed451f14448159e7042992b3dbcd8.md)
    
- trackbs.go
- benchtest/main.go
- AutoRetrieve
    
    [AR](Estuary%20Stability%204bf65cbeac8348018cee21aba824b31f/AR%2031fc42ad1bff451094040d841ba662fe.md)
    
- [ ]  Unit Tests (Quality Assurance) - there is a unit-tests branch that has placeholder of unit tests source files in go. I know it’s not the best thing to do so I think we should just collectively, slowly and piece by piece put up a “chore” commit to clean up and create unit tests as we go.
    - [ ]  Revisit: [https://github.com/application-research/estuary/tree/unit-tests](https://github.com/application-research/estuary/tree/unit-tests)
- [ ]  Automated / Regression Tests - we should at least run the shell or postman jobs to run the API endpoint tests.
    - [ ]  Revisit if we can run a script to call postman passing the collection file using  postman CLI: [https://github.com/application-research/estuary/tree/master/tests](https://github.com/application-research/estuary/tree/master/tests)

### Functional Improvements

[Proposal: Collections API V2](Proposal%20Collections%20API%20V2%204a94dec4fdbb44ad81badc297fe9f567.md) 

[Proposal: Directory API](Proposal%20Directory%20API%20ce8fe075d06349a3a104b1bcbaeee396.md) 

[Proposal: API Versioning for Estuary](Proposal%20API%20Versioning%20for%20Estuary%20e928cc99d7d748e7a90654ade8c873d6.md) 

[Proposal: Proxy-Forwarder](Proposal%20Proxy-Forwarder%20bf65c8b82d9f4e13b49728a99e228e90.md) 

[Proposal: API Gateway](Proposal%20API%20Gateway%20179b82bf93d442cf8e88d6c39f1870f9.md) 

## Support

- [ ]  Customer Support Ticket System
- [ ]  IsEstuaryDown.com public monitoring tool

## Refactor / Rearchitecture

- [ ]  Refactor code to its appropriate packages
- [ ]  Redesign