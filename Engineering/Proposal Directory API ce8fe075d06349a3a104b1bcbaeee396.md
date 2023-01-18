# Proposal: Directory API

| Author |  |
| --- | --- |
| Status | Draft |
| Revision |  |

**This is a WIP**

## Proposal/Overview

Estuary currently doesn’t support uploading directories. In order to broaden the scope of our target users, we need to have support for both files and directories. 

## Solution

A directory in the IPFS sense is a collection of specific type of node (DirNode) that can be associated with different links (ChildNode). The DirNode creates the initial underlying structure of the merkle which then can be used by the developer to programmatically add links to it to form a “directory” structure in a merkle dag format. This merkle dag then is stored on the estuary node blockstore via go-blockstore library. 

## Assumptions

Directories are similar to `ipfs add -r` where it’ll take a directory, recursively go thru the directories and add each nodes as either parent (dir) or child (dir or file)

## Technical Design

Impacted components - estuary-www and estuary node

Upload directory using js-ipfs / estuary-www - we will need to include the js-ipfs to estuary-www to allow the user to upload the directory via the browser with built-in IPFS. We then use the js-ipfs to create the DIR/Child structure from estuary.

Upload directory using endpoint - introduce a new uploadDir endpoint that accepts a raw json file with the CID or metadata generated from the frontend

- We can either create the CID for each file and directory
- We can also base64 the files and pass them as is on the uploadDir endpoint (limitation: size)

## Endpoints

- /directory
- /directory/file
- /directory/upload - pass a json object with name and base64encoded string of each file. Note that a base64encoding has a limit of 192mb only. Which is more than enough for most websites.

## Testing

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  SQL File to add the new table
- [ ]  Swagger documentation changes
- [ ]  Documentation changes