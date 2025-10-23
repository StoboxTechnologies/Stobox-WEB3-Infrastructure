# Stobox-WEB3-Infrastructure

<img width="1696" height="1192" alt="image" src="https://github.com/user-attachments/assets/f87dcfee-147e-43e3-89d5-e5e4c050a81a" />


# Infrastructure description

Stobox has a modular smart contract infrastructure for the Stobox RWA (Real World Assets) protocol, with clear separation of responsibilities and interactions between contracts. The system facilitates the creation, management, validation, and registration of tokenized assets and related modules:

---

## 1. [StoboxRWA Vault Factory SC](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#code) (entry point for asset vault creation)

- **Role:** Main factory contract responsible for deploying and registering new vault instances.
  - Acts as the entry point for creating new components of the protocol.
  - Responsible for deploying and initializing other smart contracts.
- **Creates:**
  - **STV3Treasury SC** – manages protocol treasury operations for the vault.
  - **StoboxProtocolSTV3 SC** – core protocol logic for a specific vault instance.
- **Manages:** Maintains administrative control over deployed components.
- **Integration:** Registers & manages vaults inside the StoboxRWAOfferingRegistry SC for comprehensive offering management.

---

## 2. [STV3Treasury SC](https://arbiscan.io/address/0x17b94f6b0ddeaac3753b33ee006a7fdbc1292dcf#code) (financial management)

- **Role:** Handles treasury functions
  - Stores and manages funds or tokenized assets related to the RWA protocol.
  - Ensures secure handling of protocol-related assets.
- **Created by:** StoboxRWAVaultFactory SC.
- **Connections:** Works together with StoboxProtocolSTV3 SC for vault-related operations.

---

## 3. [StoboxProtocolSTV3 SC](https://arbiscan.io/address/0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46#code) (core protocol logic)

- **Role:** Implements the business logic for RWA token operations, compliance, and transactions.
- **Created by:** StoboxRWAVaultFactory SC.
- **Managed:** Directly by the factory contract.
- **Works with:** Validation HasDIDRule SC for identity verification.

---

## 4. [Validation HasDIDRule SC](https://arbiscan.io/address/0x85a3eae3cd8cecac03d2fc44001e030621320acb#code) (identity-based rule check)

- **Role:** A validation module
  - Implements a specific compliance rule: Has DID (Decentralized Identifier).
  - Ensures that only entities with a valid DID can perform certain actions within the protocol.
- **Interaction:**
  - Invoked by **StoboxProtocolSTV3 SC** during operations that require DID verification.
  - Performs **CHECK** operations against the **StoboxDID SC**.

---

## 5. [StoboxDID SC](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#code) (decentralized identity registry)

- **Role:** Stores and manages DIDs (Decentralized Identifiers) for participants.
  - Manages DID creation, attributes, linked addresses, and access permissions.
  - Provides the source of truth for identity verification across the ecosystem.
- **Interaction:** Queried by Validation HasDIDRule SC to confirm DID ownership and validity.

---

## 6. [StoboxRWAOfferingRegistry SC](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code) (offering management & compliance registry)

- **Role:** Manages registration, lifecycle, and compliance validation for Security Token Offerings (STOs) tied to Real World Assets (RWAs).
  - Handles complete lifecycle: creation, activation, suspension, cancellation, termination of offerings.
  - Enforces compliance via a flexible rule engine and role-based access controls.
- **Architecture:** Built using EIP-2535 Diamond Pattern for upgradability and modular facets:
  - **DiamondCutFacet** – Handles upgrades and facet management.
  - **DiamondLoupeFacet** – Provides introspection and facet/query logic.
  - **RegistryRoleFacet** – Manages user roles and permissions (admin, tech service, offering manager, etc).
  - **OfferingGovernanceFacet** – Offering lifecycle management and governance.
  - **RegistryStorageFacet** – Reliable data storage and retrieval for offerings.
  - **RegistryRuleEngineFacet** – Enforces and manages offerings' compliance rules.
- **Interaction:**
  - Integrates with **StoboxProtocolSTV3 SC** for security token management.
  - Pulls identity data from **StoboxDID SC** via validation rules (e.g., AccreditedUSMinInvestmentRule).
  - Enables offering registration and compliance enforcement through custom rules and whitelisting.
  - Can lock and unlock security tokens based on offering state.
- **Deployed at:**
  - **Mainnet:** [`0xb05cf4f652eb5475c3e4246ab976785678a1806a`](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code) ([v1.0.1 release notes](./ST4RWAOfferingRegistry-Release-Notes.md))
- **Status:** Implemented; see [release notes](./ST4RWAOfferingRegistry-Release-Notes.md) and [contract source](https://github.com/StoboxTechnologies/ST4RWAOfferingRegistry).

---

## Flow Summary

1. **StoboxRWAVaultFactory SC** creates both the **STV3Treasury SC** and **StoboxProtocolSTV3 SC**.
2. **StoboxRWAVaultFactory SC** manages the deployed protocol contracts.
3. **StoboxProtocolSTV3 SC** interacts with **Validation HasDIDRule SC** to check compliance.
4. **Validation HasDIDRule SC** queries **StoboxDID SC** to verify decentralized identities.
5. **StoboxRWAOfferingRegistry SC** manages STO lifecycle and compliance validation, integrating with the full RWA ecosystem.

---

**Integration Highlights:**
- The **StoboxRWAOfferingRegistry SC** is designed to coordinate with the full RWA ecosystem:
  - When a new offering is created, it can assign roles, configure compliance rules, require investors to meet identity/KYC requirements (via rules engine and DID checks), and manage token reservation.
  - The registry acts as a unified point of compliance, history, and role enforcement for all RWA offerings on Stobox.




