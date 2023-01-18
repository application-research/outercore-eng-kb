# Proposal: API Gateway

| Author | Alvin Reyes |
| --- | --- |
| Status | Draft |
| Revision |  |

## Overview

We need a dedicated gateway for serving files and folders (directories) on estuary. We need to have a dedicated gateway for this and not use the current nodes to host it.

## Solution

Create an API gateway or extend the current gateway to load files or directories properly. If a directory is loaded, it needs to return a viewer response of directories or a link > childnode view similar to explore.ipld.io

## Testing

## Edge cases

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  SQL File to add the new table
- [ ]  Swagger documentation changes
- [ ]  Documentation changes