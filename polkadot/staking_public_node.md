# Getting Started

The staking process for Polkadot in the Staking API with a public node consists of several main steps:

1. Submit Bond: Submit a bond to the Polkadot network in exchange for certain benefits.
2. Submit Nomination: The process of selecting validators within the Polkadot network.

[Get an authentication token](doc:authentication) to start using Staking API.

A request example is provided using [cURL](https://curl.se/).

## 1. Submit Bond

1. Send a POST request to [/api/v1/polkadot/{network}/staking/bond](ref:polkadot-staking-bond).

   Example request (for `westend` network):

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/polkadot/westend/staking/bond \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
   {
     "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
     "rewardDestinationType": "account",
     "rewardDestination": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
     "amount": 3
   }'
   ```

  - `stashAccountAddress` — main stash account address which keeps tokens for bonding.

  - `rewardDestinationType` — rewards destination type:

    - `staked` — Rewards will be sent to your Stash account and added to your current bond (compounding rewards).
    - `stash` — Rewards will be sent to your Stash account as transferrable balance (not compounding rewards).
    - `controller` — Rewards will be sent to the controller account.
    - `account` — Rewards will be sent to any account you specify as transferrable balance.

  - `rewardDestination` — rewards destination account address.

  - `amount` — amount of tokens to bond. In usual DOT for the main network, KSM for Kusama, and WND for Westend.

   Example response:

   ```curl
   {
     "result": {
       "unsignedTransaction": "0xac0406000b00487835a302032c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f",
       "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
       "rewardDestinationType": "account",
       "rewardDestination": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
       "amount": 3,
       "createdAt": "2023-08-15T15:07:54.795Z"
     }
   }
   ```

  - `unsignedTransaction` — unsigned transaction in hex format. Sign the transaction and submit it to the blockchain to perform the called action.

  - `stashAccountAddress` — main stash account address which keeps tokens for bonding.

  - `rewardDestinationType` — rewards destination type:

    - `staked` — Pay into the stash account, increasing the amount at stake accordingly.
    - `stash` — Pay into the stash account, not increasing the amount at stake.
    - `controller` — Pay into the controller account.
    - `account` — Pay into a custom account.

  - `rewardDestination` — rewards destination account address.

  - `amount` — the amount of tokens to bond, in usual DOT for the main network, KSM for Kusama, and WND for Westend.

  - `createdAt` — timestamp of the transaction in the ISO 8601 format.

2. [Sign and broadcast](doc:signing-transaction-polkadot) `unsignedTransaction` to the Polkadot network.

## 2. Submit Nomination

1. Send a POST request to [/api/v1/polkadot/{network}/staking/nominate](ref:polkadot-staking-nominate).

   Example request (for `westend` network):

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/polkadot/westend/staking/nominate \
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
       "unsignedTransaction": "0x2102040605100096b33e0a9647f13198ad16a2812c549a363646a3a7ddbdcc5590f5839c408c6200767f36484b1e2acf5c265c7a64bfb46e95259c66a8189bbcd216195def43685200c21ad1e5198cc0dc3b0f9f43a50f292678f63235ea321e59385d7ee45a7208360018164fa6f9ce28792fb781185e8de4e6eaae34c0f545e5864952fe23c183df0c",
       "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
       "targets": [
         "5FUJHYEzKpVJfNbtXmR9HFqmcSEz6ak7ZUhBECz7GpsFkSYR",
         "5Ek5JCnrRsyUGYNRaEvkufG1i1EUxEE9cytuWBBjA9oNZVsf",
         "5GTD7ZeD823BjpmZBCSzBQp7cvHR1Gunq7oDkurZr9zUev2n",
         "5CcHdjf6sPcEkTmXFzF2CfH7MFrVHyY5PZtSm1eZsxgsj1KC"
       ],
       "createdAt": "2023-09-18T14:49:23.998Z"
     }
   }
   ```

  - `unsignedTransaction` — unsigned transaction in hex format. Sign the transaction and submit it to the blockchain to perform the called action.
  - `stashAccountAddress` — main stash account address which keeps tokens for bonding.
  - `targets` — selected validators in the targets.
  - `createdAt` — timestamp of the transaction in the ISO 8601 format.

2. [Sign and broadcast](doc:signing-transaction-polkadot) `unsignedTransaction` to the Polkadot network.

## What's Next?

- [Staking API](ref:polkadot) reference.
- [Withdrawal](doc:withdrawal-polkadot).