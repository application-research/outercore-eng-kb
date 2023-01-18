# Proposal: CM and AR Component Separation

| Author | Alvin Reyes |
| --- | --- |
| Status | Draft |
| Revision |  |
|  |  |

## Proposal/Overview

The main estuary node runs a few heavy go-routines that affects the performance of the ServerAPI node. 

## Solution

This proposal is to separate the components such as content manager and autoretrieve to it’s own daemon that can run in parallel with the main api node. In this way, we are cleanly separating the load of the api node from the content manager and autoretrieve.

## Assumptions

We will need to re-create some of the node specification (context) of the server api to the content manager and autoretrieve. 

## Technical Design

## Code Changes

## Endpoints

## Testing

## Edge cases

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  SQL File to add the new table
- [ ]  Swagger documentation changes
- [ ]  Documentation changes