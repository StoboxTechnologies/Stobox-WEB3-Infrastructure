# Stobox-WEB3-Infrastructure

<img width="2048" height="1544" alt="image" src="https://github.com/user-attachments/assets/ed8dae7f-bcb7-4b12-af0e-20eea58129ad" />


# Infrastructure description

Stobox has a modular smart contract infrastructure for the Stobox RWA (Real World Assets) protocol, with clear separation of responsibilities and interactions between contracts. The system facilitates the creation, management, validation, and registration of tokenized assets and related modules:

---

## 1. [StoboxRWA Vault Factory SC](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#code) (entry point for asset vault creation)

- **Role:** Main factory contract responsible for deploying and registering new vault instances.
  - Acts as the entry point for creating new components of the protocol.
  - Responsible for deploying and initializing other smart contracts.
  - Acts as the deployer for StoboxProtocolSTV3 Diamond contracts.
- **Creates:**
  - **STV3Treasury SC** – manages protocol treasury operations for the vault.
  - **StoboxProtocolSTV3 SC** – core protocol logic for a specific vault instance (Diamond Standard implementation with facets).
- **Manages:** 
  - Maintains administrative control over deployed components.
  - Configures validation rules and trusted addresses for protocol instances.
- **Integration:** Registers & manages vaults inside the StoboxRWAOfferingRegistry SC for comprehensive offering management.
- **Release Notes:** [StoboxRWAVaultFactory-Release-Notes.md](./StoboxRWAVaultFactory-Release-Notes.md)

---

## 2. [STV3Treasury SC](https://arbiscan.io/address/0x17b94f6b0ddeaac3753b33ee006a7fdbc1292dcf#code) (financial management)

- **Role:** Handles treasury functions
  - Stores and manages funds or tokenized assets related to the RWA protocol.
  - Ensures secure handling of protocol-related assets.
- **Created by:** StoboxRWAVaultFactory SC.
- **Connections:** Works together with StoboxProtocolSTV3 SC for vault-related operations.

---

## 3. [StoboxProtocolSTV3 SC](https://arbiscan.io/address/0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46#code) (core protocol logic)

- **Role:** STV3 represents Stobox's latest proprietary tokenization standard, engineered to deliver a more secure, efficient, and scalable framework for managing tokenized assets.
- **Architecture:** Built using **Diamond Standard (EIP-2535)** for modularity and upgradability.
  - Enables modular design by separating functionality into different facets
  - Contract logic is encapsulated within a single contract using DELEGATECALL to invoke facet contracts
  - Supports dynamic upgrades without redeploying the entire system
- **Token Standard:** ERC-20 compliant with extended functionality
- **Created by:** StoboxRWAVaultFactory SC
- **Managed:** Directly by the factory contract
- **Works with:** Validation HasDIDRule SC for identity verification

### Core Features:
- **Diamond Standard (EIP-2535)**: Modular design and contract upgradability
- **ERC-20 Token Standard**: Standard token functionalities
- **Role-Based Access Control**: Grants specific addresses permission to mint and burn tokens
- **Validation Management**: Trusted addresses, validation rules, and pre-transfer validation checks
- **Treasury Operations**: Token withdrawals, allowances management, metadata access
- **Monetary Controls**: Issue, redeem, and transfer tokens from treasury
- **Emergency Controls**: Forceful transfers, minting, burning, and protocol pause capabilities

### Facets Structure:

> **What is a Facet?** A facet is a stateless smart contract or Solidity library with external functions. Facets are deployed and their functions are added to diamonds. Facets don't store data in their own storage but can define state and read/write to diamond storage. The term "facet" comes from the diamond industry - it's a side or flat surface of a diamond.

#### **Core Contract (Immutable)**
Contains immutable functions defined directly in the diamond that cannot be replaced or removed:
- `constructor()`, `receive()`, `fallback()`
- `transfer()`, `approve()`, `transferFrom()`
- `setDeployer()`, `transferOwnership()`
- `name()`, `symbol()`, `decimals()`, `totalSupply()`, `balanceOf()`, `maxSupply()`, `allowance()`
- `deployer()`, `owner()`, `getVersion()`

