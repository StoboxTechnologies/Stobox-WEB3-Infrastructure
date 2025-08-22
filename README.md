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
- **Future Integration:** Will also register & manage vaults inside the RWAOfferingRegistry SC (currently not yet active).

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

## 6. RWAOfferingRegistry SC (not yet implemented, planned module)

> **Note:** This module is planned for future development and is **not yet implemented**.

- **Role:** Will be a registry of offerings for RWAs.
  - Intended to register and manage specific RWA offerings.
  - Will integrate with StoboxRWAVaultFactory SC for asset lifecycle tracking.
- **Interaction:** Will be integrated with the StoboxRWAVault Factory SC for asset lifecycle tracking and management.
- **Status:** Not implemented.

---

## Flow Summary

1. **StoboxRWAVaultFactory SC** creates both the **STV3Treasury SC** and **StoboxProtocolSTV3 SC**.
2. **StoboxRWAVaultFactory SC** manages the deployed protocol contracts.
3. **StoboxProtocolSTV3 SC** interacts with **Validation HasDIDRule SC** to check compliance.
4. **Validation HasDIDRule SC** queries **StoboxDID SC** to verify decentralized identities.
5. *(Future step):* The factory may also register offerings in **RWAOfferingRegistry SC**.
