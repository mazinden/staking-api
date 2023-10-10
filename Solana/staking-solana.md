# Getting Started

Staking in the Solana network using the Staking API consists of several main steps:

1. Create stake transaction.
2. Sign and send transaction to the network.

[Get an authentication token](doc:authentication) to start using Staking API.

A request example is provided using [cURL](https://curl.se/).

## 1. Create Stake Transaction

Send a POST request to [/api/v1/solana/{network}/staking/stake](ref:).

Example request (for `testnet` network):

```curl
curl --request POST \
     --url https://api-test.p2p.org/api/v1/solana/testnet/staking/stake \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <token>' \
     --header 'content-type: application/json' \
     --data '
     {
       "feePayer": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
       "fromPublicKey": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
       "amount": "1002282880",
       "stakeAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
       "withdrawAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC"
     }'
```

- `feePayer` — account address that will pay the fee for the transaction.
- `fromPublicKey` — account address from which the staking account will be created.
- `amount` — amount of tokens to stake in lamports (1 SOl = 10^9 lamports). Min amount is `1002282880`.
- `stakeAuthority` — account address that can perform staking operations with staking account. If not specified, rights will be taken from the `fromPublicKey` parameter.
- `withdrawAuthority` — account address that can perform withdrawal operation with staking account. If not specified, rights will be taken from the `fromPublicKey` parameter.

Example response:

```json
{
  "result": {
    "feePayer": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
    "fromPublicKey": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
    "stakeAccount": "6ZuLUCwVTvuQJrN1HrpoHJheQUw9Zk8CtiD3CEpHiA9E",
    "stakeAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
    "withdrawAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
    "amount": "1002282880",
    "unsignedTransaction": "0xac0406000b00487835a302032c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f",
    "createdAt": "2023-08-15T15:07:54.795Z"
  }
}
```

- `feePayer` — account address that will pay the fee for the transaction.
- `fromPublicKey` — account address from which the staking account will be created.
- `stakeAccount` — account address that stores tokens for staking.
- `stakeAuthority` — account address that can perform staking operations with staking account.
- `withdrawAuthority` — account address that can perform withdrawal operation with staking account.
- `amount` — amount of tokens to stake in lamports (1 SOl = 10^9 lamports). Min amount is `1002282880`.
- `unsignedTransaction` — unsigned transaction in Base64 encrypted format. Sign the transaction and submit it to the blockchain to perform the called action.
- `createdAt` — timestamp of the transaction in the ISO 8601 format.

> The transaction exists for one minute.

## 2. Sign and Send Transaction

Use `unsignedTransaction` to [sign and send](doc:signing-transaction-polkadot) signed transaction to the Solana network.

To check transaction status, send GET request to [/api/v1/solana/{network}/account/staking](ref:).

Example request (for `testnet` network):

```curl
curl --request GET \
     --url https://api-test.p2p.org/api/v1/solana/testnet/account/staking?stakeAuthorities=C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC&withdrawAuthorities=C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <token>' \
     --header 'content-type: application/json'
```

- `stakeAuthorities` — list of account addresses that can perform staking operations with staking accounts.
- `withdrawAuthorities` — list of account addresses that can perform withdrawal operation with staking accounts.

Example response:

```json
{
  "result": {
    "accounts": [
      "amount": 2282881,
      "stakeAccount": "1298uXQBW2azVA33UonLu2RoouDWkNqsV1rMUrQAy1SN",
      "withdrawAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
      "stakeAuthority": "AaMC5y1RofbY3unyrahRzyCzsesp7rfGuaGegbAg1Hxb",
      "voteAccount": "FKsC411dik9ktS6xPADxs4Fk2SCENvAiuccQHLAPndvk",
      "status": "active"
    ]
  }
}
```

- `accounts` — list of stake accounts.

   - `amount` — amount of tokens to stake in lamports (1 SOl = 10^9 lamports).
   - `stakeAccount` — account address that stores tokens for staking.
   - `stakeAuthority` — account address that can perform staking operations with staking account.
   - `withdrawAuthority` — account address that can perform withdrawal operation with staking account.
   - `voteAccount` — vote account address.
   - `status` — stake account status: `active`, `inactive`, `activating`, `deactivating`.

## What's Next?

- [Sign and Send Transaction](doc:).
- [Withdrawal](doc:withdrawal-solana).
- [Staking API](ref:solana) reference.