# Reference Architectures and Designs

**Estuary:**

![Estuary building components.jpg](Reference%20Architectures%20and%20Designs%2048971c708b444590822f6646478aadc0/Estuary_building_components.jpg)

Estuary Building Blocks: 

1- A client can send data to Estuary via the Estuary API:

- An HTTP request, e.g. curl, javascript
- or using Barge, a tool that manages incremental updates from source to Estuary, splitting (future) of files larger than 32GiB, encryption (future) before sending it to Estuary.

2- Estuary Primary node is a full IPFS node and an auto-dealmaker for Filecoin. Its primary function is to make deals, and manage them. e.g. checks if all files stored by Estuary have six copies, and all deals are valid (not failed, or faulted).

3- Estuary Shuttle nodes are spread in different region, and take some of the resource intensive operations off the primary node: a- Staging of data from clients, b- Hashing, c- Sending data to providers. 

**Client hosted Estuary solution options:**

1- Run their own primary node:

- Control the providers they want to store data to.
- Make their own deals out of their own wallets.

2- Run a private shuttle node:

- A shuttle node will act as a local IPFS node for hot data. (limitations to local storage are still to be explored).
- The shuttle node can be run  in private mode. A primary node will never delegate tasks to a private shuttle node.
- The shuttle node will use the primary node to make deals and select providers.

**Minimum hardware spec:**

24core 2.2GHz Process

192GB DDR4 RAM

Primary node: 96TB for staging* (currently set as a raid 10: usable is 50%, this doesn't have to be the case of course).

Shuttle node: all NVMe node with 10TiB capacity. This seems to perform very well. 

SSD for Database (size won't matter, better to have at least 480GB minimum)