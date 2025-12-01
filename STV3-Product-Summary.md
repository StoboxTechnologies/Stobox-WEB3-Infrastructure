# STV3 Protocol - Product Summary

## Overview

**StoboxProtocolSTV3** (STV3) represents Stobox's latest proprietary tokenization standard, engineered to deliver a more secure, efficient, and scalable framework for managing tokenized Real World Assets (RWAs). Built on the Diamond Standard (EIP-2535) and fully ERC-20 compliant, STV3 provides a modular, upgradeable infrastructure for security token lifecycle management with comprehensive compliance, treasury operations, and emergency controls.

**Current Version:** 1.0.0  
**Network:** Arbitrum Mainnet  
**Contract Address:** [`0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46`](https://arbiscan.io/address/0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46#code)  
**Repository:** [Stobox_STV3_Protocol](https://github.com/StoboxTechnologies/Stobox_STV3_Protocol)

**Token Details:**
- **Name:** Stobox Security Token v.3
- **Symbol:** STBX
- **Decimals:** 6
- **Chain ID:** 42161 (Arbitrum Mainnet)

---

## Product Vision

STV3 serves as the **core tokenization engine** for the Stobox RWA ecosystem, providing:
- **Regulatory Compliance** - Built-in validation rules and identity verification
- **Operational Flexibility** - Modular architecture enabling feature extensions without redeployment
- **Enterprise Security** - Multi-role access control with emergency recovery capabilities
- **Treasury Management** - Comprehensive monetary controls for token issuance and distribution
- **Upgrade Path** - Diamond Standard architecture supporting seamless protocol evolution

STV3 tokens represent ownership in tokenized real-world assets such as real estate, private equity, commodities, intellectual property, and other alternative investments, bringing institutional-grade compliance and flexibility to blockchain-based asset management.

---

## Key Features

### 1. Diamond Standard Architecture (EIP-2535)

Modular, upgradeable smart contract design enabling unlimited functionality expansion:

**Benefits:**
- **Modularity** - Functionality separated into independent facets
- **Upgradeability** - Add, replace, or remove features without redeployment
- **Gas Efficiency** - DELEGATECALL pattern minimizes deployment costs
- **Contract Size Limits** - Bypass 24KB contract size limitations
- **State Preservation** - Maintain token balances and state through upgrades

**Facet Architecture:**
- Core Contract (immutable ERC-20 functions)
- DiamondCutFacet (upgrade management)
- DiamondLoupeFacet (introspection)
- ValidationFacet (compliance rules)
- RolesFacet (access control)
- MonetaryFacet (treasury operations)
- EmergencyFacet (recovery controls)

### 2. ERC-20 Token Standard Compliance

Full compatibility with existing DeFi infrastructure and wallets:

**Standard Functions:**
- `transfer()`, `transferFrom()`, `approve()`, `allowance()`
- `balanceOf()`, `totalSupply()`, `name()`, `symbol()`, `decimals()`
- Event emission: `Transfer`, `Approval`

**Extended Features:**
- `maxSupply()` - Maximum token supply cap
- `deployer()` - Deployer address tracking
- `owner()` - Ownership management
- `getVersion()` - Semantic versioning (major.minor.patch)

**Compatibility:**
- Works with all ERC-20 compatible wallets
- Integrates with DEXs, lending protocols, and DeFi platforms
- Standard token approval and transfer mechanisms

### 3. Validation Management System

Comprehensive on-chain compliance and rule enforcement:

**Trust Management:**
- Maintain allowlist of trusted addresses
- Bypass validation for authorized entities
- Typically includes: factory, offering registry, treasury

**Validation Rules:**
- Link multiple validation rules to token
- Examples: HasDIDRule, AccreditationRule, JurisdictionRule
- Pre-transfer validation checks
- Flexible rule addition/removal

**Token Controls:**
- Pause/unpause token transfers
- Emergency stop mechanisms
- Compliance enforcement without transfer blocking

**Integration:**
- Works with StoboxDID for identity verification
- Integrates with StoboxRWAOfferingRegistry for offering-specific rules
- Supports custom validation logic per asset type

**Capabilities:**
- `trust()` / `distrust()` - Manage trusted addresses
- `linkRule()` / `unlinkRule()` - Connect validation rules
- `pauseToken()` / `unpauseToken()` - Control transfer state
- `beforeUpdateValidation()` / `afterUpdateValidation()` - Validation hooks

### 4. Role-Based Access Control

Granular permission system with specialized operational roles:

**DEFAULT_ADMIN_ROLE:**
- Ultimate authority over the protocol
- Grant/revoke all roles
- Modify role hierarchies

**FINANCIAL_OPERATOR:**
- Execute monetary operations
- Issue tokens to treasury
- Redeem tokens from treasury
- Transfer tokens from treasury
- Set maximum issuance limits

**RECOVERY_OPERATOR:**
- Emergency intervention capabilities
- Forced transfers in critical situations
- Forced minting/burning for recovery scenarios
- Protocol pause/unpause controls

**Role Management:**
- `grantRole()`, `revokeRole()`, `renounceRole()`
- `hasRole()`, `getRoleMember()`, `getRoleMemberCount()`, `getRoleMembers()`
- OpenZeppelin AccessControl standard integration
- Enumerable role tracking for transparency

### 5. Treasury Operations & Monetary Controls

Sophisticated token issuance and distribution management:

**Treasury Management:**
- Dedicated treasury address for token reserves
- Secure token storage for offerings
- Controlled distribution mechanisms

**Monetary Operations:**
- **Issue** - Mint new tokens to treasury (respects max supply and issuance limits)
- **Redeem** - Burn tokens from treasury
- **TransferFromTreasury** - Distribute tokens to investors or stakeholders

**Issuance Controls (v1.1.0+):**
- `setMaxIssuance()` - Define maximum cumulative token issuance
- `maxIssuance()` - Query current issuance limit
- `totalIssued()` - Track cumulative issued tokens
- Prevents excessive token creation
- Supports regulatory compliance and monetary policy

**Access Control:**
- All operations restricted to FINANCIAL_OPERATOR role
- Treasury address configurable by deployer only
- Prevents unauthorized token creation or distribution

### 6. Emergency Controls & Recovery

Critical safeguards for exceptional circumstances:

**Emergency Operations (RECOVERY_OPERATOR only):**
- `forcedTransfer()` - Relocate tokens between addresses (e.g., recover stolen tokens, correct errors)
- `forcedMint()` - Create tokens in emergency scenarios (e.g., system recovery)
- `forcedBurn()` - Destroy tokens when necessary (e.g., regulatory compliance)

**Protocol Controls:**
- `pauseProtocolSTV3()` - Halt all token operations
- `unpauseProtocolSTV3()` - Resume normal operations

**Use Cases:**
- Smart contract vulnerabilities requiring token movement
- Stolen or compromised wallet recovery
- Regulatory enforcement actions
- System-wide security incidents
- Correcting transaction errors

**Safeguards:**
- Restricted to RECOVERY_OPERATOR role only
- All emergency actions logged via events
- Transparent on-chain audit trail

### 7. Semantic Versioning System

Precise tracking of protocol evolution and facet composition:

**Version Format:** major.minor.patch

**Version Components:**
- **major** - Architectural changes (breaking changes)
- **minor** - Feature additions (facet composition changes)
- **patch** - Bug fixes and minor updates (diamondCut operations)

**Example Versioning:**
1. Initial deployment → `1.0.0`
2. Add facets during build → `1.0.1`, `1.0.2`
3. New feature release → `1.1.0`
4. Subsequent updates → `1.1.1`, `1.1.2`

**Implementation:**
- LibVersion library with Diamond Storage pattern
- `setVersion()` - Initialize version
- `increasePatch()` - Auto-increment on upgrades
- `getVersion()` - Query current version
- `VersionUpdated` event emission

**Benefits:**
- Clear upgrade history
- Transparent protocol evolution
- Compatibility tracking
- Audit trail for changes

---

## Architecture

### Diamond Standard Implementation

STV3 utilizes **EIP-2535 (Diamond Standard)** for maximum flexibility and upgradeability.

#### Core Contract (Immutable)

Contains essential functions that cannot be replaced or removed:

**ERC-20 Core:**
- `transfer()`, `approve()`, `transferFrom()`
- `name()`, `symbol()`, `decimals()`, `totalSupply()`, `balanceOf()`, `maxSupply()`, `allowance()`

**Administration:**
- `setDeployer()`, `transferOwnership()`
- `deployer()`, `owner()`, `getVersion()`

**Infrastructure:**
- `constructor()`, `receive()`, `fallback()`

#### DiamondCutFacet (v1.0.0)

Manages protocol upgrades and facet modifications:

**Capabilities:**
- Add new facets with function selectors
- Replace existing facet functions
- Remove obsolete functionality

**FacetCut Actions:**
- **Add** - Map function selectors to new facet
- **Replace** - Update function selector mappings
- **Remove** - Delete function selectors from diamond

**Access Control:**
- Restricted to deployer via `enforceIsDeployer()`

#### DiamondLoupeFacet (v1.0.0)

Provides introspection into diamond structure:

**Introspection Functions:**
- `facets()` - List all facets with their selectors
- `facetFunctionSelectors(address)` - Get selectors for specific facet
- `facetAddresses()` - List all facet addresses
- `facetAddress(bytes4)` - Find facet for specific selector
- `supportsInterface(bytes4)` - ERC-165 interface detection

**Use Cases:**
- System transparency and auditing
- Integration and tooling
- Debugging and diagnostics

#### ValidationFacet (v1.0.1)

On-chain compliance and validation management:

**Trust Management:**
- `trust()`, `distrust()` - Manage trusted addresses
- `trustList()`, `isTrusted()` - Query trust status

**Rule Management:**
- `linkRule()`, `unlinkRule()` - Connect validation rules
- `ruleDescription()`, `validationRulesList()` - Query rules

**Token Controls:**
- `pauseToken()`, `unpauseToken()`, `tokenPaused()` - Transfer controls

**Validation Hooks:**
- `beforeUpdateValidation()` - Pre-transfer checks
- `afterUpdateValidation()` - Post-transfer updates

**Version History:**
- v1.0.0 → v1.0.1: Added pause/unpause capabilities

#### RolesFacet (v1.0.0)

Role-based access control management:

**Role Administration:**
- `initIERC165Roles()` - Initialize role interfaces
- `grantRole()`, `revokeRole()`, `renounceRole()` - Manage roles

**Role Queries:**
- `hasRole()` - Check role assignment
- `getRoleAdmin()` - Get role administrator
- `getRoleMember()`, `getRoleMemberCount()`, `getRoleMembers()` - Enumerate roles

**Defined Roles:**
- `FINANCIAL_OPERATOR()` - Monetary operations
- `RECOVERY_OPERATOR()` - Emergency controls

**Standard:**
- OpenZeppelin AccessControl + AccessControlEnumerable

#### MonetaryFacet (v1.1.0)

Treasury and monetary operations management:

**Treasury:**
- `setTreasury()` - Configure treasury address (deployer only)
- `treasury()` - Query treasury address

**Operations:**
- `issue()` - Mint tokens to treasury (FINANCIAL_OPERATOR)
- `redeem()` - Burn tokens from treasury (FINANCIAL_OPERATOR)
- `transferFromTreasury()` - Distribute tokens (FINANCIAL_OPERATOR)

**Issuance Controls (v1.1.0):**
- `setMaxIssuance()` - Set issuance cap (deployer only)
- `maxIssuance()` - Query issuance limit
- `totalIssued()` - Track issued tokens

**Version History:**
- v1.0.0 → v1.1.0: Added issuance limits and tracking

#### EmergencyFacet (v1.0.0)

Emergency operations for critical scenarios:

**Emergency Transfers:**
- `forcedTransfer()` - Move tokens between addresses (RECOVERY_OPERATOR)
- `forcedMint()` - Create tokens in emergency (RECOVERY_OPERATOR)
- `forcedBurn()` - Destroy tokens when needed (RECOVERY_OPERATOR)

**Protocol Controls:**
- `pauseProtocolSTV3()` - Halt all operations (RECOVERY_OPERATOR)
- `unpauseProtocolSTV3()` - Resume operations (RECOVERY_OPERATOR)

### Protocol Libraries

Internal Solidity libraries for code reuse:

**LibDiamond** (v1.0.0)
- Diamond pattern implementation
- Storage management
- Access control enforcement
- Deployer and owner management

**LibERC20** (v1.0.0)
- ERC-20 token logic
- Balance tracking
- Transfer mechanisms
- Approval management

**LibMonetary** (v1.0.0)
- Treasury operations logic
- Issuance tracking
- Monetary policy enforcement

**LibRoles** (v1.0.0)
- Role-based access control
- Permission checking
- Role enumeration

**LibValidator** (v1.0.0)
- Validation rule management
- Trust list operations
- Pre-transfer validation

**LibVersion** (v1.0.0)
- Semantic versioning
- Version tracking
- Upgrade history

### Technology Stack

- **Blockchain:** Arbitrum One (Ethereum Layer 2)
- **Solidity Version:** 0.8.x
- **Token Standard:** ERC-20 with Diamond extensions
- **Architecture Pattern:** EIP-2535 Diamond Standard
- **Access Control:** OpenZeppelin AccessControl
- **Storage Pattern:** Diamond Storage (avoid collisions)

---

## Ecosystem Integration

STV3 seamlessly integrates with the Stobox RWA infrastructure:

### 1. StoboxRWAVaultFactory Integration

**Creation:**
- Factory deploys STV3 Diamond contracts
- Configures initial facets from blueprint packages
- Sets up treasury and roles
- Links validation rules

**Management:**
- Factory acts as deployer with special privileges
- Post-deployment facet updates
- Trust management for new tokens
- Pause/unpause controls

**Initial Setup:**
- `initialSetup()` - Configure treasury, roles, validation rules
- Grants FINANCIAL_OPERATOR and RECOVERY_OPERATOR roles
- Links HasDIDRule or custom validation rules
- Trusts factory as authorized address

### 2. StoboxRWAOfferingRegistry Integration

**Offering Management:**
- Registry reserves tokens from treasury for offerings
- Validates investor eligibility using STV3 validation rules
- Locks/unlocks tokens based on offering state
- Coordinates purchase approvals with token distribution

**Compliance:**
- Uses STV3 validation hooks for investor checks
- Enforces transfer restrictions during lockup
- Integrates with DID-based rules (via ValidationFacet)
- Manages token allocation across multiple offerings

### 3. StoboxDID Integration

**Identity Verification:**
- ValidationFacet queries StoboxDID for investor verification
- HasDIDRule ensures token holders have valid DIDs
- DID attributes used for compliance (accreditation, jurisdiction)
- Multi-address linking support (one DID, multiple wallets)

**Use Cases:**
- KYC/AML compliance before transfers
- Accredited investor verification
- Geographic restrictions
- Regulatory compliance automation

### 4. Treasury Management (STV3Treasury)

**Token Storage:**
- Dedicated treasury contract for token reserves
- Controlled by FINANCIAL_OPERATOR role
- Integration with MonetaryFacet for issuance

**Operations:**
- Issue tokens to treasury
- Distribute tokens to investors
- Redeem unused tokens
- Track reserved vs. available tokens

---

## Use Cases

### 1. Tokenized Real Estate

**Asset:** Commercial property worth $50M

**Token Configuration:**
- 5,000,000 STV3 tokens at $10 each
- Maximum supply: 5,000,000 tokens
- Decimals: 6 (fractional ownership)

**Validation Rules:**
- HasDIDRule - All token holders must have verified DID
- AccreditationRule - US investors must be accredited
- Geographic restrictions for non-US investors

**Treasury Operations:**
- Issue 5M tokens to treasury
- Reserve tokens for primary offering
- Distribute tokens upon investor purchase
- Manage secondary market transfers

**Compliance:**
- Pause token if regulatory issues arise
- Force transfer if wallet compromised
- Track all token movements for reporting

### 2. Private Equity Fund Tokenization

**Asset:** $100M PE fund with quarterly distributions

**Token Configuration:**
- 10,000,000 STV3 tokens
- Different token classes (Class A, Class B)
- Vesting schedules for management tokens

**Advanced Features:**
- Multiple validation rules (investor type, minimum investment)
- Issuance limits tied to fund capitalization
- Emergency controls for corporate actions
- Role-based distribution management

**Monetary Controls:**
- `setMaxIssuance()` - Cap at $100M equivalent
- `totalIssued()` - Track cumulative issuance
- Prevent over-issuance beyond fund size

### 3. Revenue-Sharing IP Tokenization

**Asset:** Film production with revenue sharing

**Token Configuration:**
- 1,000,000 STV3 tokens representing revenue rights
- Lockup periods for initial investors
- Transfer restrictions during production phase

**Lifecycle Management:**
- Initial issuance to treasury
- Staged token distribution (seed, pre-sale, public)
- Lockup enforcement via ValidationFacet
- Secondary market enablement post-lockup

**Emergency Scenarios:**
- Pause protocol if production halted
- Force burn if tokens need to be recalled
- Recovery operations for technical issues

### 4. Commodities Tokenization

**Asset:** Gold reserves in vault

**Token Configuration:**
- Each token = 1 gram of gold
- Redeemable for physical gold
- Backed 1:1 by vault holdings

**Features:**
- Real-time issuance based on vault additions
- Redemption burns tokens when gold withdrawn
- Validation ensures only verified entities trade
- Emergency controls for vault security events

---

## Benefits

### For Asset Issuers

**Regulatory Compliance**
- Built-in validation rules for automated compliance
- KYC/AML integration via StoboxDID
- Transfer restrictions and lockup enforcement
- Audit trail for all token movements

**Operational Flexibility**
- Modular architecture enables feature additions
- Upgrade without redeployment or migration
- Custom validation rules per asset type
- Multi-role access control for team management

**Treasury Control**
- Sophisticated monetary policy management
- Issuance caps prevent over-creation
- Controlled distribution mechanisms
- Track issued vs. available tokens

**Risk Management**
- Emergency pause capabilities
- Recovery operations for critical situations
- Multi-signature support for sensitive operations
- Transparent on-chain governance

### For Investors

**Compliance & Trust**
- Smart contract-enforced rules
- Transparent validation logic
- Immutable ownership records
- Blockchain-verified transactions

**Liquidity & Access**
- ERC-20 compatibility with DeFi ecosystem
- Standard wallet support
- Potential secondary market trading
- Fractional ownership opportunities

**Protection**
- Lockup periods enforced by smart contracts
- Recovery mechanisms for lost/stolen wallets
- Regulatory compliance verification
- Identity protection via DID

**Transparency**
- On-chain token supply and distribution
- Public verification of holdings
- Auditable transaction history
- Real-time balance updates

### For Regulators

**Oversight**
- Complete on-chain transaction history
- Identity verification integration
- Compliance rule enforcement
- Real-time monitoring capabilities

**Reporting**
- Automated audit trails
- Immutable compliance records
- KYC/AML integration points
- Transfer restriction enforcement

**Protection**
- Investor eligibility validation
- Transfer controls and lockups
- Emergency intervention capabilities
- Transparent governance actions

---

## Security Features

### Access Control Security

**Multi-Role System:**
- Separation of duties across roles
- Principle of least privilege
- Role enumeration for transparency
- Admin controls for role management

**Role Protections:**
- Only deployer can set treasury
- Only FINANCIAL_OPERATOR can issue/redeem
- Only RECOVERY_OPERATOR can force operations
- Critical functions restricted by role

### Validation Security

**Trust Management:**
- Allowlist of trusted addresses
- Deployer-controlled trust modifications
- Prevents unauthorized bypass of validation

**Rule Enforcement:**
- Pre-transfer validation checks
- Multiple rules can be linked
- Fail-safe mechanisms for rule failures
- Transparent rule logic on-chain

### Treasury Security

**Issuance Controls:**
- Maximum supply cap (immutable)
- Maximum issuance limit (configurable)
- Track cumulative issued tokens
- Prevent excessive token creation

**Distribution Controls:**
- Treasury address only modifiable by deployer
- Transfer from treasury requires FINANCIAL_OPERATOR
- All operations logged via events

### Emergency Security

**Controlled Access:**
- RECOVERY_OPERATOR role required for all emergency functions
- Multi-signature recommended for recovery role
- Transparent emergency actions via events

**Fail-Safe Mechanisms:**
- Protocol pause halts all operations
- Can unpause when situation resolved
- Emergency operations logged for audit

### Upgrade Security

**Diamond Cut Controls:**
- Only deployer can execute diamondCut
- Add/Replace/Remove actions validated
- Immutable core functions protected
- State preservation through upgrades

**Version Tracking:**
- All upgrades increment version
- Clear upgrade history on-chain
- Semantic versioning for compatibility

---

## Deployment Information

### Arbitrum Mainnet

**Protocol Contract:**
- **Address:** [`0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46`](https://arbiscan.io/address/0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46#code)
- **Name:** Stobox Security Token v.3
- **Symbol:** STBX
- **Decimals:** 6
- **Chain ID:** 42161
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

**Deployed Facets:**

Facets are deployed by StoboxRWAVaultFactory and composed into diamond instances per asset vault. Each STV3 token deployment uses facets from factory blueprint packages.

**Treasury Contract:**
- **Address:** [`0x17b94f6b0ddeaac3753b33ee006a7fdbc1292dcf`](https://arbiscan.io/address/0x17b94f6b0ddeaac3753b33ee006a7fdbc1292dcf#code)
- **Purpose:** Token reserve storage and distribution

**Validation Rules:**

Example validation rule deployed on Arbitrum:

- **HasDIDRule:** [`0x85a3eae3cd8cecac03d2fc44001e030621320acb`](https://arbiscan.io/address/0x85a3eae3cd8cecac03d2fc44001e030621320acb#code)

### Related Infrastructure

**Factory (Deploys STV3):**
- **Address:** [`0x096D75d0501c3B1479FFe15569192CeC998223b4`](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#code)
- **Version:** 1.0.1

**DID Registry:**
- **Address:** [`0x25E6036178656b1329ee51696407b367D8C6ba84`](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#code)

**Offering Registry:**
- **Address:** [`0xb05cf4f652eb5475c3e4246ab976785678a1806a`](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code)
- **Version:** 1.0.1

---

## Roadmap & Future Enhancements

### Planned Features

**Advanced Validation**
- Dynamic rule composition (AND/OR logic)
- Time-based validation rules
- Balance-based transfer limits
- Velocity checks (transfer frequency limits)

**Enhanced Monetary Controls**
- Scheduled token releases
- Automated vesting mechanisms
- Burn mechanisms for buybacks
- Dividend distribution facet

**Governance Integration**
- On-chain voting mechanisms
- Token holder governance rights
- Proposal and execution workflows
- Decentralized protocol upgrades

**Cross-Chain Support**
- Bridge integration for multi-chain deployment
- Unified token view across chains
- Cross-chain validation rules
- Inter-chain messaging for compliance

**Compliance Enhancements**
- Automated regulatory reporting
- Multi-jurisdiction rule packages
- Real-time compliance dashboards
- Regulator API integration

**DeFi Integration**
- Liquidity pool compatibility
- Collateral usage in lending protocols
- Yield generation mechanisms
- Composability with DeFi primitives

### Version Roadmap

**v1.1.0** (Planned)
- Enhanced validation rule composition
- Advanced monetary controls
- Gas optimizations

**v1.2.0** (Future)
- Cross-chain support
- Governance mechanisms
- Advanced compliance features

**v2.0.0** (Long-term)
- Architectural enhancements
- Major protocol upgrades
- New feature categories

---

## Documentation & Resources

### Technical Documentation
- [GitHub Repository](https://github.com/StoboxTechnologies/Stobox_STV3_Protocol)
- [Release Notes](./StoboxProtocolSTV3-Release-Notes.md)
- [Infrastructure Overview](./README.md)

### Smart Contract Verification
- [Mainnet Contract on Arbiscan](https://arbiscan.io/address/0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46#code)
- [Treasury Contract on Arbiscan](https://arbiscan.io/address/0x17b94f6b0ddeaac3753b33ee006a7fdbc1292dcf#code)

### Related Documentation
- [StoboxRWAVaultFactory Release Notes](./StoboxRWAVaultFactory-Release-Notes.md)
- [StoboxRWAOfferingRegistry Summary](./Token-Offering-Module-Summary.md)
- [ST4RWAOfferingRegistry Release Notes](./ST4RWAOfferingRegistry-Release-Notes.md)
- [Stobox Docs V4](https://docs.stobox.io)

### Educational Resources
- Diamond Standard (EIP-2535) Overview
- ERC-20 Token Standard Documentation
- Arbitrum Network Documentation
- Security Token Compliance Guidelines

---

## Support & Contact

For integration support, technical questions, or partnership inquiries:

- **GitHub Repository:** [Stobox_STV3_Protocol](https://github.com/StoboxTechnologies/Stobox_STV3_Protocol)
- **Infrastructure Repo:** [Stobox-WEB3-Infrastructure](https://github.com/StoboxTechnologies/Stobox-WEB3-Infrastructure)
- **Documentation:** [Stobox Docs](https://docs.stobox.io)
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

---

## License

This project is licensed under the terms specified in the [LICENSE](./LICENSE) file.

---

## Version History

**v1.0.0** (Current)
- Diamond Standard implementation
- Complete facet architecture (7 facets)
- ERC-20 compliance with extensions
- Role-based access control
- Treasury and monetary operations
- Emergency controls
- Validation management system
- Semantic versioning
- Production deployment to Arbitrum One

---

**Last Updated:** December 2025  
**Current Version:** 1.0.0  
**Status:** Production (Arbitrum One Mainnet)

