# Getting Started

The staking process for Polkadot in Staking API consists of several steps:

1. Submit Bond: submitting a bond to the Polkadot network in exchange for some other benefit.
2. Sign transaction: sign transaction before broadcasting.
3. Send transaction: broadcast transaction to the Polkadot network.
4. Submit Nomination: the action of choosing validators within the Polkadot network.

[Get an authentication token](doc:authentication) to start using Staking API.

For Polkadot available several networks:

- `mainnet` — the main network.
- `kusama` — a canary network that holds real economic value.
- `westend` — a testnet.

A request examples is provided using [cURL](https://curl.se/).

## 1. Submit Bond

Send a POST request to [/api/v1/polkadot/{network}/staking/bond](). Use https://test-api.p2p.org for testing or https://api.p2p.org for production.

Example request (for `westend`):

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

   - `staked` — Pay into the stash account, increasing the amount at stake accordingly.
   - `stash` — Pay into the stash account, not increasing the amount at stake.
   - `controller` — Pay into the controller account.
   - `account` — Pay into a custom account.

- `rewardDestination` — rewards destination account address.
- `amount` — amount of tokens to bond. In usual DOT for main network, KSM for Kusama, and WND for Westend.

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
- `amount` — amount of tokens to bond. In usual DOT for main network, KSM for Kusama, and WND for Westend.
- `createdAt` — timestamp of the transaction in the ISO 8601 format.

## 2. Sign transaction

Sign transaction using one of the following methods:

- https://github.com/Kron0S/sign-polkadot
- https://github.com/dmitrytarassov/polkadot_sign_demo

As a result, you will get a signed transaction in Base64 encrypted format.

## 3. Send transaction

Send a POST request to [/api/v1/polkadot/{network}/tx/send]().

Example request (for `westend`):

```curl
curl --request POST \
     --url https://api.p2p.org/api/v1/polkadot/westend/tx/send \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <token>' \
     --header 'content-type: application/json' \
     --data '
{
  "signedTransaction": "0x450284002c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f01befdb7fa39c5a995a8d58676a0513d082be"
}'
```

- `signedTransaction` — signed transaction in Base64 encrypted format.

Example response:

```curl
{
  "result": {
    "status": "success",
    "blockHash": "0x0628743b05ffb4c9d5ea2144b359af38910f0ae439a685f57d85b50b9481ba3f",
    "blockId": "17168395",
    "extrinsicId": "0xb838911d5a5f965f33b8ee134e1115b5b9902abfc567f0c3050073faf9d3c3e0",
    "transactionHash": "0x450284002c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f01befdb7fa39c5a995a8d58676a0513d082be",
    "signerAccount": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
    "createdAt": "2023-08-15T15:07:54.795Z"
  }
}
```

- `staus` — transaction status.
- `blockHash` — block hash in which the transaction was included.
- `blockId` — unique block identifier.
- `extrinsicId` — unique extrinsic identifier.
- `transactionHash` — signed transaction in hex format.
- `signerAccount` — account that signed the transaction.
- `createdAt` — timestamp of the transaction in the ISO 8601 format.

## 4. Submit Nomination

Send a POST request to [/api/v1/polkadot/{network}/staking/nominate]().

Example request (for `westend`):

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

## What's Next?

- [Unbond]().
- [Withdrawal]().
- [Proxy Accounts]().
- [Staking API](ref:ethereum) reference.