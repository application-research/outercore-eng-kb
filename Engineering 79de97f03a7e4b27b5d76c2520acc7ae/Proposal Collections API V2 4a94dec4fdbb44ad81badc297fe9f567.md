# Proposal: Collections API V2

| Author | Outercore Engineering |
| --- | --- |
| Status | Draft |
| Revision |  |

**This is WIP**

## Proposal/Overview

The collections API is seemingly used as a directory upload rather than a grouping mechanism and I think we need to create a distinction between the two use cases. A directory in the IPFS sense is a collection of specific type of node (DirNode) that can be associated with different links (ChildNode). The DirNode creates the initial underlying structure of the merkle which then can be used by the developer to programmatically add links to it to form a “directory” structure in a merkle dag format. This merkle dag then is stored on the estuary node blockstore via go-blockstore library. 

To this day, we observed that most users who uses collections api treat them as directories and use them as such. This is clearly not the case since it won’t perform the same as a directory. primarily because the current version lacks all the endpoints to manage a collection “as a” directory. 

## Solution

We need to have a distinction between collections API and directories and my proposal is to re-vamp the collections API as a tagging mechanism as oppose to a directory system. We should create a new API for directories which will be similar to `ipfs add -r` command and use DirNode for directories.

For collections, we should just group them on the database level (postgres) and make sure they are retrievable via an endpoint. The creation of a new merkledag for this approach is completely optional since the source of “grouping” will be from the database. 

## Assumptions

- Collections API will only live in the context of Estuary. Source of truth of the grouping is on postgres database.
- The Collections API will not be propagated to any other storage provider services similar to Estuary since it’ll be a unique feature only to Estuary.

## Technical Design

### Descriptive

- create a tag
    - insert database entry to collections table with type = tag, tag column = tag name
- add a content to a tag
    - get tag name
    - query database with tag name
        - validate if exist. if it doesn’t exist. create a new tag with the name
    - insert database entry on collections table with type = child and tag column = tagname
- add list to a tag (cid or content id)
    - get tag name
    - query database with tag name
        - validate if exist. if it doesn’t exist. create a new tag with the name
    - loop through the list
        - For each child
            - insert database entry on collections table with type = child and tag column = tagname

## Endpoints

- /collections/tag
- /collections/tag/:tagname/:content - content
- /collections/tag/:tagname/contents - list of content
- /collections/tag/:tagname/cid - list of content
- /collections/untag/:tagname/:content - content
- /collections/untag/:tagname/contents - list of content
- /collections/untag/:tagname/cid - list of content
- /collections/commit/:tag - tag
- /collections/download/:tag - tag
- /collections/search/tag/:tag - search key words
- /collections/search/ - search keywords

## Testing

- User scenarios
    - As a user I want to manage a tag
        - Create
        - Delete
        - Update
        - Rename
    - As a user, I want to add content
        - add a single content using estuary content id
        - add a list of content using estuary content id
        - add a single content using CID
        - add a list of content using collection of CID
    - As a user, I want to unpin content
        - unpin already added content
    - As a user, I want to commit a tag
    - As a user I want to search
        - Search by tag name
        - Search by content id
        - Search by CID
    - As a user I want to download
        - Download by tag name
        - Download by content id
        - Download by CID

## Open Items

- All transactions will include a database interaction and blockstore
- We can introduce an uncommit with a given CID.

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  SQL File to add the new table
- [ ]  Swagger documentation changes
- [ ]  Documentation changes

## Collections API Call (mike, gabe, lawrence, alvin)

- we need directories first
- add file manager (create folder structure on backend)
    - user can CRUD folders
    - maybe symlinks (good to have)
- database mocks what user sees as a directory
- background worker to create ipfs
- tagging: use ipfs metadata tag