# Getting Started Privat Node

The staking process for Polkadot in the Staking API with a privat node consists of several main steps:

1. Submit Bond: Submit a bond to the Polkadot network in exchange for certain benefits.
2. Sign Transaction: Sign the transaction before broadcasting it.
3. Send Transaction: Broadcast the transaction to the Polkadot network.
4. Add Proxy Account: Staking proxies allow users to delegate their staking rights to another account, which can then sign transactions on their behalf.

[Get an authentication token](doc:authentication) to start using Staking API.

A request examples is provided using [cURL](https://curl.se/).

## 1. Submit Bond

Send a POST request to [/api/v1/polkadot/{network}/staking/bond]().

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

To verify transaction status, send a GET request to [/api/v1/polkadot/{network}/tx/status/{blockHash}/{transactionHash}]().

Example request (for `westend` network):

```curl
curl --request GET \
     --url https://api.p2p.org/api/v1/polkadot/westend/tx/status/0x2b235d960a1880661ecff07f8d3f017eedfb94526ab00e96ce5833d812c23b2b/0x9cd8514d3e6935426fe3ac8496307a0d8b5fdda035e22723e43045b7fa0896c6 \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <token>' \
     --header 'content-type: application/json'
```

Example response:

```curl
{
  "result": {
    "status": "success",
    "blockHash": "0x2b235d960a1880661ecff07f8d3f017eedfb94526ab00e96ce5833d812c23b2b",
    "blockId": 17138875,
    "extrinsicId": 2,
    "transactionHash": "0x9cd8514d3e6935426fe3ac8496307a0d8b5fdda035e22723e43045b7fa0896c6",
    "signerAccount": "5GYwcZcx1i23AxtUEqTkUG7muLB9fYqoeHW5eMNVXhdNrZas",
    "createdAt": "2023-09-20T07:34:20.433Z"
  }
}
```

## 3. Send transaction

Send a POST request to [/api/v1/polkadot/{network}/tx/send]().

Example request (for `westend` network):

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

To verify transaction status, send a GET request to [/api/v1/polkadot/{network}/tx/status/{blockHash}/{transactionHash}]().

## 4. Add Proxy Account

Send a POST request to [/api/v1/polkadot/{network}/account/add]().

Example request (for `westend` network):

```curl
curl --request POST \
     --url https://api.p2p.org/api/v1/polkadot/westend/account/add \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <token>' \
     --header 'content-type: application/json' \
     --data '
{
  "stashAccountAddress": "5H6ryBWChC5w7eaQ4GZjo329sEnhvjetSr6MBEt42mZ5tPw5",
  "proxyAccountAddress": "5Ggpg3JepXM3ZrktNpoc5QA1sKaFVpUPWMRr7jppiMxTuU75"
}'
```

- `stashAccountAddress` — proxied account is an address that transfers rights to a proxy account. The original account retains all of its rights, and the proxy account can be removed at any time.
- `proxyAccountAddress` — proxy account is an address that receives rights from the proxied account.

Example response:

```curl
{
  "result": {
    "unsignedTransaction": "0xa404160100cc7cb7325ad1208212e2d8ee41a7572e816d53ac1bcac1be5df433486819213c0200000000",
    "createdAt": "2023-09-18T14:49:23.998Z"
  }
}
```

- `unsignedTransaction` — unsigned transaction in hex format. Sign the transaction and submit it to the blockchain to perform the called action.
- `createdAt` — timestamp of the transaction in the ISO 8601 format.

## What's Next?

- [Getting Started Public Node]().
- [Withdrawal]().
- [Staking API](ref:ethereum) reference.
