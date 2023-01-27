# Proposal: Automated Market Maker

| Author | Jay Logelin |
| --- | --- |
| Status | Draft |
| Revision | 0.0.1 |

## Proposal/Overview
A decentralized exchange (DEX) is a platform for trading digital assets without the need for a centralized intermediary. One popular type of DEX is an Automated Market Maker (AMM) which uses smart contracts to automatically match buyers and sellers based on pre-determined rules. This design outlines a smart contract-based system that facilitates liquidity provision and exchange on an AMM DEX.

## Benefits
- Decentralized: The system operates on a decentralized blockchain, eliminating the need for a centralized intermediary and providing a more secure and transparent trading experience.
- Automated: The system uses smart contracts to automate the trading process, eliminating the need for manual intervention and reducing the risk of errors.
- Liquidity: The system allows for liquidity providers to add funds to the pool, increasing the liquidity of the exchange and allowing for more efficient trading.

## Goals:
- Facilitate liquidity provision and exchange on an AMM DEX
- Ensure security and transparency of the trading process
- Provide a user-friendly interface for traders

## Design Overview:
- The system consists of two main components: the liquidity pool smart contract and the trading interface.
- The liquidity pool smart contract is responsible for managing the funds in the pool and executing trades based on pre-determined rules.
- The trading interface is a front-end application that allows traders to interact with the liquidity pool smart contract and execute trades.
- The system also includes a token contract that defines the rules for the tokens being traded.

## Detailed Design:
- The liquidity pool smart contract is implemented as a standard ERC20 token contract with additional functionality for managing the liquidity pool.
- The contract includes functions for adding and removing liquidity, and for executing trades based on the current liquidity and token prices.
- The trading interface is a web application that allows traders to interact with the liquidity pool smart contract by sending transactions to the contract.
- The interface allows traders to view the current token prices, add or remove liquidity, and execute trades.

### Example Implementation

```solidity
pragma solidity ^0.8.0;

contract TokenSwap {
    mapping(address => uint256) public balanceOf;

    function addLiquidity(address _token, uint256 _amount) public payable {
        require(_amount > 0);
        require(address(this).balance >= _amount);
        balanceOf[_token] += _amount;
        _token.transfer(_amount);
    }

    function removeLiquidity(address _token, uint256 _amount) public {
        require(balanceOf[_token] >= _amount);
        balanceOf[_token] -= _amount;
        _token.transfer(_amount);
    }

    function swap(address _fromToken, address _to token, uint256 _amount) public {
        require(balanceOf[_fromToken] >= _amount);
        uint256 _fromPrice = fetchPrice(_fromToken);
        uint256 _toPrice = fetchPrice(_toToken);
        uint256 _toAmount = _amount * _toPrice / _fromPrice;
        balanceOf[_fromToken] -= _amount;
        balanceOf[_toToken] += _toAmount;
        _fromToken.transfer(_amount);
        _toToken.transfer(_toAmount);
    }
    
    function fetchPrice(address _token) public view returns (uint256) {
        return oraclize_query("URL", _token.symbol());
    }
```

## Dependencies
- FEVM blockchain: The system runs on the FEVM blockchain and requires a connection to the FEVM network.
- Web3.js: The trading interface uses web3.js to interact with the FEVM network and send transactions to the liquidity pool smart contract.
- Oraclize: the smart contract uses Oraclize to fetch the current prices of tokens from a centralised oracle.

## Security Implications
- The system relies on the security of the FEVM blockchain to ensure the safety of funds in the liquidity pool.
- The smart contract code should be audited to ensure it is free from vulnerabilities.
- The trading interface should be designed to prevent phishing attacks and other malicious activities.

## Project Deliverables / Definition of Done
- [ ] Smart contract code for the liquidity pool and token contracts
- [ ] A trading interface for users to interact with the smart contract
- [ ] A detailed documentation for the system, including instructions for deployment and usage
- [ ] A security audit report for the smart contract code

_Note: The example code is just an example, It's not tested or audited and it's not suitable for production use._
