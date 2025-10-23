# ST4RWAOfferingRegistry v1.0.1 Release
**introduces comprehensive Real World Asset (RWA) offering management system with diamond pattern architecture, complete STO lifecycle management, flexible rule engine, and production deployment to Arbitrum One.**

## What's Changed

**Diamond Pattern Architecture:** Implemented EIP-2535 diamond pattern for upgradeable smart contract system with modular facet-based functionality.

**Complete STO Lifecycle Management:** Added comprehensive Security Token Offering (STO) management including creation, activation, suspension, cancellation, and termination capabilities.

**Flexible Rule Engine:** Implemented configurable investment rules and compliance requirements with support for custom validation logic and investor-specific settings.

**Multi-Token Support:** Added support for various purchase tokens and security tokens with configurable token addresses and validation.

**Role-Based Access Control:** Implemented granular permissions system with different user roles including admin, tech service, and offering management roles.

**Purchase Tracking:** Added comprehensive recording and management of investor purchases with detailed transaction history and validation.

**Lockup Management:** Implemented configurable token lockup periods and reference points for compliance and security requirements.

**Production Deployment:** Successfully deployed to Arbitrum One mainnet with full verification and comprehensive testing suite.

## Architecture

The system is built using the diamond pattern (EIP-2535) with the following core facets:

- **DiamondCutFacet v1.0.0** - Upgrade functionality and facet management
- **DiamondLoupeFacet v1.0.0** - Introspection and querying capabilities  
- **RegistryRoleFacet v1.0.0** - Access control and role management
- **OfferingGovernanceFacet v1.1.0** - STO lifecycle management and governance
- **RegistryStorageFacet v1.1.0** - Data storage and retrieval operations
- **RegistryRuleEngineFacet v1.0.0** - Rule enforcement and compliance validation

## Key Features

- **Complete STO Lifecycle Management**: Create, activate, suspend, cancel, and terminate offerings
- **Flexible Rule Engine**: Configurable investment rules and compliance requirements
- **Multi-Token Support**: Support for various purchase tokens and security tokens
- **Role-Based Access Control**: Granular permissions for different user types
- **Purchase Tracking**: Comprehensive recording and management of investor purchases
- **Lockup Management**: Configurable token lockup periods and reference points
- **Upgradeable Architecture**: Diamond pattern allows for seamless upgrades
- **Comprehensive Testing**: Extensive test suite with edge cases and invariants

## Deployed Contracts

### Arbitrum One Mainnet

**[StoboxRWAOfferingRegistry](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code) | v1.0.1 | 0xb05cf4f652eb5475c3e4246ab976785678a1806a**

**[DiamondInit v1.0.0](https://arbiscan.io/address/0x92e70a6fb4e2d260d00051cb63a35cfa2b690eac#code) | 0x92e70a6fb4e2d260d00051cb63a35cfa2b690eac**

**[DiamondCutFacet v1.0.0](https://arbiscan.io/address/0xA297A8294719a0ee14B71481A216cC1f95F61940#code) | 0xA297A8294719a0ee14B71481A216cC1f95F61940**

**[DiamondLoupeFacet v1.0.0](https://arbiscan.io/address/0x2d3c020efCe3FC9c256562a4A9Cf8dDdBd9B4aF1#code) | 0x2d3c020efCe3FC9c256562a4A9Cf8dDdBd9B4aF1**

**[RegistryRoleFacet v1.0.0](https://arbiscan.io/address/0x7Dc69348d96ace706886e7BB41Db1BcE45145B9c#code) | 0x7Dc69348d96ace706886e7BB41Db1BcE45145B9c**

**[RegistryStorageFacet v1.1.0](https://arbiscan.io/address/0xc5946aed054b57A840B5593b047A5848e5452a41#code) | 0xc5946aed054b57A840B5593b047A5848e5452a41**

**[OfferingGovernanceFacet v1.1.0](https://arbiscan.io/address/0xDB2d8D3c18250661AED2a7B0D81901F19E937230#code) | 0xDB2d8D3c18250661AED2a7B0D81901F19E937230**

**[RegistryRuleEngineFacet v1.0.0](https://arbiscan.io/address/0xe0f7cAe0F3845E24102ffdE2e42b2cEC2b0a6DD1#code) | 0xe0f7cAe0F3845E24102ffdE2e42b2cEC2b0a6DD1**

### ST4RWAOfferingRegistry Rules

**[AccreditedUSMinInvestmentRule v1.0.0](https://arbiscan.io/address/0x523d543d3d711d85740897aa7dbcd109ebbe7b6c#code) | 0x523d543d3d711d85740897aa7dbcd109ebbe7b6c**

### Shadow Registry

**[ShadowOfferingRegistry](https://arbiscan.io/address/0xfb6169e6a0804a849ff724863c461d60a3a3045c#code) | v1.0.1 | 0xfb6169e6a0804a849ff724863c461d60a3a3045c**

### Testnet

**[OfferingRegistry](https://sepolia.arbiscan.io/address/0x2113C87620221DbA86F82cE71E5f3EAEDB135F4b#code) | 0x2113C87620221DbA86F82cE71E5f3EAEDB135F4b**

## Technical Details

- **Solidity Version**: 0.8.28
- **Foundry Framework**: Built with Foundry for comprehensive testing and deployment
- **Diamond Pattern**: EIP-2535 compliant upgradeable architecture
- **Role-Based Access**: Granular permission system with multiple user roles
- **Rule Engine**: Flexible validation system for compliance requirements
- **Multi-Token Support**: Support for various ERC-20 tokens and security tokens
- **Comprehensive Testing**: Extensive test suite including edge cases and invariants

## Security Features

- **Access Control**: Role-based permissions with admin and tech service roles
- **Rule Validation**: Comprehensive validation system for investment rules
- **Purchase Validation**: Secure purchase recording and validation mechanisms
- **Lockup Controls**: Configurable token lockup periods for compliance
- **Emergency Controls**: Ability to suspend and cancel offerings when needed

## Integration

The ST4RWAOfferingRegistry integrates with:
- **Stobox Protocol STV3**: For security token management
- **DID System**: For investor identity verification
- **Treasury Management**: For token reservation and distribution
- **Rule Engine**: For compliance and validation requirements

For complete technical documentation, see the [project repository](https://github.com/StoboxTechnologies/ST4RWAOfferingRegistry).

---

# ST4RWAOfferingRegistry v1.0.0
**This is the initial release of the ST4RWAOfferingRegistry, establishing the foundational architecture and core functionality for Real World Asset offering management.**

## What's Changed

**Initial Diamond Pattern Implementation:** Established the core diamond pattern architecture with basic facet structure.

**Core Facet Development:** Implemented essential facets for registry management, role control, and offering governance.

**Basic STO Management:** Added fundamental Security Token Offering creation and management capabilities.

**Rule Engine Foundation:** Established the basic rule engine framework for compliance validation.

**Testing Framework:** Implemented comprehensive testing suite with Foundry framework.

**Initial Deployment:** Deployed initial version to testnet for validation and testing.

For a complete list of changes, see the [full changelog](https://github.com/StoboxTechnologies/ST4RWAOfferingRegistry/commits/v1.0.0).
