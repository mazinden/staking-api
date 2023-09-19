# Ethereum Staking Overview

Staking API enables staking in the Ethereum network.

To start using Staking API:

1. [Get an authentication token](doc:authentication).
2. Prepare `id` that is an arbitrary UUID. Generate it in one of the following ways:

    - [Online UUID Generator](https://www.uuidgenerator.net/)
    - [uuid npm package](https://www.npmjs.com/package/uuid)

The staking process consists of several steps:

1. Create Staking Request: set up nodes for staking using P2P infrastructure.
2. Check Request Status: check the status of the node set-up operation.
3. Prepare Staking Transaction: construct a serialized transaction to deposit the stake amount with a P2P smart contract.
4. Sign and broadcast the transaction.

We provide two distinct endpoints for testing and production environments. The test environment is linked to the Goerli network (Ethereum).

- PRODUCTION: https://api.p2p.org
- TESTING: https://api-test.p2p.org

## What's Next?

- [Staking]().
- [Check Status]().
- [Withdrawal]().
- [Staking API](ref:ethereum) reference.
