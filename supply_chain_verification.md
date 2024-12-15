
### **Schema Proposal: Sustainable Supply Chain Verification (Using OpenSC API)**

#### **Data Source Name**

OpenSC is an open-source platform for verifying the sustainability of supply chains using blockchain technology. The OpenSC API provides real-time traceability and verification of product sourcing, production processes, and environmental impact, ensuring that goods are sourced ethically and sustainably.

#### **API Endpoint URL**

`GET https://api.opensc.org/v1/products/{product_id}/verification`

#### **Purpose**

This schema validates the sustainability and ethical sourcing of a product by verifying its journey through the supply chain. The schema uses the OpenSC API to check if the product complies with sustainability standards, such as ethical labor practices, carbon footprint reduction, and sustainable sourcing of raw materials.

#### **Sample JSON Response**

```json


`{
  "product_id": "abc123xyz456",
  "status": "verified",
  "sustainability_score": 88,
  "supply_chain": [
    {
      "step": "Raw Material Sourcing",
      "location": "Brazil",
      "verified": true,
      "certifications": ["FairTrade", "Rainforest Alliance"]
    },
    {
      "step": "Manufacturing",
      "location": "Germany",
      "verified": true,
      "certifications": ["ISO 14001", "Carbon Neutral"]
    },
    {
      "step": "Shipping",
      "location": "USA",
      "verified": false,
      "certifications": []
    }
  ],
  "carbon_footprint": "10.5 kg CO2 equivalent",
  "expiration_date": "2025-12-31",
  "proof": {
    "type": "Blockchain Proof",
    "created": "2024-12-15T10:00:00Z",
    "verificationMethod": "did:opensc:12345abcde67890",
    "proofPurpose": "assertionMethod",
    "jws": "eyJhbGciOiJFUzI1NiJ9..."
  }
}` 
```

#### **Technical Breakdown**

This schema verifies that a product has been sourced and produced through sustainable and ethical means by tracking its journey across the supply chain. The verification process ensures compliance with sustainability standards and provides transparent traceability to consumers and businesses alike.

**Key Validation Conditions:**

-   **`status`**: Must be `verified`, confirming that the product's sustainability claims are validated.
-   **Sustainability Score**: The product must have a sustainability score above a defined threshold (e.g., 70).
-   **Supply Chain Steps**: Each step in the supply chain (raw material sourcing, manufacturing, shipping) must be verified, with certifications like FairTrade, ISO 14001, etc.
-   **Carbon Footprint**: The carbon footprint of the product should be within an acceptable range, which is verified through the API.
-   **Expiration Date**: The product’s certification validity must be checked to ensure that it is still within its expiration date.
-   **Proof**: The response must include cryptographic proof (e.g., JWS) of the product’s journey, ensuring the authenticity of the data provided.

**Key Use Cases:**

-   Verifying that products meet sustainability standards for eco-conscious consumers and businesses.
-   Providing transparency in supply chains for ethical sourcing of materials in industries such as fashion, food, and electronics.
-   Ensuring compliance with environmental regulations and corporate social responsibility (CSR) goals.
-   Auditing and reporting for sustainability certifications in corporate and government supply chain management.

----------

### **Schema Code**

```json
`{
  "issuer": "OpenSC",
  "desc": "Verifies the sustainability and ethical sourcing of a product through its supply chain.",
  "website": "https://opensc.org",
  "APIs": [
    {
      "host": "opensc.org",
      "intercept": {
        "url": "/v1/products/{product_id}/verification",
        "method": "GET"
      },
      "assert": [
        {
          "key": "status",
          "value": "verified",
          "operation": "="
        },
        {
          "key": "sustainability_score",
          "value": "70",
          "operation": ">="
        },
        {
          "key": "supply_chain[0].verified",
          "value": "true",
          "operation": "="
        },
        {
          "key": "supply_chain[1].verified",
          "value": "true",
          "operation": "="
        },
        {
          "key": "supply_chain[2].verified",
          "value": "true",
          "operation": "="
        },
        {
          "key": "carbon_footprint",
          "value": "10.5",
          "operation": "<="
        },
        {
          "key": "expiration_date",
          "value": "2025-12-31",
          "operation": ">"
        },
        {
          "key": "proof.jws",
          "operation": "exists"
        }
      ],
      "nullifier": "product_id"
    }
  ],
  "HRCondition": [
    "The product must be verified at all steps of the supply chain.",
    "The product's sustainability score must meet the minimum threshold (70 or higher).",
    "Carbon footprint must be within acceptable limits."
  ],
  "tips": {
    "message": "Ensure that all steps in the supply chain are verified and that the product's sustainability score exceeds the minimum requirement."
  },
  "category": "Sustainability",
  "id": "0xopenscSustainabilityValidation01"
}` 
```
----------

### **Demo/Test Case**

#### **Scenario**: A retailer is verifying the sustainability claims of a product before selling it on their platform.

-   **Step 1**: The retailer queries the OpenSC API using the `GET /v1/products/{product_id}/verification` endpoint, providing the product’s unique `product_id`.
-   **Step 2**: The API returns a response confirming the product's sustainability status, including its supply chain verification, sustainability score, carbon footprint, and expiration date.
-   **Step 3**: The retailer checks the product’s sustainability score to ensure it meets the threshold (e.g., 70 or higher) and verifies that each supply chain step is certified.
-   **Step 4**: If the product meets the sustainability criteria, it is listed on the platform; otherwise, the product is rejected or flagged for further review.
-   **Step 5**: The retailer uses the proof (JWS) to ensure that the data cannot be tampered with, ensuring the authenticity of the sustainability claims.

----------

### **Real-world Use Case**

This schema can be integrated into e-commerce platforms, ethical sourcing platforms, or businesses with sustainability goals. For example, a retailer like Patagonia could use this schema to verify that the products they sell are sourced ethically and meet sustainability standards. This approach ensures that both businesses and consumers can trust the provenance of the products, providing transparency and accountability across supply chains.

Additionally, this system is valuable for companies that need to ensure compliance with environmental regulations or for certifications such as ISO 14001, FairTrade, or carbon-neutral certifications.

By using blockchain technology for supply chain traceability, this schema ensures that the verification process is secure, transparent, and immutable, providing reliable proof of ethical sourcing and sustainability.

----------

This schema offers a robust method for ensuring transparency and authenticity in supply chains, focusing on sustainability and ethical sourcing. Through real-time verification of each step in the supply chain, it provides an important tool for businesses looking to meet corporate social responsibility goals and for consumers who want to make informed, eco-conscious purchasing decisions.