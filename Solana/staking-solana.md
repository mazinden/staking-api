# Getting Started

Staking in the Solana network using the Staking API consists of several main steps:

1. Create stake transaction.
2. Sign unsigned transaction.
3. Send signed transaction.

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
- `fromPublicKey` — 
- `amount` — the amount of tokens to stake in lamports (1 SOl = 10^9 lamports).
- `stakeAuthority` —
- `withdrawAuthority` —

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
- `fromPublicKey` —
- `stakeAccount` — main stash account address which keeps tokens for bonding.
- `stakeAuthority` —
- `withdrawAuthority` —
- `amount` — in lamports (1 SOl = 10^9 lamports)
- `unsignedTransaction` — unsigned transaction in hex format. Sign the transaction and submit it to the blockchain to perform the called action.
- `createdAt` — timestamp of the transaction in the ISO 8601 format.

## 2. Sign Unsigned Transaction


## 3. Send Signed Transaction

Send a POST request to [/api/v1/solana/{network}/tx/send](ref:).

Example request (for `testnet` network):

```curl
curl --request POST \
     --url https://api-test.p2p.org/api/v1/solana/testnet/tx/send \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <token>' \
     --header 'content-type: application/json' \
     --data '
     {
       "signedTransaction": "AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAcJjkQt4XcX43Vk8FZ7QbUVXSF5oo9jt7x2Dm0E9ut/y+jagnMHpK8BDHt0PpssHwXGD2fBxS6MWBoxptD2u9TvrgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA1NjSeWM5+GSJdoQd43Al9SVVXC9FfWGwbe7icpomwAUGodgXkTdUKpg0N73+KnqyVX9TXIp4citopJ3AAAAAAAah2BelAgULaAeR5s5tuI4eW3FQ9h/GeQpOtNEAAAAABqfVFxjHdMkoVmOYaR1etoteuKObS21cc1VbIQAAAAAGp9UXGSxcUSGMyUw9SvF/WNruCJuh/UTj29mKAAAAAAan1RcZNYTQ/u2bs0MdEyBr5UQoG1e4VmzFN1/0AAAAijW940iwWddz25ZC37fI0ue5fa+eTbC2ynBM3b0t4pcDAgMAAQBgAwAAAI5ELeF3F+N1ZPBWe0G1FV0heaKPY7e8dg5tBPbrf8voBAAAAAAAAABzZWVkgJaYAAAAAADIAAAAAAAAAAah2BeRN1QqmDQ3vf4qerJVf1NcinhyK2ikncAAAAAABAIBB3QAAAAAjkQt4XcX43Vk8FZ7QbUVXSF5oo9jt7x2Dm0E9ut/y+iORC3hdxfjdWTwVntBtRVdIXmij2O3vHYObQT263/L6AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQGAQMGCAUABAIAAAA="
     }'
```

- `signedTransaction` — 


Example response:

```json
{
  "result": {
    "transactionId": "0x0628743b05ffb4c9d5ea2144b359af38910f0ae439a685f57d85b50b9481ba3f",
    "slot": 17168395,
    "signerAccounts": [
      "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC"
    ],
    "createdAt": "2023-08-15T15:07:54.795Z"
  }
}
```

- `transactionId` — 
- `slot` —
- `signerAccounts` — 
- `createdAt` — timestamp of the transaction in the ISO 8601 format.

To check transaction status, send GET request to [/api/v1/solana/{network}/account/staking](ref:).

Example request (for `testnet` network):

```curl
curl --request GET \
     --url https://api-test.p2p.org/api/v1/solana/testnet/account/staking?stakeAuthorities=C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC&withdrawAuthorities=C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <token>' \
     --header 'content-type: application/json'
```

- `stakeAuthority` —
- `withdrawAuthority` —

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

- `accounts` —

   - `amount` —
   - `stakeAccount` —
   - `withdrawAuthority`
   - `stakeAuthority`
   - `voteAccount`
   - `status`

## What's Next?

- [Staking API](ref:solana) reference.
- [Withdrawal](doc:withdrawal-solana).