# Check Status

Using the Staking API for the Ethereum network, you can check the following statuses:

- **Check Status Request**: The status of the node set-up operation.
- **Check Status Validator**: The status of the validators.

## Check Request Status

Send a GET request to [/api/v1/eth/staking/direct/nodes-request/status/{id}](). Use https://test-api.p2p.org for testing or https://api.p2p.org for production.

Example request:

   ```curl
   curl --request GET \
        --url https://api.p2p.org/api/v1/eth/staking/direct/nodes-request/status/3611b95c-e1b3-40c0-9086-3de0a4379943 \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>'
   ```

- `id` — UUID that was specified in the node set-up request.

Example response:

   ```json
   {
     "result": {
       "id": "3611b95c-e1b3-40c0-9086-3de0a4379943",
       "status": "ready",
       "validatorsCount": 1,
       "withdrawalAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
       "controllerAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
       "feeRecipientAddress": "0x53da3c92fCCEb0CFE1764f65DDfF1564A2b15585",
       "depositData": [
         {
           "pubkey": "0xac1e9969d7b87f3102549ab41558136674a7306b85b9f73cfbd7d9fdb7db85724569da3ebd4d7de9689f6ac058d7e2a3",
           "signature": "0xb656f9c771166c82a7891b930e6a920878d9736eb3f9f241753a15ea69d8e2f20a3740dfaf546c70e31bd323e14b341205d04e3227dd4cf2923644a375f6792875ac02c5f256f7a17c96b09bafcbce7e4443e1862356b1e90d78875d78e9a742",
           "depositDataRoot": "0xba013b4950b9aff0c3c19017ec5b6e0ed5b957b36f6ff03a545e5cc5605baff8"
         }
       ]
     }
   }
   ```

- `id` — UUID that was specified in the node set-up request.
- `status` — current status of the nodes request.
- `validatorsCount` — number of validators. One validator is equal to 32 ETH.
- `withdrawalAddress` — withdrawal address for the validators.
- `controllerAddress` — controller address for the validators.
- `feeRecipientAddress` — fee recipient address.
- `depositData`:

   - `pubkey` — validator public key.
   - `signature` — validator signature.
   - `depositDataRoot` — hash of the deposit data.

## Check Validator Status

Send a POST request to [/api/v1/eth/staking/direct/validator/status](). Use https://test-api.p2p.org for testing or https://api.p2p.org for production.

Example request:

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/eth/staking/direct/validator/status \
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
       "list": [
         {
           "status": "active_online",
           "amount": 1,
           "pubkey": "0xffC08FcD7cFeF5c70fB2b0e1f2A8EaA690AaE2bDFfa5dBEc4dEef31DcC0B19eB1f9Cebe3E2fe9eefBD9a1BDF6FD89b39",
           "withdrawalAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17"
         }
       ]
     }
   }
   ```

- `status` — state of the validator.
- `amount` — total stake amount for the validator at the moment.
- `pubkeys` — list of validators public keys.
- `withdrawalAddress` — withdrawal address for the validator.

## What's Next?

- [Staking API](ref:ethereum) reference.
- [Staking]().
- [Withdrawal]().
