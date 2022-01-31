# About System Contracts

The EOSIO blockchain platform is unique in that the features and characteristics of the blockchain built on it are flexible, that is, they can be changed, or modified completely to suit each business case requirement. Core blockchain features such as consensus, fee schedules, account creation and modification, token economics, block producer registration, voting, multi-sig, etc., are implemented inside smart contracts which are deployed on the blockchain built on the EOSIO platform.

## System contracts defined in eosio.contracts

AirWire implements and maintains EOSIO open source platform which contains, as an example, the system contracts encapsulating the base functionality for an EOSIO based blockchain.

1. [eosio.bios]
2. [eosio.system]
3. [eosio.msig]
4. [eosio.token]
5. [eosio.wrap]

## Key Concepts Implemented by eosio.system

1. [System]
2. [RAM]
3. [CPU]
4. [NET]
5. [Stake]
6. [Vote]

## Build and deploy

To build and deploy the system contract follow the instruction from Build and deploy section.

[eosio.bios]:action-reference/eosio.bios.md
[eosio.system]:action-reference/eosio.system.md
[eosio.msig]:action-reference/eosio.msig.md
[eosio.token]:action-reference/eosio.token.md
[eosio.wrap]:action-reference/eosio.wrap.md

[System]:key-concepts/system-contract-and-accounts,-privileged-accounts.md
[RAM]:key_concepts/RAM-as-resource.md
[CPU]:key_concepts/CPU-as-resource.md
[NET]:key_concepts/NET-as-resource.md
[Stake]:key_concepts/staking-on-EOSIO-based-blockchains.md
[Vote]:key_concepts/voting-on-EOSIO-based-blockchains.md