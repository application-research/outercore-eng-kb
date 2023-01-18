# Proposal: Proxy-Forwarder

| Author | Alvin Reyes |
| --- | --- |
| Status | Draft |
| Revision |  |

## Proposal/Overview

We want to build a proxy that will wrap all the “shuttle” functionality. The main solution here is the load-balancer which will need to be smarter than round-robin selection.

1 - The proxy needs to know the list of “available” shuttle. 

2 - geo location aware using X-Real-IP.

3 - Use the IP to locate the nearest shuttle available.

4 - location takes precendence. round robin priority after.

## Solution

## Assumptions

## Technical Design

## Testing

## Edge cases

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  SQL File to add the new table
- [ ]  Swagger documentation changes
- [ ]  Documentation changes