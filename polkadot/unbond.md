# Withdrawal

The withdrawal process for Polkadot in Staking API consists of several steps:

1. Unbond tokens:

   1. Submit Unbond: unbonding tokens within the Polkadot network refers to the process of withdrawing or releasing tokens that were previously staked or bonded.
   2. Sign transaction: sign transaction before broadcasting.
   3. Send transaction: broadcast transaction to the Polkadot network.

   > It takes 7 days (28 eras) to unbond on Kusama, 28 days (28 eras) on Polkadot, and 12 hours (2 eras) on Westend.

2. Withdrawal:

   1. Withdrawal Unbonded: withdrawing tokens within the Polkadot network that were previously unbonded.
   2. Sign transaction: sign transaction before broadcasting.
   3. Send transaction: broadcast transaction to the Polkadot network.

## 1. Unbond

### 1.1. Submit Unbond

Send a POST request to [/api/v1/polkadot/{network}/staking/unbond](). Use https://api-test.p2p.org for testing or https://api.p2p.org for production.

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

### 1.2. Sign transaction

Sign transaction using one of the following methods:

- https://github.com/Kron0S/sign-polkadot
- https://github.com/dmitrytarassov/polkadot_sign_demo

As a result, you will get a signed transaction in Base64 encrypted format.

### 1.3. Send transaction

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

## 2. Withdrawal

### 2.1. Withdrawal Unbonded

Send a POST request to [/api/v1/polkadot/{network}/staking/withdrawal-unbonded]().

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

### 2.2. Sign transaction

Sign transaction using one of the following methods:

- https://github.com/Kron0S/sign-polkadot
- https://github.com/dmitrytarassov/polkadot_sign_demo

As a result, you will get a signed transaction in Base64 encrypted format.

### 2.3. Send transaction

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

## What's Next?

- [Staking Bond]().
- [Withdrawal]().
- [Proxy Accounts]().
- [Staking API](ref:ethereum) reference.