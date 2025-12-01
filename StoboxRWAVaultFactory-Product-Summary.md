# StoboxRWAVaultFactory - Product Summary

## Overview

**StoboxRWAVaultFactory** is a foundational smart contract designed to facilitate the creation and management of tokenized Real-World Asset (RWA) Vaults using the Diamond Standard (EIP-2535). It serves as a secure, upgradeable, and modular entry point for deploying RWA vaults, each potentially representing a distinct real-world asset such as real estate, commodities, private equity, or financial instruments.

**Current Version:** 1.0.1  
**Network:** Arbitrum One Mainnet  
**Contract Address:** [`0x096D75d0501c3B1479FFe15569192CeC998223b4`](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#code)  
**Name:** StoboxRWAVaultFactory  
**Chain ID:** 42161 (Arbitrum Mainnet)  
**Repository:** [Stobox-WEB3-Infrastructure](https://github.com/StoboxTechnologies/Stobox-WEB3-Infrastructure)

---

## Product Vision

The StoboxRWAVaultFactory serves as the **deployment engine and orchestration hub** for the Stobox RWA ecosystem, providing:

- **Standardized Deployment** - Consistent, repeatable token vault creation process
- **Blueprint Architecture** - Reusable facet packages for different asset types
- **Lifecycle Management** - Complete token management from creation to operation
- **Infrastructure Automation** - Automated setup of roles, treasury, and validation rules
- **Upgrade Capability** - Diamond Standard enables seamless protocol evolution
- **Multi-Asset Support** - Single factory deploys tokens for diverse asset classes

The factory abstracts the complexity of deploying sophisticated Diamond-based security tokens, enabling issuers to launch compliant, feature-rich tokenized assets with minimal technical overhead while maintaining maximum flexibility and upgradeability.

---

## Key Features

### 1. Diamond Standard Architecture (EIP-2535)

Modular factory design enabling unlimited functionality expansion:

**Benefits:**
- **Modularity** - Separate concerns across independent facets
- **Upgradeability** - Add, replace, remove features without redeployment
- **Gas Efficiency** - Shared logic reduces deployment costs
- **Unlimited Size** - Bypass 24KB contract size limitations
- **State Preservation** - Maintain factory state through upgrades

**Factory Facets:**
- Core Contract (immutable administrative functions)
- DiamondCutFacet (upgrade management)
- DiamondLoupeFacet (introspection)
- TokenBlueprintFacet (facet package management)
- CreateSTV3Facet (token deployment)
- SetInfrastructureFacet (initial configuration)
- STV3TokenManagementFacet (post-deployment management)
- FactoryRolesFacet (access control)

**Architecture Advantages:**
- Single factory address for all deployments
- Consistent upgrade path for all tokens
- Centralized management interface
- Transparent facet composition

### 2. Blueprint System for Facet Packages

Reusable facet compositions for standardized token deployments:

**Blueprint Concept:**
- Organize token facets into named packages
- Each package represents a specific token configuration
- Reuse across multiple token deployments
- Versioned facet management

**Blueprint Management:**
- `addTokenFacet()` - Register new facet with name and version
- `addPackage()` - Create blueprint from selected facets
- `packageDetails()` - Get facet addresses in package
- `fullPackageInfo()` - Get comprehensive package metadata (names, addresses, versions)

**Package Components:**
- DiamondCutFacet
- DiamondLoupeFacet
- ValidationFacet
- RolesFacet
- MonetaryFacet
- EmergencyFacet

**Benefits:**
- **Consistency** - All tokens from same blueprint have identical features
- **Efficiency** - Deploy once, reuse many times
- **Versioning** - Track facet versions in packages
- **Flexibility** - Create asset-specific blueprints
- **Quality** - Pre-tested, audited facet combinations

**Use Cases:**
- "Real Estate Package" - Specific facets for real estate tokens
- "Private Equity Package" - PE fund-specific configuration
- "Commodity Package" - Commodities tokenization setup
- "Custom Package" - Client-specific requirements

### 3. Automated Token Creation

One-transaction deployment of complete STV3 Diamond tokens:

**Token Creation Process:**
1. Deploy new STV3 Diamond contract
2. Deploy associated treasury contract
3. Configure initial facets from blueprint
4. Set token metadata (name, symbol, decimals, maxSupply)
5. Initialize Diamond with specified version
6. Link deployer (factory) as authorized address

**CreateSTV3Facet Capabilities:**
- `createSecurityToken()` - Deploy token and treasury
- `setTokenVersionToCreate()` - Configure version for new tokens
- `setInitializer()` - Set initialization contract
- `tokenCreationDetails()` - Query token deployment info
- `tokenCount()` - Total tokens created
- `generalTokenList()` - All deployed token addresses

**Creation Parameters:**
- **name** - Token name (e.g., "Property XYZ Token")
- **symbol** - Token symbol (e.g., "PXYZ")
- **decimals** - Decimal places (typically 6 or 18)
- **maxSupply** - Maximum token supply cap
- **initCalldata** - Custom initialization data

**Deployment Tracking:**
- Track all created tokens
- Record creation timestamps
- Monitor token versions
- Maintain deployment history

**Benefits:**
- Single transaction deployment
- Standardized token structure
- Automated treasury setup
- Consistent initialization
- Deployment audit trail

### 4. Infrastructure Setup & Configuration

Comprehensive post-deployment configuration automation:

**SetInfrastructureFacet:**

Complete initial setup in one transaction via `initialSetup()`:

**Setup Operations:**
1. **Treasury Configuration**
   - Set treasury address for token reserves
   - Link treasury with token contract

2. **Role Assignment**
   - Grant FINANCIAL_OPERATOR role for monetary operations
   - Grant RECOVERY_OPERATOR role for emergency controls
   - Configure role hierarchies

3. **Validation Rules**
   - Link validation rules (e.g., HasDIDRule)
   - Connect compliance logic
   - Enable transfer restrictions

4. **Trust Management**
   - Trust factory as authorized address
   - Trust offering registry if applicable
   - Configure trusted entities

5. **Initial Verification**
   - Verify all roles assigned correctly
   - Confirm validation rules linked
   - Validate treasury connection

**Parameters:**
- `tokenAddress` - Deployed token to configure
- `treasury` - Treasury contract address
- `financialOperator` - Address for monetary operations
- `recoveryOperator` - Address for emergency controls
- `validationRules[]` - Array of validation rule addresses

**Benefits:**
- **Automation** - No manual configuration needed
- **Consistency** - Same setup process for all tokens
- **Speed** - Single transaction setup
- **Reliability** - Pre-tested configuration logic
- **Audit Trail** - All setup actions logged

**Access Control:**
- Restricted to master role only
- Prevents unauthorized configuration
- Ensures setup integrity

### 5. Token Blueprint Management

Sophisticated facet package organization and versioning:

**TokenBlueprintFacet Capabilities:**

**Facet Registration:**
- `addTokenFacet()` - Register individual facet
  - facetAddress - Deployed facet contract
  - facetName - Human-readable identifier
  - facetVersion - Semantic version
- Track all available facets
- Version management per facet

**Package Creation:**
- `addPackage()` - Assemble facets into package
  - packageName - Blueprint identifier
  - facetsToAdd[] - Array of facet addresses
- Create reusable compositions
- Named package management

**Query Capabilities:**
- `packageDetails()` - Get facet addresses in package
- `fullPackageInfo()` - Complete package metadata
  - Facet names array
  - Facet addresses array
  - Facet versions array
- `tokenFacetDetails()` - Individual facet info
- `tokenFacetExists()` - Verify facet registration
- `packageCount()` - Total registered packages

**Version Tracking:**
- Each facet has semantic version
- Packages inherit facet versions
- Track version evolution
- Enable version-specific deployments

**Benefits:**
- **Organization** - Structured facet management
- **Reusability** - Deploy tested combinations
- **Transparency** - Query package composition
- **Versioning** - Track facet evolution
- **Quality Control** - Approved facet packages only

### 6. Post-Deployment Token Management

Comprehensive lifecycle management for deployed tokens:

**STV3TokenManagementFacet:**

**Facet Updates:**
- `updateFacetsOfSTV3Token()` - Upgrade token facets
  - Add new facets via diamondCut
  - Replace existing facets
  - Remove obsolete facets
  - Preserve token state through upgrades

**Pause Controls:**
- `pauseSTV3Token()` - Halt token transfers
- `unpauseSTV3Token()` - Resume token operations
- Emergency stop capability
- Compliance-driven pauses

**Trust Management:**
- `trustForSTV3Token()` - Add address to trusted list
- `distrustForSTV3Token()` - Remove from trusted list
- Configure bypass addresses
- Update trust relationships

**Validation Rules:**
- `linkRuleForSTV3Token()` - Connect validation rule
- `unlinkRuleForSTV3Token()` - Remove validation rule
- Dynamic compliance updates
- Rule composition management

**Monetary Controls:**
- `setMaxIssuanceForSTV3Token()` - Set issuance cap
- Configure monetary policy
- Prevent over-issuance
- Regulatory compliance

**Access Control:**
- Restricted to DEFAULT_ADMIN_ROLE
- Centralized management authority
- Prevents unauthorized modifications
- Secure control plane

**Use Cases:**
- Protocol upgrades across all tokens
- Emergency pause during security incidents
- Compliance rule updates
- Trust relationship changes
- Monetary policy adjustments

### 7. Master Role Access Control

Dedicated administrative authority for factory operations:

**Master Role:**
- Single address with supreme factory authority
- Assigned during deployment
- Transferable to new master
- Controls critical factory functions

**Master Capabilities:**
- `setMaster()` - Transfer master role to new address
- `setFactoryVersion()` - Update factory version
- Execute diamondCut via DiamondCutFacet
- Configure factory parameters
- Manage factory upgrades

**Master-Restricted Functions:**
- Token blueprint registration
- Package creation
- Infrastructure setup execution
- Token creation (via CreateSTV3Facet)
- Post-deployment management

**Transition to Role-Based:**
- DiamondCutFacet (v1.0.0 → v1.0.1): Changed from master-only to DEFAULT_ADMIN_ROLE
- STV3TokenManagementFacet: Uses DEFAULT_ADMIN_ROLE
- Gradual migration to granular role system

**Security:**
- Single point of authority
- Multi-signature wallet recommended
- Transparent role transitions
- Immutable master queries

### 8. Semantic Versioning System

Precise tracking of factory evolution and composition:

**Version Format:** major.minor.patch

**Version Components:**

**major** - Architectural version
- Fundamental factory infrastructure changes
- Breaking changes to factory architecture
- Expected to remain 1 unless complete redesign

**minor** - Factory Diamond build version
- New facets added to factory
- Feature additions to factory capabilities
- Business logic changes

**patch** - Local facet changes
- Facet modifications via diamondCut
- Bug fixes and minor updates
- Incremented automatically

**Core Contract Functions:**
- `setFactoryVersion()` - Update factory version (master only)
- `getVersion()` - Query current version (returns major, minor, patch)

**Version Tracking:**
- Uses LibVersion library
- Diamond Storage pattern
- Event emission on updates
- Clear version history

**Example Versioning:**
1. Factory deployment → `1.0.0`
2. Add TokenBlueprintFacet → `1.0.1`
3. Add CreateSTV3Facet → `1.0.2`
4. New feature release → `1.1.0`
5. Bug fix → `1.1.1`

**Benefits:**
- Clear upgrade history
- Compatibility tracking
- Transparent evolution
- Integration versioning

---

## Architecture

### Diamond Standard Implementation

The factory uses **EIP-2535 (Diamond Standard)** for maximum modularity.

#### Core Contract (Immutable)

Essential administrative functions that cannot be replaced:

**Administration:**
- `setMaster(address newMaster)` - Transfer master authority
- `setFactoryVersion(uint256 major, uint256 minor, uint256 patch)` - Update version
- `master()` - Query master address
- `getVersion()` - Get current version (major, minor, patch)

**Infrastructure:**
- `constructor()` - Initialize factory with master and version
- `receive()` - Accept ETH transfers
- `fallback()` - Diamond function resolution

**Access Modifier:**
- `onlyMaster` - Restricts functions to master role

#### DiamondCutFacet (v1.0.1)

Factory upgrade and modification management:

**Capabilities:**
- `diamondCut()` - Add, replace, or remove factory facets
- FacetCut actions: Add, Replace, Remove
- Batch modifications in single transaction

**Access Control:**
- v1.0.0: Master-only
- v1.0.1: DEFAULT_ADMIN_ROLE (role-based)

**Version Change Benefits:**
- Granular permission management
- Multi-role administration support
- Enhanced security through role separation

#### DiamondLoupeFacet (v1.0.0)

Factory introspection and transparency:

**Query Functions:**
- `facets()` - All factory facets with selectors
- `facetFunctionSelectors(address)` - Selectors for specific facet
- `facetAddresses()` - All facet addresses
- `facetAddress(bytes4)` - Facet for specific selector
- `supportsInterface(bytes4)` - ERC-165 interface detection

**Use Cases:**
- Verify factory composition
- Audit facet configuration
- Integration tooling
- Debugging and diagnostics

#### TokenBlueprintFacet (v1.0.1)

Facet package management and organization:

**Facet Registration:**
- `addTokenFacet()` - Register facet with metadata
- `tokenFacetDetails()` - Query facet name and version
- `tokenFacetExists()` - Verify facet registration

**Package Management:**
- `addPackage()` - Create facet package
- `packageDetails()` - Get package facet addresses
- `fullPackageInfo()` - Complete package metadata (v1.0.1)
- `packageCount()` - Total registered packages

**Access:** Master role only

**Version History:**
- v1.0.0 → v1.0.1: Added `fullPackageInfo()` for enhanced metadata

#### CreateSTV3Facet (v1.0.1)

Security token deployment engine:

**Token Creation:**
- `createSecurityToken()` - Deploy STV3 token with treasury
  - Parameters: name, symbol, decimals, maxSupply, initCalldata
  - Returns: token address and treasury address
  - Creates Diamond contract with blueprint facets

**Configuration:**
- `setTokenVersionToCreate()` - Configure version for new tokens
- `setInitializer()` - Set initialization contract

**Tracking:**
- `tokenCreationDetails()` - Deployment info for specific token
- `tokenVersionToCreate()` - Version assigned to next token
- `tokenInitializer()` - Current initializer contract
- `tokenCount()` - Total tokens created
- `generalTokenList()` - All created token addresses

**Access:** Master role

**Version History:**
- v1.0.0 → v1.0.1: Enhanced tracking and metadata

#### SetInfrastructureFacet (v1.1.0)

Initial token configuration automation:

**Setup Function:**
- `initialSetup()` - Complete token initialization
  - tokenAddress - Token to configure
  - treasury - Treasury contract address
  - financialOperator - FINANCIAL_OPERATOR role
  - recoveryOperator - RECOVERY_OPERATOR role
  - validationRules[] - Rules to link

**Setup Operations:**
1. Set treasury address
2. Grant FINANCIAL_OPERATOR role
3. Grant RECOVERY_OPERATOR role
4. Link validation rules (loop through array)
5. Trust factory address

**Access:** Master role only

**Version History:**
- v1.0.0 → v1.1.0: Enhanced automation and validation

#### STV3TokenManagementFacet (v1.1.0)

Post-deployment token lifecycle management:

**Facet Management:**
- `updateFacetsOfSTV3Token()` - Execute diamondCut on token

**Pause Controls:**
- `pauseSTV3Token()` - Pause token transfers
- `unpauseSTV3Token()` - Resume token transfers

**Trust Management:**
- `trustForSTV3Token()` - Add trusted address
- `distrustForSTV3Token()` - Remove trusted address

**Validation Rules:**
- `linkRuleForSTV3Token()` - Link validation rule
- `unlinkRuleForSTV3Token()` - Unlink validation rule

**Monetary Controls:**
- `setMaxIssuanceForSTV3Token()` - Set issuance limit (v1.1.0)

**Access:** DEFAULT_ADMIN_ROLE

**Version History:**
- v1.0.0 → v1.1.0: Added `setMaxIssuanceForSTV3Token()`

#### FactoryRolesFacet (v1.0.0)

Role-based access control for factory:

**Role Management:**
- `grantRole()` - Assign role to address
- `revokeRole()` - Remove role from address
- `renounceRole()` - Self-revoke role

**Role Queries:**
- `hasRole()` - Check role assignment
- `getRoleAdmin()` - Get role administrator
- `getRoleMember()` - Get role member by index
- `getRoleMemberCount()` - Count role members

**Defined Roles:**
- `DEFAULT_ADMIN_ROLE()` - Administrator role

**Standard:**
- OpenZeppelin AccessControl + AccessControlEnumerable

### Factory Libraries

Internal Solidity libraries for code reuse:

**LibDiamond** (v1.0.0)
- Diamond pattern implementation
- Storage management via DiamondStorage
- Access control enforcement (master, deployer)
- Facet management logic

**LibVersion** (v1.0.0)
- Semantic versioning (major.minor.patch)
- Version storage via fixed slot
- Version increment logic
- VersionUpdated event emission

**LibBlueprint** (v1.0.1)
- Token facet blueprint management
- Package organization
- Facet metadata storage
- Query logic for packages

**LibCreateSTV3** (v1.0.0)
- STV3 token creation logic
- Treasury deployment
- Token tracking
- Creation metadata management

**LibFactoryRoles** (v1.0.0)
- Role-based access control
- Permission checking
- Role enumeration

### Technology Stack

- **Blockchain:** Arbitrum One (Ethereum Layer 2)
- **Solidity Version:** 0.8.x
- **Architecture Pattern:** EIP-2535 Diamond Standard
- **Access Control:** Master role + OpenZeppelin AccessControl
- **Storage Pattern:** Diamond Storage (collision-free)
- **Deployment Framework:** Foundry

---

## Ecosystem Integration

The factory is the **orchestration hub** connecting all Stobox RWA components:

### 1. StoboxProtocolSTV3 Creation

**Token Deployment:**
- Factory deploys all STV3 Diamond contracts
- Configures initial facets from blueprints
- Sets up deployer (factory) as privileged address
- Links validation rules during creation

**Deployer Privileges:**
- Factory acts as deployer for all tokens
- Can modify token facets via STV3TokenManagementFacet
- Manages trust relationships
- Controls validation rule linking

**Token Lifecycle:**
1. Create token via `createSecurityToken()`
2. Setup infrastructure via `initialSetup()`
3. Manage token via STV3TokenManagementFacet
4. Upgrade token facets as needed

### 2. Treasury Integration (STV3Treasury)

**Treasury Deployment:**
- Factory creates treasury contract for each token
- Links treasury to token via MonetaryFacet
- Configures treasury address during setup

**Treasury Operations:**
- FINANCIAL_OPERATOR role assigned during setup
- Treasury stores token reserves
- MonetaryFacet manages issuance/redemption
- Controlled distribution to investors

### 3. StoboxDID Integration

**Validation Rules:**
- Factory links HasDIDRule during `initialSetup()`
- Validates token holders have active DIDs
- Enforces KYC/AML compliance
- Supports custom DID-based rules

**Setup Process:**
- validationRules parameter in `initialSetup()`
- Automatic linking to token ValidationFacet
- Immediate compliance enforcement
- Post-deployment rule modifications via STV3TokenManagementFacet

### 4. StoboxRWAOfferingRegistry Integration

**Offering Coordination:**
- Tokens created by factory used in offerings
- Factory trusts offering registry (if configured)
- Registry manages token distribution
- Lock/unlock coordination

**Trust Configuration:**
- Factory can trust offering registry during setup
- Allows registry to interact with tokens
- Facilitates purchase processing
- Enables lockup management

### 5. Multi-Token Management

**Centralized Control:**
- Factory maintains list of all created tokens
- Single interface for all token management
- Consistent upgrade path
- Unified monitoring

**Operations at Scale:**
- Update facets across multiple tokens
- Apply policy changes system-wide
- Monitor all token activity
- Centralized reporting

---

## Use Cases

### 1. Real Estate Portfolio Tokenization

**Scenario:** Investment firm tokenizing 10 commercial properties

**Factory Workflow:**

**Setup Phase:**
1. Create "Real Estate Blueprint" package
   - ValidationFacet with property-specific rules
   - MonetaryFacet with dividend distribution
   - Custom facets for rent payments

**Deployment Phase:**
2. For each property:
   ```solidity
   createSecurityToken(
       "Property ABC Token",
       "PABC",
       6,
       1000000 * 10**6,  // 1M tokens
       initData
   )
   ```

3. Configure infrastructure:
   ```solidity
   initialSetup(
       tokenAddress,
       treasuryAddress,
       financialOperatorAddress,
       recoveryOperatorAddress,
       [hasDIDRule, accreditationRule, propertyRule]
   )
   ```

**Management Phase:**
4. Post-deployment management:
   - Link offering via `trustForSTV3Token()`
   - Set issuance limits per property valuation
   - Update rules as regulations change
   - Upgrade facets for new features

**Benefits:**
- Consistent token structure across portfolio
- Centralized management of 10+ tokens
- Standardized compliance rules
- Efficient deployment process

### 2. Private Equity Fund Tokenization

**Scenario:** $100M PE fund with multiple investment rounds

**Blueprint Configuration:**
- Create "Private Equity Blueprint"
- Include vesting facets
- Add governance facets
- Configure voting mechanisms

**Multi-Round Deployment:**

**Seed Round:**
```solidity
createSecurityToken("PE Fund Seed", "PEFS", 18, 10000000 * 10**18, seedInit)
initialSetup(..., [hasDIDRule, accreditedInvestorRule], ...)
```

**Series A:**
```solidity
createSecurityToken("PE Fund Series A", "PEFA", 18, 25000000 * 10**18, seriesAInit)
initialSetup(..., [hasDIDRule, institutionalRule], ...)
```

**Series B:**
```solidity
createSecurityToken("PE Fund Series B", "PEFB", 18, 50000000 * 10**18, seriesBInit)
initialSetup(..., [hasDIDRule, publicRule], ...)
```

**Ongoing Management:**
- Update all rounds with new governance facet
- Apply uniform compliance updates
- Coordinate cross-round operations
- Manage token migrations

### 3. Commodity-Backed Token Deployment

**Scenario:** Gold tokenization platform

**Blueprint Design:**
- Physical asset verification facet
- Redemption mechanism facet
- Audit trail facet
- Storage cost distribution facet

**Token Creation:**
```solidity
createSecurityToken(
    "Stobox Gold Token",
    "STBXAU",
    6,
    type(uint256).max,  // No max supply (mint as gold added)
    goldInit
)
```

**Infrastructure Setup:**
```solidity
initialSetup(
    goldTokenAddress,
    vaultTreasuryAddress,
    vaultOperator,
    securityTeam,
    [hasDIDRule, kycRule, jurisdictionRule]
)
```

**Operational Features:**
- Issue tokens when gold deposited
- Redeem tokens when gold withdrawn
- Update redemption rules dynamically
- Link additional vaults (trust new treasuries)

### 4. Multi-Asset Platform Deployment

**Scenario:** Platform offering diverse tokenized assets

**Multiple Blueprints:**
- "Real Estate Blueprint" - Property tokens
- "Equity Blueprint" - Company shares
- "Debt Blueprint" - Bond-like instruments
- "Commodity Blueprint" - Physical assets
- "IP Blueprint" - Intellectual property

**Deployment Strategy:**
- Create blueprint for each asset class
- Deploy tokens using appropriate blueprint
- Standardize compliance within asset class
- Customize per asset requirements

**Management Benefits:**
- Asset class-specific upgrades
- Shared compliance rules
- Cross-asset operations
- Unified platform experience

### 5. Regulatory Compliance Updates

**Scenario:** New regulation requires additional compliance checks

**Update Process:**

1. **Deploy New Validation Rule:**
   ```solidity
   // Deploy new compliance rule contract
   NewRegulationRule rule = new NewRegulationRule()
   ```

2. **Link to All Existing Tokens:**
   ```solidity
   // For each token in generalTokenList()
   linkRuleForSTV3Token(tokenAddress, address(rule))
   ```

3. **Verify Compliance:**
   - All tokens now enforce new rule
   - Centralized update process
   - Consistent compliance across platform

**Benefits:**
- Rapid regulatory response
- Uniform rule application
- Centralized compliance management
- Audit trail of changes

### 6. Token Upgrade Rollout

**Scenario:** Security patch needed across all tokens

**Upgrade Process:**

1. **Deploy Fixed Facet:**
   ```solidity
   ValidationFacetV2 newFacet = new ValidationFacetV2()
   ```

2. **Update All Tokens:**
   ```solidity
   // For each token
   updateFacetsOfSTV3Token(
       tokenAddress,
       [FacetCut({
           facetAddress: address(newFacet),
           action: FacetCutAction.Replace,
           functionSelectors: selectors
       })],
       address(0),
       ""
   )
   ```

3. **Verify Upgrades:**
   - Check each token updated successfully
   - Monitor for issues
   - Rollback if necessary

**Advantages:**
- Centralized upgrade coordination
- Consistent patch application
- Minimal downtime
- State preservation

---

## Benefits

### For Asset Issuers

**Rapid Deployment:**
- Minutes to deploy production-ready token
- Pre-configured compliance and security
- Automated infrastructure setup
- No smart contract expertise required

**Standardization:**
- Consistent token structure
- Proven facet combinations
- Battle-tested blueprints
- Reduced audit costs

**Flexibility:**
- Choose appropriate blueprint
- Customize per asset requirements
- Add custom facets as needed
- Post-deployment modifications

**Cost Efficiency:**
- Shared facet deployments
- Reduced deployment costs
- Lower audit overhead
- Economies of scale

**Ongoing Management:**
- Centralized control interface
- Multi-token operations
- Rapid compliance updates
- Coordinated upgrades

### For Platform Operators

**Scalability:**
- Deploy unlimited tokens
- Consistent architecture
- Centralized management
- Efficient operations

**Quality Control:**
- Approved facet packages
- Tested configurations
- Version management
- Audit trails

**Operational Efficiency:**
- Single management interface
- Batch operations support
- Automated processes
- Reduced manual work

**Risk Management:**
- Emergency pause capability
- Rapid security responses
- Coordinated upgrades
- Rollback capability

### For Developers

**Integration:**
- Standard factory interface
- Consistent token structure
- Clear upgrade paths
- Comprehensive events

**Transparency:**
- Introspection capabilities
- Version tracking
- Deployment history
- Open source code

**Extensibility:**
- Custom facet support
- Blueprint creation
- Feature additions
- Protocol evolution

### For Investors

**Trust:**
- Standardized token infrastructure
- Proven security architecture
- Transparent deployment
- Professional management

**Consistency:**
- Predictable token behavior
- Uniform compliance
- Standard interfaces
- Reliable operations

**Protection:**
- Built-in compliance
- Emergency controls
- Upgrade capability
- Regulatory alignment

---

## Security Features

### Access Control Security

**Master Role Protection:**
- Single authority for factory
- Multi-signature wallet recommended
- Transparent role transitions
- Emergency master transfer

**Role-Based Evolution:**
- Migration from master-only to roles
- Granular permission management
- Separation of duties
- Audit trail via roles

**Factory Operation Security:**
- Master-only token creation
- Restricted blueprint management
- Protected infrastructure setup
- Controlled post-deployment management

### Blueprint Security

**Facet Validation:**
- Only master can register facets
- Version tracking per facet
- Package approval process
- Quality control gates

**Package Integrity:**
- Immutable package composition
- Versioned facet references
- Transparent package contents
- Audit-friendly structure

**Deployment Verification:**
- Consistent blueprint application
- Verification before deployment
- Post-deployment validation
- Deployment tracking

### Token Deployment Security

**Initialization:**
- Automated setup process
- Prevents misconfiguration
- Validates parameters
- Atomic operations

**Role Assignment:**
- Correct role grants
- Prevents role confusion
- Multi-role verification
- Assignment logging

**Validation Rules:**
- Automatic rule linking
- Prevents compliance gaps
- Rule verification
- Update capability

### Upgrade Security

**Factory Upgrades:**
- Master-controlled diamondCut
- State preservation
- Rollback capability
- Upgrade verification

**Token Upgrades:**
- Admin-controlled modifications
- Facet validation
- State preservation
- Coordinated rollouts

**Version Control:**
- Semantic versioning
- Clear upgrade history
- Compatibility tracking
- Rollback information

### Emergency Controls

**Factory Pause:**
- Can implement emergency pause
- Prevents new deployments
- Protects existing tokens
- Resumable operations

**Token Pause:**
- Individual token pause via STV3TokenManagementFacet
- Emergency stop capability
- Selective operation halt
- Recoverable state

**Access Revocation:**
- Remove malicious addresses from trust lists
- Unlink compromised validation rules
- Emergency role revocations
- Rapid response capability

---

## Deployment Information

### Arbitrum One Mainnet

**Factory Contract:**
- **Address:** [`0x096D75d0501c3B1479FFe15569192CeC998223b4`](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#code)
- **Name:** StoboxRWAVaultFactory
- **Version:** 1.0.1
- **Chain ID:** 42161
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

**Deployed Facets (Arbitrum Mainnet):**

| Facet | Address | Version |
|-------|---------|---------|
| DiamondInit | `0x3d353efc33c380b739f351FC0d65BA9590aeE0d1` | 1.0.0 |
| DiamondCutFacet | `0x4F09Af316e4053aa2876424BF0C7E08996fd93e1` | 1.0.0 |
| DiamondLoupeFacet | `0x9051Da5380980FCF7753AEe624Bc5f74Eb2f81BB` | 1.0.0 |
| TokenBlueprintFacet | `0x5456e10Ccb99B3Ff98dA3238839127ab839ecEf4` | 1.0.0 |
| CreateSTV3Facet | `0x1809B6422F730eD4ED8C8563C753Cd317639B4B6` | 1.0.0 |
| SetInfrastructureFacet | `0xFA62406a5ca68f1CaCDCE69ba71b4C5dd2705eD1` | 1.0.0 |

**View on Arbiscan:**
- [Contract Code](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#code)
- [Read Contract](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#readContract)
- [Write Contract](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#writeContract)

**Shadow Factory (Testing):**
- **Address:** `0x0CAa5870BAEEe0960eD007d91a87B92D30ee5d74`
- **Name:** ShadowFactory
- **Version:** 1.0.2

### Arbitrum Sepolia Testnet

**Factory Contract:**
- **Address:** `0xa1F864848e821D43bD8dDA123ffD47E92c558698`
- **Name:** Factory
- **Chain ID:** 421614
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

**Deployed Facets (Arbitrum Sepolia):**

| Facet | Address | Version |
|-------|---------|---------|
| DiamondInit | `0xfd358b0d233d975a7DD1Cc686414A8c10aC00d66` | 1.0.0 |
| DiamondCutFacet | `0x5342679e7f6A90aDBB5Ea9bdE16277D1298A2CD7` | 1.0.1 |
| DiamondLoupeFacet | `0xc79468A8dBADe0B8201bef63982F77fb909E8e92` | 1.0.0 |
| TokenBlueprintFacet | `0x32D15396f70E12287CDDB40E9Ef315113b6420E5` | 1.0.1 |
| CreateSTV3Facet | `0xf2cC491EC6451075fCB98310c49C02E93A171De9` | 1.0.1 |
| SetInfrastructureFacet | `0x45bBB1C1C433b294140c8F3C133c72ab71E48611` | 1.0.1 |
| FactoryRolesFacet | `0x702Aae01bbEefAb1F968C30AA25BbAc20601D921` | 1.0.0 |
| STV3TokenManagementFacet | `0x84026f3621797e0D5f22b42fFb12b775Af5F30BA` | 1.0.1 |

**View on Sepolia Arbiscan:**
- [Contract Code](https://sepolia.arbiscan.io/address/0xa1F864848e821D43bD8dDA123ffD47E92c558698#code)

### Related Infrastructure

**Tokens Created by Factory:**
- All StoboxProtocolSTV3 tokens deployed via factory
- Query via `generalTokenList()` function

**Example Deployment:**
- **StoboxProtocolSTV3:** [`0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46`](https://arbiscan.io/address/0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46#code)
- **STV3Treasury:** [`0x17b94f6b0ddeaac3753b33ee006a7fdbc1292dcf`](https://arbiscan.io/address/0x17b94f6b0ddeaac3753b33ee006a7fdbc1292dcf#code)

**Validation Rules:**
- **HasDIDRule:** [`0x85a3eae3cd8cecac03d2fc44001e030621320acb`](https://arbiscan.io/address/0x85a3eae3cd8cecac03d2fc44001e030621320acb#code)

**DID Registry:**
- **StoboxDID:** [`0x25E6036178656b1329ee51696407b367D8C6ba84`](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#code)

**Offering Registry:**
- **StoboxRWAOfferingRegistry:** [`0xb05cf4f652eb5475c3e4246ab976785678a1806a`](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code)

---

## Integration Guide

### For Asset Issuers

**Deploying Your First Token:**

```solidity
// 1. Create token via factory
(address token, address treasury) = factory.createSecurityToken(
    "My Asset Token",
    "MAT",
    6,                      // decimals
    1000000 * 10**6,        // max supply
    ""                      // custom init data
);

// 2. Setup infrastructure
factory.initialSetup(
    token,
    treasury,
    financialOperatorAddress,
    recoveryOperatorAddress,
    [hasDIDRuleAddress, customRuleAddress]
);

// 3. Token is ready for offering registration
```

**Managing Deployed Token:**

```solidity
// Pause token if needed
factory.pauseSTV3Token(tokenAddress);

// Link additional validation rule
factory.linkRuleForSTV3Token(tokenAddress, newRuleAddress);

// Set issuance limit
factory.setMaxIssuanceForSTV3Token(tokenAddress, 50000000 * 10**6);

// Resume token
factory.unpauseSTV3Token(tokenAddress);
```

### For Platform Operators

**Creating Custom Blueprint:**

```solidity
// 1. Deploy custom facets
CustomFacet custom = new CustomFacet();

// 2. Register facet
factory.addTokenFacet(address(custom), "CustomFacet", "1.0.0");

// 3. Create package
address[] memory facets = new address[](7);
facets[0] = diamondCutFacetAddress;
facets[1] = diamondLoupeFacetAddress;
facets[2] = validationFacetAddress;
facets[3] = rolesFacetAddress;
facets[4] = monetaryFacetAddress;
facets[5] = emergencyFacetAddress;
facets[6] = address(custom);

factory.addPackage("Custom Asset Package", facets);
```

**Querying Factory State:**

```solidity
// Get total tokens created
uint256 count = factory.tokenCount();

// Get all tokens
address[] memory tokens = factory.generalTokenList();

// Get token details
(
    string memory name,
    string memory symbol,
    address treasury,
    uint256 created,
    uint256 major,
    uint256 minor,
    uint256 patch
) = factory.tokenCreationDetails(tokenAddress);

// Get package info
(
    string[] memory names,
    address[] memory addresses,
    string[] memory versions
) = factory.fullPackageInfo("Standard Package");
```

### For Developers

**Factory Events to Monitor:**

```solidity
// Listen for new token deployments
event TokenCreated(
    address indexed tokenAddress,
    address indexed treasuryAddress,
    string name,
    string symbol
);

// Monitor infrastructure setup
event InfrastructureSetup(
    address indexed tokenAddress,
    address treasury,
    address financialOperator,
    address recoveryOperator
);

// Track version updates
event VersionUpdated(
    uint256 indexed major,
    uint256 indexed minor,
    uint256 indexed patch
);
```

**Integration Pattern:**

```solidity
interface IStoboxRWAVaultFactory {
    function createSecurityToken(
        string calldata name,
        string calldata symbol,
        uint8 decimals,
        uint256 maxSupply,
        bytes calldata initCalldata
    ) external returns (address token, address treasury);
    
    function initialSetup(
        address tokenAddress,
        address treasury,
        address financialOperator,
        address recoveryOperator,
        address[] calldata validationRules
    ) external;
    
    function generalTokenList() external view returns (address[] memory);
}
```

---

## API Reference

### Core Contract Functions

**Administration:**
```solidity
function setMaster(address newMaster) external onlyMaster;

function setFactoryVersion(uint256 major, uint256 minor, uint256 patch) external onlyMaster;

function master() external view returns (address);

function getVersion() external view returns (uint256 major, uint256 minor, uint256 patch);
```

### TokenBlueprintFacet

**Facet Management:**
```solidity
function addTokenFacet(
    address facetAddress,
    string calldata facetName,
    string calldata facetVersion
) external; // master only

function tokenFacetDetails(address facetAddress) external view 
    returns (string memory name, string memory version);

function tokenFacetExists(address facetAddress) external view returns (bool);
```

**Package Management:**
```solidity
function addPackage(
    string calldata packageName,
    address[] calldata facetsToAdd
) external; // master only

function packageDetails(string calldata packageName) external view 
    returns (address[] memory);

function fullPackageInfo(string calldata packageName) external view 
    returns (
        string[] memory facetNames,
        address[] memory facetAddresses,
        string[] memory facetVersions
    );

function packageCount() external view returns (uint256);
```

### CreateSTV3Facet

**Token Creation:**
```solidity
function createSecurityToken(
    string calldata name,
    string calldata symbol,
    uint8 decimals,
    uint256 maxSupply,
    bytes calldata initCalldata
) external returns (address token, address treasury); // master only
```

**Configuration:**
```solidity
function setTokenVersionToCreate(
    uint256 major,
    uint256 minor,
    uint256 patch
) external; // master only

function setInitializer(address newInitializer) external; // master only
```

**Queries:**
```solidity
function tokenCreationDetails(address tokenAddress) external view returns (
    string memory name,
    string memory symbol,
    address treasury,
    uint256 createdAt,
    uint256 major,
    uint256 minor,
    uint256 patch
);

function tokenVersionToCreate() external view returns (
    uint256 major,
    uint256 minor,
    uint256 patch
);

function tokenInitializer() external view returns (address);

function tokenCount() external view returns (uint256);

function generalTokenList() external view returns (address[] memory);
```

### SetInfrastructureFacet

**Setup:**
```solidity
function initialSetup(
    address tokenAddress,
    address treasury,
    address financialOperator,
    address recoveryOperator,
    address[] calldata validationRules
) external; // master only
```

### STV3TokenManagementFacet

**Facet Updates:**
```solidity
function updateFacetsOfSTV3Token(
    address tokenAddress,
    FacetCut[] calldata _diamondCut,
    address _init,
    bytes calldata _calldata
) external; // DEFAULT_ADMIN_ROLE
```

**Pause Controls:**
```solidity
function pauseSTV3Token(address tokenAddress) external; // DEFAULT_ADMIN_ROLE

function unpauseSTV3Token(address tokenAddress) external; // DEFAULT_ADMIN_ROLE
```

**Trust Management:**
```solidity
function trustForSTV3Token(address tokenAddress, address account) external; // DEFAULT_ADMIN_ROLE

function distrustForSTV3Token(address tokenAddress, address account) external; // DEFAULT_ADMIN_ROLE
```

**Validation Rules:**
```solidity
function linkRuleForSTV3Token(address tokenAddress, address ruleAddress) external; // DEFAULT_ADMIN_ROLE

function unlinkRuleForSTV3Token(address tokenAddress, address ruleAddress) external; // DEFAULT_ADMIN_ROLE
```

**Monetary Controls:**
```solidity
function setMaxIssuanceForSTV3Token(address tokenAddress, uint256 newMaxIssuance) external; // DEFAULT_ADMIN_ROLE
```

### DiamondCutFacet

**Upgrade:**
```solidity
function diamondCut(
    FacetCut[] calldata _diamondCut,
    address _init,
    bytes calldata _calldata
) external; // DEFAULT_ADMIN_ROLE (v1.0.1)
```

### DiamondLoupeFacet

**Introspection:**
```solidity
function facets() external view returns (Facet[] memory);

function facetFunctionSelectors(address _facet) external view returns (bytes4[] memory);

function facetAddresses() external view returns (address[] memory);

function facetAddress(bytes4 _functionSelector) external view returns (address);

function supportsInterface(bytes4 interfaceId) external view returns (bool);
```

### FactoryRolesFacet

**Role Management:**
```solidity
function grantRole(bytes32 role, address account) external;

function revokeRole(bytes32 role, address account) external;

function renounceRole(bytes32 role, address account) external;

function hasRole(bytes32 role, address account) external view returns (bool);

function getRoleAdmin(bytes32 role) external view returns (bytes32);

function getRoleMember(bytes32 role, uint256 index) external view returns (address);

function getRoleMemberCount(bytes32 role) external view returns (uint256);

function DEFAULT_ADMIN_ROLE() external pure returns (bytes32);
```

---

## Roadmap & Future Enhancements

### Planned Features

**Enhanced Blueprint System**
- Visual blueprint builder UI
- Template marketplace for common asset types
- Community-contributed blueprints
- Automated blueprint testing framework

**Advanced Token Management**
- Batch operations across multiple tokens
- Scheduled upgrades and maintenance
- Automated compliance rule updates
- Token analytics and monitoring dashboard

**Multi-Chain Support**
- Cross-chain factory deployment
- Unified token management across chains
- Bridge integration for token portability
- Multi-chain blueprint packages

**Governance Integration**
- DAO-controlled blueprint approval
- Community governance for upgrades
- Proposal and voting mechanisms
- Decentralized factory operations

**Automation & DevOps**
- CI/CD integration for deployments
- Automated testing pipelines
- Deployment scripts and templates
- Monitoring and alerting tools

**Compliance Tools**
- Regulatory compliance packages by jurisdiction
- Automated compliance reporting
- Integration with regulator APIs
- Compliance dashboard and audit tools

**Developer Experience**
- SDK for factory integration
- CLI tools for operations
- GraphQL API for queries
- Comprehensive documentation

**Enterprise Features**
- Private factory deployments
- Custom branding and white-labeling
- SLA guarantees
- Dedicated support

### Version Roadmap

**v1.1.0** (Planned)
- Enhanced STV3TokenManagementFacet
- Batch operation support
- Additional blueprint templates
- Gas optimizations

**v1.2.0** (Future)
- Governance integration
- Multi-chain support
- Advanced monitoring

**v2.0.0** (Long-term)
- Major architectural enhancements
- New feature categories
- Breaking changes with migration path

---

## Documentation & Resources

### Technical Documentation
- [GitHub Repository](https://github.com/StoboxTechnologies/Stobox-WEB3-Infrastructure)
- [Release Notes](./StoboxRWAVaultFactory-Release-Notes.md)
- [Infrastructure Overview](./README.md)

### Smart Contract Verification
- [Mainnet Factory on Arbiscan](https://arbiscan.io/address/0x096d75d0501c3b1479ffe15569192cec998223b4#code)
- [Testnet Factory on Sepolia Arbiscan](https://sepolia.arbiscan.io/address/0xa1F864848e821D43bD8dDA123ffD47E92c558698#code)

### Related Documentation
- [STV3 Product Summary](./STV3-Product-Summary.md)
- [StoboxDID Product Summary](./StoboxDID-Product-Summary.md)
- [Token Offering Module Summary](./Token-Offering-Module-Summary.md)
- [StoboxProtocolSTV3 Release Notes](./StoboxProtocolSTV3-Release-Notes.md)
- [Stobox Docs V4](https://docs.stobox.io)

### Educational Resources
- Diamond Standard (EIP-2535) specification
- Factory pattern in smart contracts
- Token deployment best practices
- Compliance automation guides

---

## Support & Contact

For integration support, technical questions, or partnership inquiries:

- **GitHub Repository:** [Stobox-WEB3-Infrastructure](https://github.com/StoboxTechnologies/Stobox-WEB3-Infrastructure)
- **Documentation:** [Stobox Docs](https://docs.stobox.io)
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

---

## License

This project is licensed under the terms specified in the [LICENSE](./LICENSE) file.

---

## Summary

StoboxRWAVaultFactory provides the **deployment foundation** for the Stobox RWA ecosystem:

✅ **Standardized** - Consistent token deployment process  
✅ **Modular** - Diamond Standard architecture  
✅ **Flexible** - Blueprint system for diverse assets  
✅ **Automated** - One-transaction setup and configuration  
✅ **Manageable** - Centralized lifecycle management  
✅ **Upgradeable** - Seamless protocol evolution  
✅ **Scalable** - Unlimited token deployments  
✅ **Secure** - Role-based access control  
✅ **Transparent** - Complete deployment history  
✅ **Enterprise-Ready** - Production-grade infrastructure  

---

**Last Updated:** December 2025  
**Current Version:** 1.0.1  
**Status:** Production (Arbitrum One Mainnet)

