# Withdrawal

To initiate the withdrawal on the Ethereum network using the Staking API:

1. Create a serialized transaction to initiate the withdrawal process for active validators and send it as a POST request to [/api/v1/eth/staking/direct/tx/withdrawal](). Use https://test-api.p2p.org for testing or https://api.p2p.org for production.

   Example request:

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/eth/staking/direct/tx/withdrawal \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
   {
     "pubkeys": [
       "0xffC08FcD7cFeF5c70fB2b0e1f2A8EaA690AaE2bDFfa5dBEc4dEef31DcC0B19eB1f9Cebe3E2fe9eefBD9a1BDF6FD89b39"
     ]
   }
   '
   ```

    - `pubkeys` — list of validators public keys.

   Example response:

   ```json
   {
      "result": {
         "serializeTx": "0x02f902d705808301674e8508530af16e830186a094681a1b3441c6bfb12f91651efd9f02c83c0702938901bc16d674ec800000b902a44f498c730000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000012000000000000000000000000000000000000000000000000000000000000001a00000000000000000000000000000000000000000000000000000000000000260000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000030aa5f27070a21d79455c4a9b73c0aa4a8b1a65a1fb530d7fd8e6cd23aa16660679ac43ee4861098f6d9166aed3a4d8abb0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000002001000000000000000000000028c84612d37de9209018ad96167f12169b653e9a000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000060978c565cd915f4e885b4201093d1501697610eb9ee99b9b60b70434dc330e98d5b42927725304ded48483a8b8f39506d09bcb22ee18d4f6b50257946ac5ee360385308d95c0e2bc963902d42e985c29ee489aa3c989ac1561c952a6424f107a800000000000000000000000000000000000000000000000000000000000000014cb452f6e3f10ba2175c86a0284f53fcb61404b458393391abc3d5622e3e55cdc0",
         "to": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
         "gasLimit": "",
         "data": "",
         "value": "",
         "chainId": "",
         "type": "",
         "maxFeePerGas": "",
         "maxPriorityFeePerGas" : ""
      }      
   }
   ```

    - `serializeTx` — serialized unsigned transaction.
    - `to` — recipient address for this transaction.
    - `gasLimit` — maximum gas limit for this block.
    - `data` — ...
    - `value` — amount this transaction is sending in Wei.
    - `chainId` — chain ID this transaction is authorized on, as specified by [EIP-155](https://eips.ethereum.org/EIPS/eip-155).
    - `type` — [EIP-2718](https://eips.ethereum.org/EIPS/eip-2718) type of this transaction envelope.
    - `maxFeePerGas` — maximum price per unit of gas this transaction will pay for the combined [EIP-1559](https://eips.ethereum.org/EIPS/eip-1559) block's base fee and this transaction's priority fee in Wei.
    - `maxPriorityFeePerGas` — price per unit of gas in Wei, which is added to the [EIP-1559](https://eips.ethereum.org/EIPS/eip-1559) block's base fee. This added fee is used to incentivize miners to prioritize this transaction.

2. Sign and broadcast transaction:

   ```javascript
   ...
   ```

## What's Next?

- [Staking API](ref:ethereum) reference.
- [Staking]().
- [Check Status]().
