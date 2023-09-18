# Staking

There are several ways to start staking on the Ethereum network using the Staking API:

- Utilize the P2P smart contract.
- Interact directly with the Ethereum Deposit Smart Contract.

[Get an authentication token](doc:authentication) to start using Staking API.

A request examples is provided using [cURL](https://curl.se/).

## P2P Smart Contract

1. Prepare`id` that is an arbitrary UUID. Generate it in one of the following ways:

   - [Online UUID Generator](https://www.uuidgenerator.net/)
   - [uuid npm package](https://www.npmjs.com/package/uuid)

2. Set up staking nodes through P2P infrastructure by sending a POST request to [/api/v1/eth/staking/direct/nodes-request/create](ref:eth-nodes-request-create). Use https://test-api.p2p.org for testing or https://api.p2p.org for production.

   Example request:

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/eth/staking/direct/nodes-request/create \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
   {
     "id": "3611b95c-e1b3-40c0-9086-3de0a4379943",
     "validatorsCount": 1,
     "withdrawalAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
     "controllerAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
     "feeRecipientAddress": "0x53da3c92fCCEb0CFE1764f65DDfF1564A2b15585",
     "nodesOptions": {
       "location": "any"
     }
   }
   '
   ```

   - `id` — arbitrary UUID. You can later use that UUID to check the status of the set-up operation.
   - `validatorsCount` — number of validators. One validator is equal to 32 ETH.
   - `withdrawalAddress` — withdrawal address for the validators.
   - `controllerAddress` — controller address for the validators.
   - `feeRecipientAddress` — fee recipient address.
   - `nodesOptions`:

      - `location` — node location. Currently, only `any` is supported.
      - `relaysSet` — miner extractable value (MEV) relay selection.

   Example response:

   ```json
   {
     "result": true
   }
   ```

3. Check the node set-up operation status by sending a GET request to [/api/v1/eth/staking/direct/nodes-request/status/{id}]().

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

4. Create a serialized transaction for depositing the stake amount and send it as a POST request to [/api/v1/eth/staking/direct/tx/deposit](ref:eth-staking-deposit).

   Example request:

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/eth/staking/direct/tx/deposit \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
   {
     "withdrawalAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
     "depositData": [
       {
         "pubkey": "0xac1e9969d7b87f3102549ab41558136674a7306b85b9f73cfbd7d9fdb7db85724569da3ebd4d7de9689f6ac058d7e2a3",
         "signature": "0xb656f9c771166c82a7891b930e6a920878d9736eb3f9f241753a15ea69d8e2f20a3740dfaf546c70e31bd323e14b341205d04e3227dd4cf2923644a375f6792875ac02c5f256f7a17c96b09bafcbce7e4443e1862356b1e90d78875d78e9a742",
         "depositDataRoot": "0xba013b4950b9aff0c3c19017ec5b6e0ed5b957b36f6ff03a545e5cc5605baff8"
       }
     ]
   }
   '
   ```

   - `withdrawalAddress` — withdrawal address for the validators.
   - `depositData`:

     - `pubkey` — validator public key.
     - `signature` — validator signature.
     - `depositDataRoot` — hash of the deposit data.

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

5. Sign and broadcast transaction:

   ```javascript
   require("dotenv").config();
   const ethers = require("ethers");

   async function signAndBroadcast() {
     console.log("Started");

     // Enter the serialized transaction
     const rawTransaction = process.env.RAW_TRANSACTION;

     // Enter the private key of the address used to transfer the stake amount
     const privateKey = process.env.PRIVATE_KEY;

     // Enter the selected RPC URL
     const rpcURL = process.env.RPC_URL;

     // Initialize the provider using the RPC URL
     const provider = new ethers.providers.JsonRpcProvider(rpcURL);

     // Initialize a new Wallet instance
     const wallet = new ethers.Wallet(privateKey, provider);

     // Parse the raw transaction
     const tx = ethers.utils.parseTransaction(rawTransaction);

     tx.nonce = await provider.getTransactionCount(wallet.address);

     // Enter the max fee per gas and prirorty fee
     tx.maxFeePerGas = ethers.utils.parseUnits(
       process.env.MAX_FEE_PER_GAS_IN_GWEI,
       "gwei"
     );
     tx.maxPriorityFeePerGas = ethers.utils.parseUnits(
       process.env.MAX_PRIORITY_FEE_IN_GWEI,
       "gwei"
     );

     // Sign the transaction
     const signedTransaction = await wallet.signTransaction(tx);

     // Send the signed transaction
     const transactionResponse = await provider.sendTransaction(signedTransaction);

     return transactionResponse;
   }

   signAndBroadcast()
     .then((transactionResponse) => {
       console.log(
         "Transaction broadcasted, transaction hash:",
         transactionResponse.hash
       );
     })
     .catch((error) => {
       console.error("Error:", error);
     })
     .finally(() => {
       console.log("Finished");
     });

   ```

To check the transaction status, use the [Check Status Request]() method.

## Ethereum Deposit Smart Contract

1. Prepare`id` that is an arbitrary UUID. Generate it in one of the following ways:

   - [Online UUID Generator](https://www.uuidgenerator.net/)
   - [uuid npm package](https://www.npmjs.com/package/uuid)

2. Set up staking nodes through P2P infrastructure by sending a POST request to [/api/v1/eth/staking/direct/nodes-request/create](ref:eth-nodes-request-create). Use https://test-api.p2p.org for testing or https://api.p2p.org for production.

   Example request:

   ```curl
   curl --request POST \
        --url https://api.p2p.org/api/v1/eth/staking/direct/nodes-request/create \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
   {
     "id": "3611b95c-e1b3-40c0-9086-3de0a4379943",
     "validatorsCount": 1,
     "withdrawalAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
     "controllerAddress": "0x39D02C253dA1d9F85ddbEB3B6Dc30bc1EcBbFA17",
     "feeRecipientAddress": "0x53da3c92fCCEb0CFE1764f65DDfF1564A2b15585",
     "nodesOptions": {
       "location": "any"
     }
   }
   '
   ```

   - `id` — arbitrary UUID. You can later use that UUID to check the status of the set-up operation.
   - `validatorsCount` — number of validators. One validator is equal to 32 ETH.
   - `withdrawalAddress` — withdrawal address for the validators.
   - `controllerAddress` — controller address for the validators.
   - `feeRecipientAddress` — fee recipient address.
   - `nodesOptions`:

      - `location` — node location. Currently, only `any` is supported.
      - `relaysSet` — Miner Extractable Value (MEV) Relay Selection.

   Example response:

   ```json
   {
     "result": true
   }
   ```

3. Send transaction to [Ethereum deposit smart contract](https://etherscan.io/address/0x00000000219ab540356cBB839Cbe05303d7705Fa).

To check the transaction status, use the [Check Status Request]() method.

## What's Next?

- [Staking API](ref:ethereum) reference.
- [Check Status]().
- [Withdrawal]().
