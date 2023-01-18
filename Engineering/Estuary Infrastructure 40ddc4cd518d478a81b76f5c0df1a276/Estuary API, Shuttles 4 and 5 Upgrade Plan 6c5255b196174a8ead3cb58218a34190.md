# Estuary API, Shuttles 4 and 5 Upgrade Plan

# Overview

We need to migrate over Estuary API, Shuttles 4 and 5. The reason for the migration is:

1. API node data center will be decommissions so it’s a mandated migration.
2. Shuttles 4 and 5 uses a non-ARM based OS so the latest filecoin-ffi won’t build there. Estuary NEEDS to use the up-to-date filecoin-ffi to fix the commP computation issues and version bumps.

There are two major steps we need to do for the migration.

## 1 - Move API node to a new server

- Upgrade API node to move to another server
- Steps
    - Create a new server
    - backup the database and restore on the new server
    - backup the blockstore and restore on the new server
    - Run the API node daemon with the same data and blockstore
    - Test the API node
- Risks
    - Database can have newer entries after the dump.
- Solution to Risk
    - Re-create a new dump from the old one after switching to new API node
    - Compare data
    - Re-insert the ones that are missing

***Progress:  I’m now working on API node (since the data center will be decommissioned by EOM). Target completion is Oct 20 EOD. We will switch over the new one as soon as we are done with testing.***

## 2 - Move shuttles 4 and 5 to a new server

We cant upgrade shuttles 4 and 5 to the latest estuary code base because we can’t build estuary on arm based OS. The solution is to to move this 2 shuttles and reinstall them on an ubuntu 22.x

- Steps
    - We can build a quick server of CIDs on Shuttle 4 and 5 so the clients can still download/retrieve data from these servers. We can minimize the downtime.
    - We shutdown shuttle-4 and 5 (we should announce this downtime).
    - We need at least 3 to 5 days to rsync the blockstore and do the pgdump_all
    - Re-install the same blockstore and database on the new EC2 instance
    - Add more storage to shuttle-4 and 5 so it can accept request again
    - We’ll end up with shuttle 4, 5, 6, 7 and 8.
- Issues
    - Shuttle 4 and 5 being unavailable means any CIDs that’s on those server won’t be retrievable
- Solution to the Issues above
    - We can build a quick server of CIDs on Shuttle 4 and 5 so the clients can still download/retrieve data from these servers. We can minimize the downtim (Step 1).

***Progress: We have yet to start working on this. Target to start looking into this is after we are done with the API node (EOW).***

## **Announce the plan to the public / community / customers**