#### **DiamondCutFacet** (v1.0.0)
Enables modifying the diamond by adding, replacing, or removing function selectors. The diamond contains a mapping of function selectors to facet addresses, and this facet allows updating that mapping in a single transaction to prevent data corruption.

**Key Function:**
- `diamondCut(FacetCut[] calldata _diamondCut, address _init, bytes calldata _calldata)` - Modifies multiple functions across different facets in one transaction

**FacetCut Actions:**
- **Add**: Maps function selectors to a new facet address (reverts if selector already exists)
- **Replace**: Updates existing function selector mappings (reverts if selector is unset or already points to same address)
- **Remove**: Removes function selectors from mapping (reverts if already unset)

**Access Control:** Only deployer can execute via `enforceIsDeployer()`

#### **DiamondLoupeFacet** (v1.0.0)
A "loupe" is a small magnifying glass used to examine diamonds. This facet provides introspection into the diamond's modular structure.

**View Functions:**
- `facets()` - Returns all facets with their function selectors
- `facetFunctionSelectors(address _facet)` - Returns all function selectors for a specific facet
- `facetAddresses()` - Returns all facet addresses used in the diamond
- `facetAddress(bytes4 _functionSelector)` - Returns the facet address for a given function selector (or address(0) if not found)
- `supportsInterface(bytes4 interfaceId)` - ERC-165 interface detection

#### **ValidationFacet** (v1.0.1)
Implements comprehensive on-chain validation management and trust relationships for tokens. Typically configured by the deployer (StoboxRWAVaultFactory).

**Trust Management:**
- `trust(address account)` - Adds an address to the trusted list
- `distrust(address account)` - Removes an address from the trusted list
- `trustList()` - Returns array of all trusted addresses
- `isTrusted(address account)` - Checks if an address is trusted

**Rule Management:**
- `linkRule(address ruleAddress)` - Links a validation rule to the token
- `unlinkRule(address ruleAddress)` - Removes a validation rule from the token
- `ruleDescription(address ruleAddress)` - Returns description of a validation rule
- `validationRulesList()` - Returns array of all active validation rules

**Token Controls:**
- `pauseToken()` - Pauses token transfers
- `unpauseToken()` - Resumes token transfers
- `tokenPaused()` - Returns current pause status

**Validation Hooks:**
- `beforeUpdateValidation()` - Pre-transfer validation logic
- `afterUpdateValidation()` - Post-transfer validation logic

**Access Control:** Only deployer can manage trust, rules, and pause state

**Version Changes (v1.0.0 → v1.0.1):**
- Enhanced protocol security with pause/unpause capabilities
- Improved version documentation and contract traceability

#### **RolesFacet** (v1.0.0)
Provides role-based access control management using OpenZeppelin's AccessControl and AccessControlEnumerable standards. Uses LibRoles library for implementation logic.

**Role Management:**
- `initIERC165Roles()` - Initializes ERC-165 support for role interfaces
- `grantRole(bytes32 role, address account)` - Grants a role to an account
- `revokeRole(bytes32 role, address account)` - Revokes a role from an account
- `renounceRole(bytes32 role, address account)` - Allows account to renounce their own role

**Role Queries:**
- `hasRole(bytes32 role, address account)` - Checks if account has a specific role
- `getRoleAdmin(bytes32 role)` - Returns the admin role for a given role
- `getRoleMember(bytes32 role, uint256 index)` - Gets role member by index
- `getRoleMemberCount(bytes32 role)` - Returns number of accounts with a role
- `getRoleMembers(bytes32 role)` - Returns array of all accounts with a role

**Defined Roles:**
- `FINANCIAL_OPERATOR()` - Role for monetary operations (issue, redeem, transfers)
- `RECOVERY_OPERATOR()` - Role for emergency operations (forced actions, pause/unpause)

