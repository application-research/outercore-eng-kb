# Proposal: FEVM Hot Storage Protocol

| Author | Jay Logelin |
| --- | --- |
| Status | Draft |
| Revision | 0.0.1 |

## Proposal/Overview

This smart contract allows for the storage and retrieval of files on the InterPlanetary File System (IPFS) using the Ethereum blockchain. The smart contract will be deployed on the Ethereum network and will allow users to upload and access files stored on IPFS using their Ethereum address.

## Functionality

Users can upload files to Estuary/IPFS and store the IPFS hash on the FEVM blockchain using the smart contract.
Users can retrieve files from Estuary/IPFS using the IPFS file hash stored on the FEVM blockchain.
Users can also update or delete their previously uploaded files.
The smart contract will have a function to verify the authenticity of the file hash to prevent tampering.
Technical Details
The smart contract will be written in Solidity and will be deployed on the FEVM network.
The smart contract will make use of the IPFS API to interact with the IPFS network.
The smart contract will store the IPFS file hash and the corresponding FEVM address of the user who uploaded the file in a mapping.
The smart contract will include a function to verify the authenticity of the file hash using a cryptographic hash function.

## IPFS Retrieval Function
The IPFS retrieval function will allow users to retrieve files from the IPFS network using the IPFS file hash stored on the FEVM blockchain.

The function will take the IPFS file hash as input, verify the authenticity of the file hash using the cryptographic hash function, and then retrieve the file from the IPFS network using the IPFS API.

The function will return the file data to the user.

The function will also check the FEVM address of the user calling the function to ensure that they are the owner of the file, before returning the file data.

The function will be called `getFile(string _ipfsHash) public view returns (bytes memory)`

The user need to call getFile(ipfsHash) function with the correct ipfs hash of the file they want to retrieve, and the function will check the authenticity of the hash and return the bytes of the file if the hash is correct and the user is the owner of the file.

In case the hash is incorrect or the user is not the owner of the file, the function will return an error message.

## Layer 2 Retrieval Verification Network
A layer 2 retrieval verification network will be built on top of the FEVM smart contract to improve the scalability and performance of file retrieval.

The network will use a sharding mechanism to distribute the retrieval and verification tasks among multiple nodes.

The network will use a consensus algorithm such as Proof of Stake (PoS) to ensure that the retrieval and verification tasks are executed correctly and to prevent malicious nodes from tampering with the file data.

The layer 2 network will consist of a set of validator nodes that will be responsible for verifying the authenticity of the file hash and retrieving the file data from IPFS.

The validator nodes will communicate with the smart contract to obtain the IPFS file hash and the EVM address of the user requesting the file.

Once the authenticity of the file hash is verified, the validator node will retrieve the file data from IPFS and return it to the user.

The user will call the smart contract function to retrieve the file and the smart contract will redirect the request to the layer 2 network.

The layer 2 network will then assign the retrieval and verification task to one of the validator nodes.

Once the validator node completes the task, it will return the file data to the user.

This design will help to improve the scalability and performance of file retrieval, as the layer 2 network will handle the retrieval and verification tasks, while the FEVM smart contract will handle the storage and access control of the files.

The layer 2 network will also provide a more secure way of file retrieval as it will use a consensus algorithm and sharding mechanism to prevent malicious nodes from tampering with the file data.

The smart contract will have to be updated with an additional function to handle the redirection of the retrieval request to the layer 2 network, and the layer 2 network will have to be deployed and integrated with the smart contract.

_What exactly are you trying to do? Why? These should be measurable and specific._

## Security Considerations
The smart contract will be tested and audited to ensure that it is free from vulnerabilities.
Access to the smart contract functions will be restricted to authorized users using access control mechanisms.
The smart contract will include a function to check the authenticity of the file hash to prevent tampering.
Deployment and Maintenance
The smart contract will be deployed on the FEVM network using a deployment tool such as Truffle.
The smart contract will require regular maintenance to ensure that it remains secure and functional.
The smart contract will be versioned to allow for updates and bug fixes to be deployed.
User Interface
Users can interact with the smart contract using a web3 enabled browser or a wallet that support smart contract interaction like MetaMask.
A simple user interface will be built to make it easy for users to interact with the smart contract and perform actions such as uploading and retrieving files.
The user interface will display the IPFS file hash and the FEVM address of the user who uploaded the file.
The user interface will also include a function to verify the authenticity of the file hash.

## Alternatives Considered

retriev.org provides a similar set of functions

## Conclusion
This smart contract allows for the storage and retrieval of files on IPFS using the FEVM blockchain. It provides a secure and decentralized way for users to store and access files, while also allowing for the verification of the authenticity of the file hash to prevent tampering. The smart contract is easy to use and provides a simple user interface for interacting with the IPFS network.

## Deliverables / Definition of Done

- [ ]  Implementation Code in solidity (Code/Unit tests/Integration tests)
- [ ]  CICD changes (Automated builds, testing, delivery)
- [ ]  Middleware network code using libp2p
- [ ]  Documentation
