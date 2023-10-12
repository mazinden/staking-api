# Sign and Broadcast Transaction

To sign and broadcast a transaction on the Polkadot network, follow these steps:

1. Prepare unsigned transaction in hex format.

2. Sign the transaction using the following code:

   ```javascript
   import { readFileSync } from "fs";
   import { ApiPromise, WsProvider, Keyring } from "@polkadot/api";
   import '@polkadot/api-augment';

   async function sign () {
     // Polkadot websocket url
     const provider = process.env.PROVIDER;
     // sr25519 secret key filename
     const secretFileName = process.env.SECRET_FILE_NAME;
     // password of secret key
     const password = process.env.PASSWORD;
     // transaction in base64
     const rawTransaction = process.env.RAW_TRANSACTION;

     // connect to polkadot node
     const wsProvider = new WsProvider(provider);
     const api = await ApiPromise.create({ provider: wsProvider });
     await api.isReady;

     // load keyring file
     const keyring = new Keyring({ type: "sr25519" });
     const fileContent = readFileSync(
       secretFileName,
       "utf8"
     );
     const keyInfo = JSON.parse(fileContent);
     const sender = keyring.addFromJson(keyInfo);
     // decode secret key
     sender.decodePkcs8(password);

     const unsigned = api.tx(rawTransaction);

     // sing transaction
     const signedExtrinsic = await unsigned.signAsync(sender);
     console.log('signedExtrinsic', signedExtrinsic.toHuman());

     // print signed transaction
     const hexEx = signedExtrinsic.toHex()
     console.log('signedExtrinsic toHex', hexEx);

     await api.disconnect()
   }

   // run sing function
   sign().catch((error) => {
     console.error(error);
     process.exit(1);
   });
   ```

3. Broadcast transaction to the Polkadot network: send a POST request to [/api/v1/polkadot/{network}/tx/send](ref:polkadot-transaction-send).

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

## What's Next?

- [Getting Started Public Node](doc:staking-polkadot-public).
- [Withdrawal](doc:withdrawal-polkadot).
- [Staking API](ref:polkadot) reference.