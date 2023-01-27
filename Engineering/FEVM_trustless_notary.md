# Proposal: FEVM Trustless Notary

| Author | Jay Logelin |
| --- | --- |
| Status | Draft |
| Revision | 0.0.1 |

## Overview:
This smart contract design serves as a governance token for the Filecoin network, enabling validators to verify the validity of content being added to the network. It also provides an ERC20 reward token at the end of a Filecoin storage deal as an incentive for Storage Providers.

## Benefits:

- Provides a decentralized and transparent way for validating content on the Filecoin network
- Incentivizes Storage Providers through the use of a reward token
- Enables a more efficient and secure storage network through the use of smart contract technology

## Goals:

- Ensure validity of content being added to the Filecoin network
- Encourage participation from Storage Providers
- Improve the overall security and efficiency of the storage network

## Design Overview:
The smart contract will be implemented on the FEVM blockchain and will have the following functions:

- Verify content: Validators can use this function to verify the validity of content being added to the Filecoin network
- Claim reward: Storage Providers can use this function to claim the ERC20 reward token at the end of a successful storage deal
- Governance: Token holders can use this function to vote on and make decisions regarding the management of the Filecoin network

## Detailed Design:

- The smart contract will be based on the ERC20 standard, with additional functionality for content verification and governance.
- Tokens will be distributed to validators and Storage Providers based on their contributions to the network.
- A voting mechanism will be implemented for token holders to make decisions regarding the management of the Filecoin network.

### Layer 2 Content Validation Network:
To improve the efficiency and scalability of the content verification process, a layer 2 content validation network will be implemented in addition to the smart contract on the FEVM blockchain. This network will consist of a group of validators who are responsible for verifying the content before it is added to the Filecoin network.

- The validators will be selected through a decentralized and transparent process, such as a staking mechanism, to ensure impartiality and security.
- The validators will use a consensus algorithm, such as Proof of Stake, to reach a decision on the validity of the content.
- The layer 2 network will communicate with the smart contract on the FEVM blockchain to update the status of the content and transfer the reward tokens.
- The layer 2 network will also be responsible for maintaining a decentralized and tamper-proof record of all the content that has been verified.

This additional layer 2 validation network will provide several benefits:

- Improve the scalability of the content verification process
- Reduce the load on the FEVM blockchain
- Enhance the security and transparency of the content verification process
- Provide more opportunities for community members to get involved and contribute to the Filecoin network.

The smart contract will be updated to include the layer 2 validation network and all the functions that will be added to the smart contract, such as the functions to verify and claim rewards, will be integrated with the layer 2 validation network.

The smart contract will still be responsible for storing all the information about the filecoin network and the reward tokens, it will only be used to store and transfer the tokens, the layer 2 validation network will take care of the content validation process.

#### Pseudocode Implementation

```go
package main

import (
	"context"
	"fmt"
	"log"

	"github.com/libp2p/go-libp2p"
	"github.com/libp2p/go-libp2p-core/crypto"
	"github.com/libp2p/go-libp2p-core/peer"
)

func main() {
	// Create a new libp2p node
	node, err := libp2p.New(context.Background(), libp2p.Identity(crypto.GenerateECPair()))
	if err != nil {
		log.Fatal(err)
	}

	// Start the node
	if err := node.Start(); err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Node created with peer ID: %s\n", node.ID())

	// Set up a listener for incoming content
	node.SetStreamHandler("/notary/content", handleContent)

	// Connect to other validators in the network
	peerID, _ := peer.Decode("QmXg9aJNGVH4UxUfjHXKfQmQQNuNgwM6EJtMZKsNxUJ9fN")
	node.Connect(context.Background(), peer.AddrInfo{ID: peerID})

	select {}
}

func handleContent(stream network.Stream) {
	// Verify the content
	// ...

	// If content is valid, broadcast it to the network
	// ...
}

```

This example demonstrates how to use libp2p to create a new node, set up a listener for incoming content, and connect to other validators in the network. The handleContent function is where the content validation process takes place. In this example it just a placeholder, but you could use other libraries or methods to validate the content before broadcasting it to the network.

The `node.Connect(context.Background(), peer.AddrInfo{ID: peerID})` is used to connect to the other validators in the network. You could use a different method for discovering peers, such as using a DHT.

Please note that this is just an example, it does not include all the functionality that you could add to a real implementation, also it's missing the consensus algorithm and the process to select the validators.

## Dependencies:

- FEVM blockchain
- ERC20 standard

## Security Implications:

- The smart contract code should be audited to ensure that it is secure and free of bugs.
- The smart contract should be deployed on a secure network, such as the main FEVM network.
- The smart contract should be designed to prevent unauthorized access and manipulation.

## Example Implementation:

```solidity
pragma solidity ^0.8.0;

contract FilecoinGovernance {
    address payable owner;
    mapping(address => uint) public tokens;

    constructor() public {
        owner = msg.sender;
    }

    function verifyContent(bytes32 contentHash) public {
        require(msg.sender == owner, "Unauthorized access");
        // Verify the validity of the content
    }

    function claimReward(address provider) public {
        require(msg.sender == provider, "Unauthorized access");
        // Transfer ERC20 reward token to the provider
    }

    function governanceVote(uint vote) public {
        require(tokens[msg.sender] > 0, "You do not have any tokens");
        // Execute the vote
    }
}
```

## Deliverables / Definition of Done

- [ ]  Smart contract code
- [ ]  Unit tests for the smart contract
- [ ]  Deployment instructions
- [ ]  Security audit report
- [ ]  Documentation for the smart contract and its functions.


