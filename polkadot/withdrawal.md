# Withdrawal

The withdrawal process in the Polkadot network using the Staking API can be done in several ways:

1. Submit Unbond: unbonding tokens within the Polkadot network that were previously staked or bonded.

   > It takes 7 days (28 eras) to unbond on Kusama, 28 days (28 eras) on Polkadot, and 12 hours (2 eras) on Westend.

2. Withdrawal (available after unbond period): withdrawing tokens within the Polkadot network that were previously unbonded.

## 1. Unbond

1. Submit Unbond: send a POST request to [/api/v1/polkadot/{network}/staking/unbond](ref:polkadot-staking-unbond).

   Example request (for `westend`):

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/polkadot/westend/staking/unbond \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
   {
     "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
     "amount": 3
   }'
   ```

   - `stashAccountAddress` — main stash account address which keeps tokens for bonding.
   - `amount` — amount of tokens to unbond. In usual DOT for main network, KSM for Kusama, and WND for Westend.

   Example response:

   ```curl
   {
     "result": {
       "unsignedTransaction": "0xac0406000b00487835a302032c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f",
       "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
       "amount": 3,
       "createdAt": "2023-08-15T15:07:54.795Z"
     }
   }
   ```

   - `unsignedTransaction` — unsigned transaction in hex format. Sign the transaction and submit it to the blockchain to perform the called action.
   - `stashAccountAddress` — main stash account address which keeps tokens for bonding.
   - `amount` — amount of tokens for bond operations. In usual DOT for main network, KSM for Kusama, and WND for Westend.
   - `createdAt` — timestamp of the transaction in the ISO 8601 format.

2. [Sign and broadcast](doc:signing-transaction-polkadot) `unsignedTransaction` to the Polkadot network.

## 2. Withdrawal

1. Withdrawal Unbonded: send a POST request to [/api/v1/polkadot/{network}/staking/withdrawal-unbonded](ref:polkadot-staking-withdrawunbonded).

   Example request (for `westend`):

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/polkadot/westend/staking/withdrawal-unbonded \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
   {
     "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5"
   }'
   ```

   - `stashAccountAddress` — main stash account address which keeps tokens for bonding.

   Example response:

   ```curl
   {
     "result": {
       "unsignedTransaction": "0xac0406000b00487835a302032c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f",
       "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
       "createdAt": "2023-08-15T15:07:54.795Z"
     }
   }
   ```

   - `unsignedTransaction` — unsigned transaction in hex format. Sign the transaction and submit it to the blockchain to perform the called action.
   - `stashAccountAddress` — main stash account address which keeps tokens for bonding.
   - `createdAt` — timestamp of the transaction in the ISO 8601 format.

2. [Sign and broadcast](doc:signing-transaction-polkadot) `unsignedTransaction` to the Polkadot network.

## What's Next?

- [Getting Started](doc:staking-polkadot-public).
- [Staking API](ref:polkadot) reference.