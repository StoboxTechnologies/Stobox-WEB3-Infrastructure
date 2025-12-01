# Token Offering Module - Product Summary

## Overview

The **StoboxRWAOfferingRegistry** (Token Offering Module) is a comprehensive blockchain-based platform designed to manage Security Token Offerings (STOs) for Real World Assets (RWAs). Built on Arbitrum One using the Diamond Standard (EIP-2535), it provides a complete, upgradeable infrastructure for creating, managing, and enforcing compliance for tokenized asset offerings.

**Current Version:** v1.0.1  
**Network:** Arbitrum One Mainnet  
**Contract Address:** [`0xb05cf4f652eb5475c3e4246ab976785678a1806a`](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code)  
**Repository:** [ST4RWAOfferingRegistry](https://github.com/StoboxTechnologies/ST4RWAOfferingRegistry)

---

## Product Vision

The Token Offering Module serves as the **central compliance and lifecycle management hub** for the entire Stobox RWA ecosystem. It acts as a unified point of control where issuers can:
- Launch compliant security token offerings
- Enforce investor eligibility requirements
- Manage the complete offering lifecycle
- Track and validate investor purchases
- Control token distribution and lockup periods
- Ensure regulatory compliance through flexible rule engines

---

## Key Features

### 1. Complete STO Lifecycle Management

Comprehensive offering state management with full governance controls:

**Offering States:**
- **Creation** - Register new security token offerings with configurable parameters
- **Activation** - Launch offerings and enable investor participation
- **Suspension** - Temporarily pause offerings for compliance or operational reasons
- **Cancellation** - Cancel offerings before completion
- **Termination** - Formally close completed offerings

**Capabilities:**
- Define offering parameters (tokens, pricing, caps, dates)
- Set minimum and maximum investment amounts
- Configure lockup periods for investor protection
- Manage offering metadata and documentation

### 2. Flexible Rule Engine

Advanced compliance validation system with customizable investment rules:

**Rule Types:**
- **Investor Eligibility Rules** - Define who can participate (e.g., accredited investors, geographic restrictions)
- **Investment Amount Rules** - Set minimum/maximum investment thresholds
- **KYC/AML Requirements** - Enforce identity verification via StoboxDID integration
- **Custom Validation Logic** - Implement offering-specific compliance requirements

**Example Rules:**
- `AccreditedUSMinInvestmentRule` - Validates US accredited investor status and minimum investment
- DID-based validation rules for identity verification
- Token balance requirements
- Time-based restrictions

**Flexibility:**
- Link multiple rules to a single offering
- Unlink or update rules during offering lifecycle
- Combine rules for complex compliance scenarios
- Investor-specific rule overrides

### 3. Multi-Token Support

Versatile token management for diverse offering structures:

**Purchase Tokens:**
- Support for multiple ERC-20 tokens as payment methods
- USDC, USDT, DAI, or custom stablecoins
- Multi-currency offerings

**Security Tokens:**
- Integration with StoboxProtocolSTV3 (Diamond-based security tokens)
- Automatic token reservation and distribution
- Lock/unlock capabilities based on offering state

### 4. Role-Based Access Control

Granular permission system with specialized user roles:

**Admin Role:**
- Full system control and configuration
- Manage roles and permissions
- Emergency controls and system upgrades

**Tech Service Role:**
- Technical operations and maintenance
- Rule engine management
- System monitoring and diagnostics

**Offering Manager Role:**
- Create and manage offerings
- Configure offering parameters
- Approve investor purchases
- Manage offering lifecycle

**Investor Role:**
- View offering details
- Submit purchase requests
- Track investment status

### 5. Purchase Tracking & Management

Comprehensive investor purchase recording and validation:

**Purchase Features:**
- Record investor purchase transactions with full details
- Validate purchases against offering rules
- Track purchase history per investor
- Manage purchase limits and caps
- Support for purchase amendments and corrections

**Validation:**
- Real-time rule validation during purchase
- Integration with compliance rules
- Token availability checks
- Investment limit enforcement

### 6. Lockup Management

Configurable token lockup mechanisms for regulatory compliance:

**Lockup Features:**
- Set lockup periods per offering
- Define lockup reference points (offering start, purchase date, etc.)
- Automated token locking/unlocking
- Integration with StoboxProtocolSTV3 for enforcement

**Use Cases:**
- Regulatory compliance (Reg D, Reg S lockups)
- Vesting schedules
- Cliff periods
- Staged token releases

### 7. Token Reservation System

Advanced token allocation and reservation management:

**Capabilities:**
- Reserve security tokens from treasury for offerings
- Track reserved vs. distributed tokens
- Prevent over-allocation
- Automatic release of unreserved tokens post-offering

### 8. Upgradeable Architecture

Diamond Pattern (EIP-2535) enables seamless upgrades:

**Benefits:**
- Add new features without redeployment
- Fix bugs without losing state or history
- Modular facet structure for independent updates
- Preserve contract address and integrations

---

## Architecture

### Diamond Pattern Implementation

The Token Offering Module uses the **EIP-2535 Diamond Standard** for maximum modularity and upgradeability.

#### Core Facets:

**DiamondCutFacet** (v1.0.0)
- Handles system upgrades and facet management
- Add, replace, or remove functionality
- Admin-controlled upgrade mechanism

**DiamondLoupeFacet** (v1.0.0)
- Introspection and querying capabilities
- View available facets and functions
- System transparency and debugging

**RegistryRoleFacet** (v1.0.0)
- Access control and permission management
- Role assignment and revocation
- Multi-role support (admin, tech service, offering manager)

**OfferingGovernanceFacet** (v1.1.0)
- STO lifecycle management
- Offering creation, activation, suspension, cancellation, termination
- State transition controls
- Governance operations

**RegistryStorageFacet** (v1.1.0)
- Data storage and retrieval operations
- Offering metadata management
- Purchase record storage
- Historical data access

**RegistryRuleEngineFacet** (v1.0.0)
- Rule enforcement and compliance validation
- Link/unlink rules to offerings
- Validate investor eligibility
- Enforce investment limits

### Technology Stack

- **Blockchain:** Arbitrum One (Layer 2 scaling for Ethereum)
- **Solidity Version:** 0.8.28
- **Development Framework:** Foundry
- **Architecture Pattern:** EIP-2535 Diamond Standard
- **Token Standards:** ERC-20 (purchase tokens), StoboxProtocolSTV3 (security tokens)
- **Testing:** Comprehensive test suite with edge cases and invariants

---

## Ecosystem Integration

The Token Offering Module integrates seamlessly with the entire Stobox RWA infrastructure:

### 1. StoboxProtocolSTV3 Integration
- **Security token management** - Lock/unlock tokens based on offering state
- **Token reservation** - Reserve tokens from treasury for offerings
- **Distribution control** - Automate token distribution to investors
- **Compliance enforcement** - Coordinate with token-level validation rules

### 2. StoboxDID Integration
- **Identity verification** - Pull DID data for KYC/AML compliance
- **Investor validation** - Check DID attributes (accreditation status, jurisdiction, etc.)
- **Rule enforcement** - Use DID data in custom validation rules (e.g., `AccreditedUSMinInvestmentRule`)

### 3. StoboxRWAVaultFactory Integration
- **Token deployment** - Coordinate with factory for new security token creation
- **Vault management** - Link offerings to specific RWA vaults
- **Infrastructure setup** - Leverage factory for token initialization

### 4. Treasury Management
- **Token allocation** - Reserve tokens from STV3Treasury
- **Distribution** - Transfer tokens to investors upon purchase approval
- **Reconciliation** - Track distributed vs. reserved tokens

---

## Use Cases

### 1. Real Estate Tokenization
**Scenario:** Tokenize a commercial property worth $10M

**Offering Configuration:**
- Security Token: Property-backed tokens (1,000,000 tokens at $10 each)
- Purchase Tokens: USDC, USDT
- Minimum Investment: $10,000 (1,000 tokens)
- Maximum Investment: $500,000 (50,000 tokens)
- Lockup Period: 12 months from purchase date
- Eligibility: Accredited investors only

**Workflow:**
1. Create offering in OfferingRegistry
2. Link `AccreditedUSMinInvestmentRule` for compliance
3. Reserve 1,000,000 tokens from treasury
4. Activate offering
5. Investors purchase tokens (validated against rules)
6. Track purchases and enforce investment limits
7. Distribute tokens with 12-month lockup
8. Terminate offering when fully subscribed

### 2. Private Equity Fund
**Scenario:** Tokenize a $50M private equity fund

**Offering Configuration:**
- Security Token: Fund participation tokens
- Multiple investment tranches with different terms
- Rule-based investor segmentation (institutional vs. retail)
- Vesting schedules for management tokens
- Geographic restrictions (US, EU, Asia)

**Advanced Features:**
- Multiple purchase tokens (USDC, USDT, DAI)
- Tiered investment rules (different minimums by investor type)
- Custom lockup periods by tranche
- DID-based jurisdiction validation

### 3. Revenue-Sharing Asset
**Scenario:** Tokenize intellectual property with revenue streams

**Offering Configuration:**
- Security Token: IP revenue-sharing tokens
- Phased offering (seed, private, public rounds)
- Different pricing and lockups per phase
- Token reservation management across rounds

**Compliance:**
- Reg D compliance for US investors
- Reg S compliance for international investors
- KYC/AML via StoboxDID
- Transfer restrictions during lockup

---

## Benefits

### For Issuers

**Compliance Automation**
- Automated investor validation
- Rule-based eligibility enforcement
- Regulatory compliance by design
- Audit trail for all transactions

**Operational Efficiency**
- Streamlined offering creation
- Automated purchase processing
- Real-time reporting and analytics
- Reduced manual compliance work

**Flexibility**
- Configurable offering parameters
- Custom rule creation
- Multi-token support
- Adaptable to various asset classes

**Transparency**
- On-chain offering state
- Immutable purchase records
- Public verification of compliance
- Investor confidence through blockchain transparency

### For Investors

**Trust & Security**
- Smart contract-enforced rules
- Transparent offering terms
- Immutable purchase records
- Blockchain-verified compliance

**Access**
- 24/7 offering availability
- Global participation (subject to rules)
- Simplified purchase process
- Real-time purchase confirmation

**Protection**
- Enforced investment limits
- Lockup period guarantees
- Regulatory compliance verification
- Identity protection via DID

### For Regulators

**Compliance Verification**
- On-chain compliance rules
- Auditable transaction history
- KYC/AML integration
- Transparent offering lifecycle

**Oversight**
- Real-time offering monitoring
- Investor protection mechanisms
- Rule enforcement verification
- Regulatory reporting capabilities

---

## Security Features

### Access Control
- Role-based permissions with granular controls
- Admin, tech service, and offering manager roles
- Multi-signature support for critical operations
- Permission validation on all state-changing functions

### Rule Validation
- Comprehensive validation system for investment rules
- Pre-purchase validation checks
- Real-time compliance enforcement
- Fail-safe mechanisms for rule conflicts

### Purchase Validation
- Secure purchase recording mechanisms
- Double-spend prevention
- Investment limit enforcement
- Token availability checks

### Lockup Controls
- Smart contract-enforced lockup periods
- Integration with security token transfer restrictions
- Automated unlock mechanisms
- Tamper-proof lockup records

### Emergency Controls
- Ability to suspend offerings in case of issues
- Cancel offerings before completion if needed
- Admin override capabilities for critical situations
- Audit logging for all emergency actions

### Audit & Compliance
- Complete transaction history on-chain
- Immutable offering state records
- Event emission for all critical operations
- Integration with compliance tools and reporting

---

## Deployment Information

### Arbitrum One Mainnet

**Registry Contract:**
- Address: `0xb05cf4f652eb5475c3e4246ab976785678a1806a`
- Version: v1.0.1
- [View on Arbiscan](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code)

**Deployed Facets:**

| Facet | Version | Address |
|-------|---------|---------|
| DiamondInit | v1.0.0 | `0x92e70a6fb4e2d260d00051cb63a35cfa2b690eac` |
| DiamondCutFacet | v1.0.0 | `0xA297A8294719a0ee14B71481A216cC1f95F61940` |
| DiamondLoupeFacet | v1.0.0 | `0x2d3c020efCe3FC9c256562a4A9Cf8dDdBd9B4aF1` |
| RegistryRoleFacet | v1.0.0 | `0x7Dc69348d96ace706886e7BB41Db1BcE45145B9c` |
| RegistryStorageFacet | v1.1.0 | `0xc5946aed054b57A840B5593b047A5848e5452a41` |
| OfferingGovernanceFacet | v1.1.0 | `0xDB2d8D3c18250661AED2a7B0D81901F19E937230` |
| RegistryRuleEngineFacet | v1.0.0 | `0xe0f7cAe0F3845E24102ffdE2e42b2cEC2b0a6DD1` |

**Example Rules:**

| Rule | Version | Address |
|------|---------|---------|
| AccreditedUSMinInvestmentRule | v1.0.0 | `0x523d543d3d711d85740897aa7dbcd109ebbe7b6c` |

**Shadow Registry (Testing):**
- Address: `0xfb6169e6a0804a849ff724863c461d60a3a3045c`
- Version: v1.0.1

### Arbitrum Sepolia Testnet

**Registry Contract:**
- Address: `0x2113C87620221DbA86F82cE71E5f3EAEDB135F4b`
- [View on Sepolia Arbiscan](https://sepolia.arbiscan.io/address/0x2113C87620221DbA86F82cE71E5f3EAEDB135F4b#code)

---

## Roadmap & Future Enhancements

### Planned Features

**Advanced Rule Engine**
- Visual rule builder for non-technical users
- Rule templates for common compliance scenarios
- AI-powered compliance suggestions
- Multi-jurisdiction rule packages

**Enhanced Governance**
- On-chain voting for offering parameters
- Investor governance rights
- Amendment proposals and approval workflows
- Decentralized offering approval mechanisms

**Secondary Market Integration**
- Post-offering trading platform integration
- Transfer restriction management
- Market maker coordination
- Liquidity pool integration

**Reporting & Analytics**
- Real-time offering dashboards
- Investor analytics and insights
- Compliance reporting automation
- Export capabilities for regulatory filings

**Multi-Chain Support**
- Cross-chain offering coordination
- Bridge integration for multi-chain tokens
- Unified offering view across chains

**Mobile & Web Apps**
- Dedicated investor portal
- Mobile app for purchase and tracking
- Issuer dashboard for offering management
- Rule engine configuration UI

---

## Documentation & Resources

### Technical Documentation
- [GitHub Repository](https://github.com/StoboxTechnologies/ST4RWAOfferingRegistry)
- [Release Notes](./ST4RWAOfferingRegistry-Release-Notes.md)
- [Infrastructure Overview](./README.md)

### Smart Contract Verification
- [Mainnet Contract on Arbiscan](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code)
- [Testnet Contract on Sepolia Arbiscan](https://sepolia.arbiscan.io/address/0x2113C87620221DbA86F82cE71E5f3EAEDB135F4b#code)

### Related Documentation
- [StoboxProtocolSTV3 Release Notes](./StoboxProtocolSTV3-Release-Notes.md)
- [StoboxRWAVaultFactory Release Notes](./StoboxRWAVaultFactory-Release-Notes.md)
- [Stobox Docs V4](https://docs.stobox.io)

---

## Support & Contact

For integration support, technical questions, or partnership inquiries:

- **GitHub Issues:** [ST4RWAOfferingRegistry Issues](https://github.com/StoboxTechnologies/ST4RWAOfferingRegistry/issues)
- **Documentation:** [Stobox Docs](https://docs.stobox.io)
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

---

## License

This project is licensed under the terms specified in the [LICENSE](./LICENSE) file.

---

## Version History

**v1.0.1** (Current)
- Enhanced offering governance capabilities
- Improved storage facet functionality
- Production deployment to Arbitrum One
- Comprehensive testing and security audits

**v1.0.0** (Initial Release)
- Core diamond pattern implementation
- Basic STO lifecycle management
- Rule engine foundation
- Initial testnet deployment

---

**Last Updated:** December 2025  
**Current Version:** v1.0.1  
**Status:** Production (Arbitrum One Mainnet)

