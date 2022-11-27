# Proposal: Estuary sidecar

| Author | Anjor |
| --- | --- |
| Status | Draft |
| Revision |  |

## Proposal/Overview

A sidecar backend service that runs alongside estuary and provides “quality of life” API. Examples of API include:

- Easier upload endpoints such as `/content/add/url` that takes a body `{"url": <url hosting data>}`
- Versioning on top of data: see [https://github.com/application-research/pestuary/blob/main/src/pestuary/versioned_uploads/versioned_uploads.py](https://github.com/application-research/pestuary/blob/main/src/pestuary/versioned_uploads/versioned_uploads.py)

## Consideration

## Technical Design

## Testing

## Edge cases

## Deliverables / Definition of Done

- [ ]  Code changes (Code/UT)
- [ ]  Documentation changes
- [ ]  Client integrations