
### **Schema Proposal: Cryptocurrency Transaction Validation (Using BlockCypher API)**

#### **Data Source Name**

BlockCypher, a provider of blockchain-based services, offers APIs to query blockchain data for various cryptocurrencies, including Bitcoin and Ethereum.

#### **API Endpoint URL**

`GET https://api.blockcypher.com/v1/btc/main/txs/{tx_hash}`

#### **Purpose**

This schema validates that a user’s cryptocurrency transaction (Bitcoin, Ethereum, etc.) has been successfully processed and confirmed on the blockchain by checking the transaction status.

#### **Sample JSON Response**

```json

`{
  "hash": "b6a1f7e1e4c264f80de278349ec5b914c6d825f8699a7c79f5645b3d1cfb41f9",
  "block_height": 684724,
  "confirmations": 9,
  "total": 0.0042,
  "fees": 0.0001,
  "inputs": [
    {
      "addresses": ["1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa"],
      "output_index": 0,
      "script_type": "pay-to-pubkey-hash",
      "value": 0.0043
    }
  ],
  "outputs": [
    {
      "addresses": ["1LQgF8V6Xvhh7HH9aDJ9gM7hsUwXZGEZUs9"],
      "value": 0.0042
    }
  ]
}` 
```

#### **Technical Breakdown**

This schema validates that a cryptocurrency transaction (Bitcoin or Ethereum) has been successfully confirmed and processed on the blockchain using the BlockCypher API.

**Key Validation Conditions:**

-   The `status` must indicate that the transaction is `confirmed`.
-   The `confirmations` field must exceed a minimum threshold (e.g., 6 confirmations).
-   The `inputs` and `outputs` addresses must match those provided by the user.
-   The `block_height` must indicate that the transaction is included in a valid block.
-   The `total` field must reflect the correct amount being transferred.

**Key Use Cases:**

-   Verifying cryptocurrency payments on e-commerce platforms.
-   Confirming blockchain-based transactions in financial, gaming, and NFT platforms.
-   Preventing fraud by ensuring the transaction has been fully validated on the blockchain.

----------

### **Schema Code**

```json
`{
  "issuer": "BlockCypher",
  "desc": "Verifies a cryptocurrency transaction has been confirmed on the blockchain",
  "website": "https://www.blockcypher.com",
  "APIs": [
    {
      "host": "blockcypher.com",
      "intercept": {
        "url": "/v1/btc/main/txs/{tx_hash}",
        "method": "GET"
      },
      "assert": [
        {
          "key": "confirmations",
          "value": 6,
          "operation": ">="
        },
        {
          "key": "status",
          "value": "confirmed",
          "operation": "="
        },
        {
          "key": "inputs[0].addresses",
          "value": "user_input_address",
          "operation": "="
        },
        {
          "key": "outputs[0].addresses",
          "value": "user_output_address",
          "operation": "="
        }
      ],
      "nullifier": "hash"
    }
  ],
  "HRCondition": [
    "The transaction must have 6 or more confirmations to be considered valid.",
    "The inputs and outputs addresses must match the user's provided addresses."
  ],
  "tips": {
    "message": "Ensure the transaction is confirmed by the blockchain before proceeding with any services. Check the number of confirmations to ensure the transaction's validity."
  },
  "category": "Blockchain",
  "id": "0xblockcypherTransactionValidation01"
}` 
```

----------

### **Demo/Test Case**

#### **Scenario**: A user submits a Bitcoin transaction for payment on an e-commerce platform.

-   **Step 1**: The user initiates the transaction and receives a transaction hash (`tx_hash`).
-   **Step 2**: The platform queries the BlockCypher API to validate the transaction by calling `GET /v1/btc/main/txs/{tx_hash}`.
-   **Step 3**: The API returns the transaction details, including the `confirmations`, `status`, and associated `addresses`.
-   **Step 4**: The system checks that the `status` is `confirmed` and that the `confirmations` count is greater than or equal to 6.
-   **Step 5**: If all conditions are met, the platform accepts the transaction and processes the payment.

----------

### **Real-world Use Case**

This schema can be integrated with various cryptocurrency payment gateways or e-commerce platforms that accept Bitcoin or other cryptocurrencies for transactions. By using this schema, the platform ensures that transactions are fully confirmed on the blockchain before processing, reducing the risk of fraud or double-spending.

This also applies to gaming platforms, NFT marketplaces, and any service that accepts cryptocurrency as a payment method, where validation of the transaction’s legitimacy is crucial before granting access or fulfilling orders.