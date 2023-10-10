# Withdrawal

The withdrawal process in the Solana network using the Staking API can be done in several ways:

1. Deactivate Stake Transaction.
2. Withdrawal.

After all operations, you need to [sign and send transaction]() to the Solana network.

It normally takes up to three days to unstake your Solana. This is because Solana uses a system of epochs, each of which lasts around 2.5 days. In order to ensure fairness and network security, when you deactivate your stake, it will remain active for the remainder of the current epoch.

## 1. Deactivate Stake Transaction

1. Send a POST request to [/api/v1/solana/{network}/staking/deactivate](ref:).

   Example request (for `testnet` network):

   ```curl
   curl --request POST \
        --url https://api-test.p2p.org/api/v1/solana/testnet/staking/deactivate \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
        {
          "feePayer": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
          "stakeAccount": "6ZuLUCwVTvuQJrN1HrpoHJheQUw9Zk8CtiD3CEpHiA9E",
          "stakeAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC"
        }'
   ```

   - `feePayer` — account address that will pay the fee for the transaction.
   - `stakeAccount` — account address that stores tokens for staking.
   - `stakeAuthority` — account address that can perform staking operations with staking account.

   Example response:

   ```json
   {
     "result": {
       "feePayer": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
       "stakeAccount": "6ZuLUCwVTvuQJrN1HrpoHJheQUw9Zk8CtiD3CEpHiA9E",
       "stakeAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
       "unsignedTransaction": "0xac0406000b00487835a302032c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f",
       "createdAt": "2023-08-15T15:07:54.795Z"
     }
   }
   ```

   - `feePayer` — account address that will pay the fee for the transaction.
   - `stakeAccount` — account address that stores tokens for staking.
   - `stakeAuthority` — account address that can perform staking operations with staking account.
   - `unsignedTransaction` — unsigned transaction in Base64 encrypted format. Sign the transaction and submit it to the blockchain to perform the called action.
   - `createdAt` — timestamp of the transaction in the ISO 8601 format.

   > The transaction exists for one minute.

2. Use `unsignedTransaction` to [sign and send](doc:signing-transaction-polkadot) signed transaction to the Solana network.

## 2. Withdrawal

1. Send a POST request to [/api/v1/solana/{network}/staking/withdraw](ref:).

   Example request (for `testnet` network):

   ```curl
   curl --request POST \
        --url https://api-test.p2p.org/api/v1/solana/testnet/staking/withdraw \
        --header 'accept: application/json' \
        --header 'authorization: Bearer <token>' \
        --header 'content-type: application/json' \
        --data '
        {
          "feePayer": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
          "stakeAccount": "6ZuLUCwVTvuQJrN1HrpoHJheQUw9Zk8CtiD3CEpHiA9E",
          "withdrawAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
          "recipient": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
          "amount": 1002282880
        }'
   ```

   - `feePayer` — account address that will pay the fee for the transaction.
   - `stakeAccount` — account address that stores tokens for staking.
   - `withdrawAuthority` — account address that can perform withdrawal operations with staking account.
   - `recipient` — account address to which tokens will be sent.
   - `amount` — amount of tokens to withdrawal in lamports (1 SOl = 10^9 lamports).

   Example response:

   ```json
   {
     "result": {
       "feePayer": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
       "stakeAccount": "6ZuLUCwVTvuQJrN1HrpoHJheQUw9Zk8CtiD3CEpHiA9E",
       "stakeAuthority": "C83GxcNFTC2tK22rLCCrLKYRkckbNVGsjethN5iswgfC",
       "unsignedTransaction": "0xac0406000b00487835a302032c6eca5cdaa3e87d7f8e06d10015bf0508b52d301c8991af113d5cf49a53553f",
       "createdAt": "2023-08-15T15:07:54.795Z"
     }
   }
   ```

   - `feePayer` — account address that will pay the fee for the transaction.
   - `stakeAccount` — account address that stores tokens for staking.
   - `stakeAuthority` — account address that can perform staking operations with staking account.
   - `unsignedTransaction` — unsigned transaction in Base64 encrypted format. Sign the transaction and submit it to the blockchain to perform the called action.
   - `createdAt` — timestamp of the transaction in the ISO 8601 format.

   > The transaction exists for one minute.

2. Use `unsignedTransaction` to [sign and send](doc:signing-transaction-polkadot) signed transaction to the Solana network.

## What's Next?

- [Staking API](ref:solana) reference.