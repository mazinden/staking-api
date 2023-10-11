# Sign and Send Transaction

To sign and send a transaction to the Solana network, follow these steps:

1. Prepare unsigned transaction in Base64 encrypted format.
2. Sign the transaction using the following code:

   ```javascript
   const web3 = require('@solana/web3.js');
   const bs58 = require('bs58');
   const { Transaction } = web3

   (async () => {

     // base58 signer private keys devided by comma `,`
     const privateKeys = process.env.PRIVATE_KEYS;
     // base64 unsigned transaction
     const transaction = process.env.TX

     let tx = Transaction.from(Buffer.from(transaction, 'base64'));

     // sign transaction for each signer
     for (const pk of privateKeys) {
       const signer = web3.Keypair.fromSecretKey(new Uint8Array(bs58.decode(pk)));
       tx.sign(signer)
     }
     // print signed transaction
     console.log(tx.serialize().toString('base64'))
   })();
   ```

3. Send the signed transaction to the Solana network by making a POST request to [/api/v1/solana/{network}/tx/send]().

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

   - `signedTransaction` — signed transaction in Base64 encrypted format.

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

   - `transactionId` — block hash in which the transaction was included.
   - `slot` — period of time during which each leader collects transactions and creates a block in the Solana network.
   - `signerAccounts` — account addresses that signed the transaction.
   - `createdAt` — timestamp of the transaction in the ISO 8601 format.

## What's Next?

- [Getting Started](doc:)
- [Withdrawal](doc:withdrawal-solana).
- [Staking API](ref:solana) reference.