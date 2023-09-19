# Polkadot Staking Overview

Staking API enables staking in the Polkadot network.

[Get an authentication token](doc:authentication) to start using Staking API.

The staking process consists of several steps:

1. Submit Bond: submitting a bond to the Polkadot network in exchange for some other benefit.
2. Sign transaction: sign transaction before broadcasting.
3. Send transaction: broadcast transaction to the Polkadot network.
4. Submit Nomination: the action of choosing validators within the Polkadot network.

We provide two distinct endpoints for testing and production environments.

- PRODUCTION: https://api.p2p.org
- TESTING: https://api-test.p2p.org

For Polkadot available several networks:

- `mainnet` — the main network.
- `kusama` — a canary network that holds real economic value.
- `westend` — a testnet.

## What's Next?

- [Staking Bond]().
- [Unbond]().
- [Withdrawal]().
- [Proxy Accounts]().
- [Staking API](ref:ethereum) reference.
