# Proposal: FEVM Hot Storage Protocol

| Author | Jay Logelin |
| --- | --- |
| Status | Draft |
| Revision | 0.0.1 |

## Proposal/Overview

This smart contract allows for the storage and retrieval of files on the InterPlanetary File System (IPFS) using the FEVM blockchain. The smart contract will be deployed on the FEVM network and will allow users to upload and access files stored on IPFS using their Ethereum address. Additionally, a simple layer 2 network using [Dynamic Proofs of Retrievability](https://www.usenix.org/conference/usenixsecurity21/presentation/anthoine#:~:text=Proofs%20of%20Retrievability%20(PoRs)%20are,small%20portion%20of%20the%20data.) will be used to validate storage across the network.

## Design

A decentralized hot storage network with IPFS and dynamic proofs of retrievability, you would need to take the following steps:

1. Use IPFS to handle the peer-to-peer communication and file storage on the network. IPFS allows for efficient and decentralized file storage and retrieval by breaking files into smaller pieces and distributing them across the network.

2. Implement dynamic proofs of retrievability (PoR) to ensure the integrity and availability of the stored files. Dynamic PoR is a technique that allows nodes to prove they have a certain file by providing a small, randomly-selected subset of the file's data, rather than the entire file.

3. Use smart contracts on a blockchain platform, such as Ethereum, to track the storage and retrieval of files on the network. The smart contract would function as a decentralized storage marketplace, where nodes can offer their storage space to the network and get rewarded for their contributions.

4. Utilize the smart contract to enforce the dynamic PoR, by requiring that the files be retrieved from multiple nodes and verified by the smart contract. This ensures that the files are still being stored and that the nodes are providing valid PoR.

5. Implement reputation system in smart contract to ensure that only reliable nodes are able to participate in the network and receive rewards. This can be done by keeping track of the storage and retrieval performance of each node and providing a score that can be used to determine their reputation.

6. Create an incentivization mechanism that rewards nodes for contributing storage to the network and for providing valid dynamic PoR. This can be done by paying nodes in a cryptocurrency or token that is native to the blockchain platform being used.

### Dynamic Proofs of Retrievability (PoR)

Dynamic proofs of retrievability (PoR) is a technique used to ensure the integrity and availability of data stored on a decentralized storage network. The main idea behind dynamic PoR is to allow nodes to prove they have a certain file by providing a small, randomly-selected subset of the file's data, rather than the entire file.

### Security
The security of dynamic PoR is based on two main principles:

1. Data fragmentation: By breaking the file into smaller pieces and distributing them across the network, it becomes much more difficult for an attacker to tamper with or delete the data.

2. Challenge-response verification: The smart contract on the blockchain can randomly select a subset of the file's data, called a challenge, and ask the node storing the file to provide the corresponding response. If the response is correct, it proves that the node has the file and it's intact.

Dynamic PoR also includes the use of a Merkle Tree for the file fragments, this allows for the verification of data integrity without having to retrieve the entire file, only the leaves of the tree need to be verified.

Additionally, dynamic PoR can also be combined with other security measures such as replication, erasure coding and node reputation systems to further increase the security of the network.

Overall, dynamic PoR provides a robust and efficient way to ensure the integrity and availability of data stored on a decentralized storage network, making it more resistant to tampering and deletion by malicious actors.

### Retrieval Function
The IPFS retrieval function will allow users to retrieve files from the IPFS network using the IPFS file hash stored on the FEVM blockchain.

The function will take the IPFS file hash as input, verify the authenticity of the file hash using the cryptographic hash function, and then retrieve the file from the IPFS network using the IPFS API.

The function will return the file data to the user.

The function will also check the FEVM address of the user calling the function to ensure that they are the owner of the file, before returning the file data.

The function will be called `getData(string _ipfsHash) public view returns (bytes memory)`

The user need to call getData(ipfsHash) function with the correct ipfs hash of the file they want to retrieve, and the function will check the authenticity of the hash and return the bytes of the file if the hash is correct and the user is the owner of the file.

In case the hash is incorrect or the user is not the owner of the file, the function will return an error message.

### Layer 2 Retrieval Verification Network
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

### Proof of retrievability
The basic structure of the network would involve using libp2p to handle peer-to-peer communication, verification and file transfer between nodes. Each node would be responsible for maintaining a certain amount of storage and be rewarded for contributing storage space to the network.

To establish a proof of retrievability, a smart contract on the Ethereum blockchain could be used to track the storage and retrieval of files on the network. The smart contract would function as a decentralized storage marketplace, where nodes can offer their storage space to the network and get rewarded for their contributions. The smart contract would also be responsible for enforcing the proof of retrievability, by requiring that the files be retrieved from multiple nodes to ensure their integrity and availability.

In addition, the smart contract can also be used to track the reputation of nodes on the network, ensuring that only reliable nodes are able to participate in the network and receive rewards.

Overall, using libp2p for peer-to-peer communication and file transfer and Ethereum smart contracts for decentralized storage marketplaces and proof of retrievability can be a powerful combination for building a decentralized, secure, and reliable network for large file storage and retrieval.

## Components

A dynamic PoR storage network typically has the following components:

1. Nodes: These are the devices that participate in the network and offer their storage space to store files.

2. File fragmentation: Files are broken down into smaller pieces, called fragments, and these fragments are distributed across the network to different nodes.

3. Merkle Tree: A Merkle tree is constructed from the fragments, which allows for the efficient verification of data integrity without having to retrieve the entire file.

4. Smart Contract: A smart contract on a blockchain platform is used to track the storage and retrieval of files on the network. It also enforces the dynamic PoR and manages the reputation of nodes.

5. Network communication: Nodes communicate with each other through a peer-to-peer network protocol, such as IPFS, to transfer files and verify proofs of retrievability.

6. Incentivization: An incentivization mechanism rewards nodes for contributing storage to the network and for providing valid dynamic PoR.

## Smart Contract Security Considerations
- The smart contract will be tested and audited to ensure that it is free from vulnerabilities.
- Access to the smart contract functions will be restricted to authorized users using access control mechanisms.
- The smart contract will include a function to check the authenticity of the file hash to prevent tampering.

## Deployment and Maintenance
- The smart contract will be deployed on the FEVM network using a deployment tool such as Truffle.
- The smart contract will require regular maintenance to ensure that it remains secure and functional.
- The smart contract will be versioned to allow for updates and bug fixes to be deployed.

## User Interface
- Users can interact with the smart contract using a web3 enabled browser or a wallet that support smart contract interaction like MetaMask.
- A simple user interface will be built to make it easy for users to interact with the smart contract and perform actions such as uploading and retrieving files.
- The user interface will display the IPFS file hash and the FEVM address of the user who uploaded the file.
- The user interface will also include a function to verify the authenticity of the file hash.

## Alternatives Considered

retriev.org provides a similar set of functions for the layer 2 network, with a retrieval slashing/verification mechanism.

## Conclusion
This smart contract allows for the storage and retrieval of files on IPFS using the FEVM blockchain. It provides a secure and decentralized way for users to store and access files, while also allowing for the verification of the authenticity of the file hash to prevent tampering. The smart contract is easy to use and provides a simple user interface for interacting with the IPFS network.

## Deliverables / Definition of Done

- [ ]  Implementation Code in solidity (Code/Unit tests/Integration tests)
- [ ]  CICD changes (Automated builds, testing, delivery)
- [ ]  Middleware network code using libp2p
- [ ]  Documentation

## References
Anthoine, G et al. [Dynamic proofs of retrievability with low server storage](https://www.usenix.org/conference/usenixsecurity21/presentation/anthoine#:~:text=Proofs%20of%20Retrievability%20(PoRs)%20are,small%20portion%20of%20the%20data). 
