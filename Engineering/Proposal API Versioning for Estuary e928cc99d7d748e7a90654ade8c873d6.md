# Proposal: API Versioning for Estuary

| Author | Alvin, Mike, Gabe |
| --- | --- |
| Status | Draft |
| Revision |  |

**This is a WIP**

## Proposal/Overview

As of today, Estuary doesn’t have a proper versioning system for its endpoints. We need to implement a versioning system for all the rest api endpoints so we can easily update them while maintaining backward compatibility.

## Solution

We need to wrap all the existing endpoints and redirect them to v1. All subsequent changes should go a new version which will just combine the previous version with the new version.

v1 - first

v2 - v1 + v2

v3 - v1 + v2

v4 - v1 + v2 + v3

## Assumptions (Considerations)

Backward compatible - the current version needs to work or redirected to v1. 

## Technical Design

- consolidate all version 1 into a new go lang file called handler_v1.go
    - this function should house all current endpoints
    - we will need to add a “v1” for all of this
- use echo Pre to create separation of versions
    - add a middleware logic to use v1 by default if no version is passed.
- Now if we introduce a new version, we can create a new endpoint on a new handler_v2.go file with endpoints that starts with “v2/”.
- Introduce a new endpoint called DDL (data definition) `/ddl` that will return all object specs and endpoint specs.

## Testing

## Edge cases

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  SQL File to add the new table
- [ ]  Swagger documentation changes
- [ ]  Documentation changes