#### **MonetaryFacet** (v1.1.0)
Implements treasury management and monetary operations with issuance limit controls. Uses LibMonetary, LibRoles, and LibDiamond libraries.

**Treasury Management:**
- `setTreasury(address newTreasury)` - Sets the treasury address (deployer only)
- `treasury()` - Returns current treasury address

**Monetary Operations:**
- `issue(uint256 amount)` - Issues new tokens to treasury (FINANCIAL_OPERATOR only)
- `redeem(uint256 amount)` - Burns tokens from treasury (FINANCIAL_OPERATOR only)
- `transferFromTreasury(address to, uint256 amount)` - Transfers tokens from treasury (FINANCIAL_OPERATOR only)

**Issuance Controls:**
- `setMaxIssuance(uint256 newMaxIssuance)` - Sets maximum token issuance limit (deployer only)
- `maxIssuance()` - Returns current maximum issuance limit
- `totalIssued()` - Returns cumulative tokens issued

**Access Modifiers:**
- `onlyDeployer` - Enforced via LibDiamond.enforceIsDeployer()
- `onlyFinOps` - Enforced via LibRoles._checkRole(FINANCIAL_OPERATOR)

**Version Changes (v1.0.0 → v1.1.0):**
- Added issuance limit controls (`setMaxIssuance`, `maxIssuance`)
- Added issuance tracking (`totalIssued`)
- Enhanced monetary policy enforcement and regulatory compliance support
- Expanded from 5 to 8 total functions

#### **EmergencyFacet** (v1.0.0)
Provides privileged emergency operations for critical recovery scenarios. All functions are restricted to the RECOVERY_OPERATOR role.

**Emergency Operations:**
- `forcedTransfer(address from, address to, uint256 amount)` - Forcefully transfers tokens between addresses
- `forcedMint(address to, uint256 amount)` - Mints tokens directly to an address
- `forcedBurn(address from, uint256 amount)` - Burns tokens from an address

**Protocol Controls:**
- `pauseProtocolSTV3()` - Pauses the entire protocol
- `unpauseProtocolSTV3()` - Resumes protocol operations

**Access Control:** All functions require RECOVERY_OPERATOR role via LibRoles

### Protocol Libraries:
The protocol uses internal Solidity libraries for code reuse between facets:
- **LibDiamond** - Diamond pattern implementation, ownership, and access control
- **LibERC20** - ERC-20 token logic and storage
- **LibMonetary** - Treasury and monetary operations logic
- **LibRoles** - Role-based access control implementation
- **LibValidator** - Validation rules and compliance checks

### Deployment Details:
- **Network:** Arbitrum Mainnet
- **Chain ID:** 42161
- **Name:** Stobox Security Token v.3
- **Symbol:** STBX
- **Decimals:** 6
- **Network Status:** arbiscan.freshstatus.io
- **Release Notes:** [StoboxProtocolSTV3-Release-Notes.md](./StoboxProtocolSTV3-Release-Notes.md)

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
- **Repository:** [ST4DIDSC](https://github.com/StoboxTechnologies/ST4DIDSC)

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

---

## GitHub Public Repositories

### Smart Contracts & Blockchain Releases:
- **[Stobox-WEB3-Infrastructure](https://github.com/StoboxTechnologies/Stobox-WEB3-Infrastructure)** - Main infrastructure documentation and architecture overview
- **[Stobox_STV3_Protocol](https://github.com/StoboxTechnologies/Stobox_STV3_Protocol)** - StoboxProtocolSTV3 smart contract implementation
- **[ST4DIDSC](https://github.com/StoboxTechnologies/ST4DIDSC)** - Decentralized Identifier (DID) smart contract
- **[ST4RWAOfferingRegistry](https://github.com/StoboxTechnologies/ST4RWAOfferingRegistry)** - RWA Offering Registry implementation

### Documentation:
For detailed technical documentation about smart contracts, facets, libraries, and deployment guides, please refer to the respective repository documentation and release notes.




