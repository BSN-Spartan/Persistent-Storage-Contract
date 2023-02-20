# Persistent Storage

[![Smart Contract](https://badgen.net/badge/smart-contract/Solidity/orange)](https://soliditylang.org/)

Persistent storage plays a major role in IT systems and it has four basic operations: create, read, update, and delete (CRUD). With the continuous improvement of IT system, diverse technologies have been integrated into data storage, and blockchain is one of them. Blockchain is not a technology, but a technology set. It is continuously improving, but its advantages and disadvantages are also distinct.

Every blockchain application interacts with its original IT system, or we can compare the traditional IT system to the "executor" of blockchain application. On one hand, the use of blockchain technology can optimize the traditional IT system and solve tough problems; on the other hand, the traditional IT system can supplement the shortcomings of blockchain technology. Complementing each other's strengths will inevitably lead to a better result.

## Prerequisite

Before using a smart contract, it is important to have a basic understanding of Ethereum and Solidity.

## Overview

The contract has imported Openzeppelin's Ownable library, and makes the modifier onlyOwner available to the functions in the contract to ensure that these functions only support calls from the contract owner (the account deploying the contract).

When the IT system is updated iteratively, the contract may be updated along with it. Updates to the contract must ensure that they do not affect existing data. To meet this requirement, the contract must be designed with a good separation between the code that processes business logic and the code that processes data.

There are various types of contract upgrade, and transparent proxy and UUPS are widely used. This contract adopts UUPS mode, the contract structure has an additional proxy contract. Users can call all functions in the proxy contract. For both deployment and upgrade, the owner of the contract should deploy the proxy contract only after the logic contract has been successfully deployed.

Usually, the use of contracts needs to be secured. In this case, Openzeppelin's Pausable library is referenced within the contract, allowing the owner of the contract to suspend everyone from continuing to invoke the contract if a contract vulnerability or flaw is found.

## Usage

Get the smart contract from [GitHub](https://github.com/BSN-Spartan/Persistent-Storage-Contract/tree/main/storage), or get the source code by command:

```
$ git clone https://github.com/BSN-Spartan/Persistent-Storage-Contract.git
```

For beginners, the contracts in this application can be deployed by the steps in [Spartan Quick Testing](https://www.spartan.bsn.foundation/main/quick-testing#step1).

In this application, there are two contracts: logic and ERC1967Proxy. Follow steps below to deploy and use them:

### Contract Deployment

1. Deploy the logic contract.
2. Deploy ERC1967Proxy contract. When deploying the contract, input the address of the logic contract in the first blank and input the parameter "0x8129fc1c" in the second blank. "0x8129fc1c" is the signature of initialize() function, which can achieve the initialization operation and the mapping between the proxy contract and the logic contract.
3. For Remix IDE, after successfully deploying the logic contract and the proxy contract, compile the logic contract again and find the "At Address" button in the deployment section. Fill in the address of the proxy contract and click the button. Then, all functions in the logic contract are listed in the proxy contract and are ready to use.

### Contract Upgrade

1. Deploy the new logic contract.
2. Call "upgradeTo" function in the proxy contract. The parameter of this function is the address of the new logic contract.
3. For Remix IDE, after successfully upgrading the contract by "upgradeTo" function, compile the new logic contract again and find the "At Address" button in the deployment section. Fill in the address of the proxy contract and click the button. Then, all functions in the new logic contract are listed in the proxy contract and are ready to use.


### Contract Calls

After deploying a contract, users can cease(pause), start(unpause), and upgrade(upgradeTo) the contract by calling each functions, and commit business data to the chain for data storage (storeData), deletion (deleteData), update (storeData), and query (queryData).

* Store: The business system defines the key, and input the business data as value. Then call "storeData" function to commit the data to the chain.
* Delete: Specify the key of the business data to be deleted in the business system, and call "deleteData" function to delete the data.
* Update: Specify the key of the business data to be updated in the business system, and input the new business data as value. Then, call "storeData" function to finish updating the data.
* Query: Specify the key of the business data to be queried in the business system, and call "queryData" function to find out the business data stored on the chain.


## Main Functions

```
function storeData(string memory key,string memory value) public onlyOwner whenNotPaused
```

Store or update the data in the form of key-value.

```
function deleteData(string memory key) public onlyOwner whenNotPaused
```

Delete the value of the data by its corresponding key.

```
function queryData(string memory key) public view whenNotPaused returns (string memory)
```

Retrieve the value of the data by its corresponding key.

## License

Spartan Persistent Storage Contract is released under the [Spartan License](https://github.com/BSN-Spartan/Beginner-Level-Contracts/blob/main/Spartan%20License.md).
