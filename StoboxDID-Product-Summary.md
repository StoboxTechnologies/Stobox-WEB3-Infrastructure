# StoboxDID - Product Summary

## Overview

**StoboxDID** (Stobox Decentralized Identity) is a comprehensive blockchain-based identity management system that enables users to create, manage, and update digital identities on-chain. As real-world asset (RWA) tokenization accelerates, the demand for robust compliance and privacy-centric identity verification grows exponentially. The Stobox DID framework directly addresses these challenges by providing secure, flexible, and regulatory-compliant identity infrastructure for the tokenized economy.

**Network:** Arbitrum Mainnet  
**Contract Address:** [`0x25E6036178656b1329ee51696407b367D8C6ba84`](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#code)  
**Name:** StoboxDID  
**Chain ID:** 42161 (Arbitrum Mainnet)  
**Repository:** [ST4DIDSC](https://github.com/StoboxTechnologies/ST4DIDSC)  
**Documentation:** [Stobox Docs V4](https://docs.stobox.io)

---

## Product Vision

StoboxDID serves as the **identity backbone** for the entire Stobox RWA ecosystem, providing:

- **Universal Identity** - Single decentralized identifier (UDID) across all Stobox protocols
- **Privacy-Centric** - User-controlled data sharing with time-limited access grants
- **Multi-Address Support** - Link multiple wallet addresses to one verified identity
- **Flexible Attributes** - Store KYC/AML data, accreditation status, jurisdiction info, and custom attributes
- **Regulatory Compliance** - Built-in controls for identity verification, blocking, and attribute expiration
- **Controlled Access** - Role-based permissions with external reader system for third-party integrations

StoboxDID enables compliant tokenized asset transactions by ensuring all participants have verified, blockchain-anchored identities that can be validated in real-time without exposing sensitive personal data.

---

## Key Features

### 1. Decentralized Identity Management

Complete lifecycle management for blockchain-based identities:

**DID Creation:**
- Unique Decentralized Identifier (UDID) per user
- Validity period with expiration dates
- Creator tracking and audit trails
- Immutable creation records

**DID Lifecycle:**
- `createDID()` - Register new decentralized identifier
- `prolongateDID()` - Extend validity period
- `blockDID()` - Suspend identity (with reason)
- `unBlockDID()` - Restore suspended identity

**Identity Properties:**
- UDID (string) - Unique identifier
- validTo (timestamp) - Expiration date
- blocked (bool) - Suspension status
- updatedAt (timestamp) - Last modification
- lastUpdatedBy (address) - Audit trail

**Use Cases:**
- KYC/AML verification for token transfers
- Investor accreditation tracking
- Regulatory compliance across offerings
- Long-term identity persistence

### 2. Multi-Address Linking

Support for multiple wallet addresses under single identity:

**Address Management:**
- Link unlimited addresses to one UDID (configurable maximum)
- Track join date and update history for each address
- Deactivate addresses without deletion (preserves history)
- Reactivate previously deactivated addresses
- Remove addresses completely when needed

**Linker Structure:**
- UDID - Associated identity
- joinDate - When address was linked
- updateDate - Last modification timestamp
- deactivated - Active/inactive status
- linkedAddresses - All addresses for this UDID

**Benefits:**
- Use same identity across multiple wallets
- Hardware wallet + hot wallet combinations
- Institutional multi-signature wallets
- Cross-platform identity consistency
- Identity preservation during wallet changes

**Operations:**
- `linkAddressToDID()` - Connect new address to DID
- `deactivateAddressOfDID()` - Temporarily disable address
- `activateAddressOfDID()` - Re-enable deactivated address
- `removeLinkedAddress()` - Permanently remove address

### 3. Flexible Attribute System

Store and manage diverse identity attributes with validation periods:

**Attribute Structure:**
- value (bytes) - Flexible data format (any type)
- valueType (string) - Human-readable type description
- createdAt (timestamp) - Creation date
- updatedAt (timestamp) - Last modification
- validTo (timestamp) - Expiration date
- lastUpdatedBy (address) - Who modified it

**Attribute Types Examples:**
- **KYC Data** - Name, date of birth, nationality (hashed)
- **Accreditation** - Investor status, income verification
- **Jurisdiction** - Country of residence, tax status
- **Compliance** - AML checks, sanctions screening
- **Custom** - Asset-specific requirements

**Attribute Management:**
- `addOrUpdateAttributes()` - Batch add/update attributes
- `deactivateDIDAttribute()` - Disable attribute (preserves history)
- Automatic expiration based on validTo timestamp
- Type flexibility (STRING, UINT, HASH, JSON, etc.)

**Data Format Flexibility:**
- Store as bytes for maximum versatility
- Support hashed PII for privacy
- JSON for structured data
- Numeric values for thresholds
- Encrypted data for sensitive info

**Validation:**
- Time-based validity (validTo field)
- Deactivation without deletion
- Audit trail via lastUpdatedBy
- History preservation

### 4. Role-Based Access Control

Granular permission system with specialized roles:

**DEFAULT_ADMIN_ROLE:**
- Supreme authority over DID system
- Grant/revoke any role
- Modify role hierarchies
- Set system-wide parameters
- Automatically assigned to deployer

**WRITER_ROLE:**
Essential operational role for DID management:
- **DID Lifecycle** - Create, prolong, block, unblock DIDs
- **Address Linking** - Link, remove, activate, deactivate addresses
- **Attribute Management** - Add, update, deactivate attributes
- **External Readers** - Add, update, remove external reader access

**ATTRIBUTE_READER_ROLE:**
Read-only access for authorized services:
- View full DID data including attributes
- Read attribute lists
- Access linked addresses
- No modification rights

**DID Owner:**
Any address linked to a DID becomes an owner:
- Link additional addresses to own DID
- Manage external reader access for own DID
- Deactivate/activate addresses on own DID
- Self-service identity management

**Access Control Features:**
- OpenZeppelin AccessControl standard
- Enumerable roles for transparency
- Multi-role assignments possible
- Role admin hierarchies

### 5. External Reader System

Time-limited, controlled data access for third parties:

**Reader Capabilities:**
- Temporary read access to specific DID data
- Expiration-based access control
- No modification rights
- Revocable access grants

**Access Levels:**
- `readFullDID()` - Complete DID structure
- `readAttributeList()` - All attribute keys
- `readLinkedAddresses()` - All linked addresses

**Management:**
- `addOrUpdateExternalReader()` - Grant/extend access (DID owner or WRITER)
- `deleteExternalReader()` - Revoke access (DID owner or WRITER)
- `externalReaderExpirationDate()` - Check access expiration
- `canRead()` - Verify read permissions

**Use Cases:**
- Compliance service providers access to KYC data
- Offering registry reading investor attributes
- Third-party verification services
- Temporary auditor access
- Partner integrations

**Security:**
- Time-limited access (expiration timestamps)
- Owner-controlled grants
- Transparent access logging via events
- Automatic expiration enforcement

### 6. Privacy & Data Control

User-centric privacy with controlled data sharing:

**Privacy Features:**
- Store only necessary data on-chain
- Hash sensitive PII before storage
- Attribute-level access control
- External reader time limits
- User-controlled data sharing

**Data Minimization:**
- Flexible attribute types (store references, not raw data)
- Off-chain data storage with on-chain proofs
- Selective disclosure capabilities
- Attribute deactivation (soft delete)

**Access Transparency:**
- Events for all data access (`AttributeListWasRead`, `FullDIDWasRead`)
- Track who reads DID data
- Audit trail for compliance
- User awareness of data access

**GDPR Considerations:**
- Right to be forgotten (address deactivation)
- Data portability (read full DID)
- Purpose limitation (external reader expiration)
- Access transparency (event logs)

### 7. Compliance & Regulatory Features

Built-in mechanisms for regulatory compliance:

**Identity Verification:**
- Immutable DID creation records
- Validity period management
- Block/unblock for regulatory actions
- Reason tracking for blocks

**Attribute Expiration:**
- Time-limited KYC data
- Automatic expiration of outdated info
- Re-verification workflows
- Compliance with data retention policies

**Blocking Mechanism:**
- `blockDID()` - Suspend identity with reason
- `unBlockDID()` - Restore identity
- Prevents token transfers when blocked
- Regulatory enforcement capability

**Audit Trail:**
- All modifications logged via events
- Creator and updater tracking
- Timestamp all changes
- Immutable history on-chain

**Maximum Address Limits:**
- `setMaxLinkedAddresses()` - Configurable per deployment
- Prevents identity farming
- Regulatory compliance (entity limits)
- Sybil attack prevention

### 8. Comprehensive Event System

Transparent activity logging for all operations:

**DID Lifecycle Events:**
- `DIDCreated` - New identity registered
- `DIDValidToDateUpdated` - Validity extended/modified
- `DIDBlockStatusUpdated` - Identity blocked/unblocked

**Address Linking Events:**
- `DIDAddressLinked` - New address connected
- `DIDAddressDeleted` - Address permanently removed
- `DIDAddressDeactivated` - Address temporarily disabled
- `DIDAddressActivated` - Address re-enabled

**Attribute Events:**
- `AttributeCreated` - New attribute added
- `AttributeUpdated` - Attribute modified
- `AttributeDeactivated` - Attribute disabled
- `AttributeValidToDateUpdated` - Expiration changed

**Access Events:**
- `NewExternalReaderAdded` - Read access granted
- `ExternalReaderUpdated` - Access extended/modified
- `ExternalReaderDeleted` - Access revoked
- `AttributeListWasRead` - Attributes queried
- `LinkedAddressesListWasRead` - Addresses queried
- `FullDIDWasRead` - Complete DID data accessed

**Debug Events:**
- `UnexpectedBehavior` - System inconsistencies detected

**Benefits:**
- Complete audit trail
- Real-time monitoring
- Integration with analytics tools
- Compliance reporting
- Debugging and diagnostics

---

## Architecture

### Smart Contract Structure

**Data Structures:**

**DID Structure:**
```solidity
struct DID {
    string UDID;                                    // Unique identifier
    uint256 validTo;                                // Expiration timestamp
    uint256 updatedAt;                              // Last update
    bool blocked;                                   // Block status
    address lastUpdatedBy;                          // Audit trail
    string[] attributeList;                         // Attribute keys
    mapping(string => Attribute) attributes;        // Attribute data
    mapping(address => uint256) externalReader;     // Reader access
}
```

**Attribute Structure:**
```solidity
struct Attribute {
    bytes value;              // Flexible data format
    string valueType;         // Type description
    uint256 createdAt;       // Creation timestamp
    uint256 updatedAt;       // Last update
    uint256 validTo;         // Expiration
    address lastUpdatedBy;   // Who modified
}
```

**Linker Structure:**
```solidity
struct Linker {
    string UDID;                    // Associated identity
    uint256 joinDate;               // Link creation date
    uint256 updateDate;             // Last modification
    bool deactivated;               // Active status
    address[] linkedAddresses;      // All linked addresses
}
```

**ParamAttribute Structure:**
```solidity
struct ParamAttribute {
    address walletAddress;      // Target address
    string attributeName;       // Attribute key
    bytes value;                // Attribute value
    string valueType;           // Type descriptor
    uint256 validToData;        // Expiration
}
```

### Access Control Architecture

**Four-Tier Permission System:**

1. **DEFAULT_ADMIN_ROLE** - System administration
2. **WRITER_ROLE** - DID management operations
3. **ATTRIBUTE_READER_ROLE** - Read-only access
4. **DID Owner** - Self-service management
5. **External Reader** - Time-limited third-party access

**Permission Matrix:**

| Operation | Admin | Writer | Reader | Owner | External |
|-----------|-------|--------|--------|-------|----------|
| Create DID | ✓ | ✓ | ✗ | ✗ | ✗ |
| Block/Unblock DID | ✓ | ✓ | ✗ | ✗ | ✗ |
| Link Address | ✓ | ✓ | ✗ | ✓ (own) | ✗ |
| Add Attributes | ✓ | ✓ | ✗ | ✗ | ✗ |
| Read Full DID | ✓ | ✓ | ✓ | ✓ (own) | ✓ (if granted) |
| Grant External Access | ✓ | ✓ | ✗ | ✓ (own) | ✗ |
| Set Max Addresses | ✓ | ✗ | ✗ | ✗ | ✗ |

### Technology Stack

- **Blockchain:** Arbitrum One (Ethereum Layer 2)
- **Solidity Version:** Latest stable
- **Access Control:** OpenZeppelin AccessControl + AccessControlEnumerable
- **Storage Pattern:** Optimized struct-based storage
- **Gas Optimization:** Batch operations for attributes

---

## Ecosystem Integration

StoboxDID integrates deeply with the entire Stobox RWA infrastructure:

### 1. StoboxProtocolSTV3 Integration

**Validation Rules:**
- HasDIDRule validates token holders have active DIDs
- ValidationFacet queries StoboxDID before transfers
- Blocked DIDs prevent token transactions
- Attribute-based transfer restrictions

**Use Cases:**
- Pre-transfer identity verification
- Accreditation status checks
- Jurisdiction-based transfer rules
- Compliance automation

### 2. StoboxRWAOfferingRegistry Integration

**Investor Validation:**
- AccreditedUSMinInvestmentRule checks DID attributes
- Offering-specific eligibility rules query DID data
- Purchase validation against investor attributes
- Real-time compliance checks

**Workflow:**
1. Investor submits purchase request
2. Registry queries StoboxDID for investor attributes
3. Validate accreditation status, jurisdiction, etc.
4. Approve/reject based on DID data
5. Track purchase against DID (not just address)

### 3. StoboxRWAVaultFactory Integration

**Token Creation:**
- Factory links HasDIDRule to new tokens
- Initial setup trusts DID-validated addresses
- Configure DID-based validation rules
- Automated compliance setup

### 4. Third-Party Integrations

**KYC/AML Providers:**
- Write verified data to DID attributes
- Update accreditation status
- Set attribute expiration dates
- Revoke outdated verifications

**Compliance Services:**
- External reader access for compliance checks
- Real-time identity verification
- Sanctions screening results
- Ongoing monitoring integration

**Custodians & Exchanges:**
- Query DID for client verification
- Multi-address support for institutional wallets
- Attribute-based trading permissions
- Compliance reporting

---

## Use Cases

### 1. Security Token Compliance

**Scenario:** Real estate token with investor restrictions

**DID Setup:**
- Create DID for each investor
- Add KYC attributes (nationality, residency)
- Store accreditation status
- Set attribute expiration (annual re-verification)

**Validation Flow:**
1. Investor attempts token transfer
2. STV3 ValidationFacet checks HasDIDRule
3. Query StoboxDID for investor's DID
4. Verify DID is active (not blocked, not expired)
5. Check required attributes exist and are valid
6. Approve/reject transfer based on results

**Benefits:**
- Automated compliance checks
- Real-time validation
- No manual intervention
- Audit trail on-chain

### 2. Multi-Wallet Investor Management

**Scenario:** Institutional investor with multiple wallets

**DID Configuration:**
- Single UDID for institution
- Link cold wallet address
- Link hot wallet address
- Link employee wallet addresses
- All share same verified identity

**Operations:**
- KYC done once, applies to all wallets
- Token transfers from any linked address
- Centralized attribute management
- Simplified compliance tracking

**Advantages:**
- Reduced KYC overhead
- Flexible wallet management
- Consistent identity across wallets
- Easy wallet rotation

### 3. Cross-Border Offering Compliance

**Scenario:** Global STO with regional restrictions

**DID Attributes:**
- Country of residence
- Tax jurisdiction
- Accreditation status by region
- Investment limits by country

**Offering Rules:**
- Check residency attribute before purchase
- Validate accreditation for US investors
- Apply country-specific investment caps
- Block sanctioned jurisdictions

**Compliance:**
- Automated geographic restrictions
- Real-time eligibility checks
- Regulatory compliance by design
- Reduced legal risk

### 4. Ongoing KYC Refresh

**Scenario:** Annual re-verification requirement

**Attribute Management:**
- Set validTo = 365 days from KYC date
- Monitor approaching expirations
- Trigger re-verification workflows
- Update attributes upon re-verification

**Automation:**
- Smart contracts check attribute validity
- Expired attributes fail validation
- Token transfers blocked until refresh
- Automated compliance maintenance

### 5. Privacy-Preserving Verification

**Scenario:** Third-party verification without exposing PII

**External Reader Setup:**
- Grant time-limited access to compliance service
- Service reads attribute list
- Verify required attributes exist and are valid
- No access to actual PII values (stored off-chain or hashed)

**Privacy Benefits:**
- Minimal data exposure
- Purpose-limited access
- Time-limited grants
- User awareness via events

### 6. Regulatory Action Response

**Scenario:** Sanctions or legal order

**DID Blocking:**
- Regulatory authority requests block
- WRITER_ROLE executes `blockDID()` with reason
- All linked addresses immediately restricted
- Token transfers fail validation
- Unblock when situation resolved

**Advantages:**
- Immediate response capability
- Multi-address enforcement
- Transparent reason tracking
- Reversible when appropriate

---

## Benefits

### For Asset Issuers

**Compliance Automation:**
- Pre-verified investor identities
- Real-time eligibility checks
- Automated attribute validation
- Reduced manual compliance work

**Risk Management:**
- Identity blocking for emergencies
- Attribute expiration enforcement
- Multi-address tracking per entity
- Regulatory action capability

**Operational Efficiency:**
- Single KYC for multiple offerings
- Reusable investor identities
- Batch attribute updates
- Centralized identity management

**Cost Reduction:**
- No repeated KYC per offering
- Automated compliance checks
- Reduced legal overhead
- Lower operational costs

### For Investors

**Privacy Control:**
- User-controlled data sharing
- Time-limited access grants
- Minimal data exposure
- Transparent access logging

**Convenience:**
- Single identity across ecosystem
- Multi-wallet support
- One-time KYC process
- Seamless offering participation

**Portability:**
- Identity persists across offerings
- Transferable between platforms (if integrated)
- Long-term identity preservation
- Independent of specific issuer

**Trust:**
- Blockchain-verified identity
- Immutable audit trail
- Transparent operations
- Decentralized control

### For Compliance Providers

**Integration:**
- External reader API
- Standard data structures
- Event-based notifications
- Real-time data access

**Efficiency:**
- Write once, use everywhere
- Standardized attribute formats
- Batch operations support
- Automated expiration management

**Revenue Opportunities:**
- KYC-as-a-service
- Ongoing monitoring services
- Attribute verification
- Compliance reporting

### For Regulators

**Oversight:**
- Transparent identity system
- Complete audit trail
- Real-time monitoring capability
- Immutable compliance records

**Enforcement:**
- Identity blocking mechanism
- Reason tracking for actions
- Multi-address enforcement
- Reversible controls

**Reporting:**
- On-chain activity logs
- Identity verification records
- Attribute history
- Access transparency

---

## Security Features

### Access Control Security

**Role Separation:**
- Admin vs. operational roles
- Read-only vs. write permissions
- Owner vs. administrator separation
- Principle of least privilege

**Permission Validation:**
- All write operations check roles
- Owner verification for self-service
- External reader expiration checks
- Multi-signature support (admin role)

### Data Integrity

**Immutable Records:**
- DID creation permanently logged
- Modification history preserved
- Event logs immutable
- Audit trail cannot be altered

**Validation:**
- Address linking prevents duplicates
- Maximum address limits
- Attribute expiration enforcement
- Block status checks

### Privacy Protection

**Minimal Exposure:**
- Flexible data formats (store hashes)
- Off-chain storage for sensitive data
- Attribute-level access control
- Time-limited reader access

**User Control:**
- Owner can grant/revoke external access
- Deactivate addresses without deletion
- Control linked addresses
- Transparent access via events

### Attack Prevention

**Sybil Resistance:**
- Maximum linked addresses per DID
- WRITER_ROLE required for creation
- Address uniqueness enforcement
- Identity verification requirements

**Access Attack Mitigation:**
- Time-limited external reader access
- Permission checks on all operations
- Event logging for monitoring
- Automatic expiration enforcement

### Emergency Controls

**Identity Blocking:**
- Block compromised identities
- Reason tracking for transparency
- Reversible when resolved
- Multi-address effect

**Address Management:**
- Remove compromised addresses
- Deactivate suspicious addresses
- Link replacement addresses
- History preservation

---

## Custom Errors

Specific error messages for common failure scenarios:

**Administration Errors:**
- `CantRevokeLastSuperAdmin()` - Prevent system lockout
- `NotAuthorizedForThisTransaction(address)` - Permission denied

**DID Errors:**
- `DIDAlreadyExists(string UDID)` - Duplicate prevention
- `DIDDoesNotExist(string UDID)` - Invalid DID reference
- `DIDIsBlocked(string UDID)` - Operation on blocked identity
- `DIDIsNotBlocked(string UDID)` - Unblock inactive identity

**Address Errors:**
- `ZeroAddressNotAllowed()` - Invalid address
- `AddressAlreadyLinkedToDID(address, string)` - Duplicate linking
- `AddressDoesNotLinkedToDID(address)` - Invalid unlink
- `AddressDoesNotHaveLinker(address)` - Missing linker data
- `AddressAlreadyDeactivated(address)` - Already inactive
- `AddressAlreadyActivated(address)` - Already active
- `CantRemoveLastLinkedAddress()` - DID must have ≥1 address
- `MaxLinkedAddressesExceeded(string, uint256)` - Limit reached

**Validation Errors:**
- `ValidToDataMustBeInFuture(uint256)` - Invalid expiration
- `AddressIsNotExternalReader(string, address)` - Invalid reader

**Benefits:**
- Clear error messages
- Debugging assistance
- Integration guidance
- User-friendly failures

---

## Deployment Information

### Arbitrum Mainnet

**DID Contract:**
- **Address:** [`0x25E6036178656b1329ee51696407b367D8C6ba84`](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#code)
- **Name:** StoboxDID
- **Chain ID:** 42161
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

**View on Arbiscan:**
- [Contract Code](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#code)
- [Read Contract](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#readContract)
- [Write Contract](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#writeContract)

### Arbitrum Sepolia Testnet

**DID Contract:**
- **Address:** `0xA832662d1E11F2a6cEF706cE54A993E7eeEDC440`
- **Name:** SDID
- **Chain ID:** 421614
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

**View on Sepolia Arbiscan:**
- [Contract Code](https://sepolia.arbiscan.io/address/0xA832662d1E11F2a6cEF706cE54A993E7eeEDC440#code)

### Related Infrastructure

**Validation Rule (HasDIDRule):**
- **Address:** [`0x85a3eae3cd8cecac03d2fc44001e030621320acb`](https://arbiscan.io/address/0x85a3eae3cd8cecac03d2fc44001e030621320acb#code)

**StoboxProtocolSTV3:**
- **Address:** [`0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46`](https://arbiscan.io/address/0x998a0beaf37ca4ba61b5cfac59fdee0da2211a46#code)

**StoboxRWAOfferingRegistry:**
- **Address:** [`0xb05cf4f652eb5475c3e4246ab976785678a1806a`](https://arbiscan.io/address/0xb05cf4f652eb5475c3e4246ab976785678a1806a#code)

---

## Integration Guide

### For Developers

**Reading DID Data:**

```solidity
// Check if address has DID
(string memory udid, uint256 validTo, , bool blocked, ) = stoboxDID.getUserDID(investorAddress);

// Verify DID is active
require(bytes(udid).length > 0, "No DID found");
require(validTo > block.timestamp, "DID expired");
require(!blocked, "DID blocked");

// Read specific attribute
(bytes memory value, string memory valueType, , , uint256 attrValidTo, ) = 
    stoboxDID.getAttribute(investorAddress, "accreditation");

require(attrValidTo > block.timestamp, "Attribute expired");
```

**Writing DID Data (WRITER_ROLE):**

```solidity
// Create new DID
stoboxDID.createDID("UDID-12345", block.timestamp + 365 days);

// Link address to DID
stoboxDID.linkAddressToDID("UDID-12345", investorAddress);

// Add attributes
ParamAttribute[] memory attrs = new ParamAttribute[](1);
attrs[0] = ParamAttribute({
    walletAddress: investorAddress,
    attributeName: "accreditation",
    value: abi.encode(true),
    valueType: "BOOL",
    validToData: block.timestamp + 365 days
});
stoboxDID.addOrUpdateAttributes(attrs);
```

**External Reader Access:**

```solidity
// Grant time-limited access
stoboxDID.addOrUpdateExternalReader(
    "UDID-12345",
    complianceServiceAddress,
    block.timestamp + 30 days
);

// Check if can read
bool canRead = stoboxDID.canRead("UDID-12345", complianceServiceAddress);

// Read full DID (if authorized)
stoboxDID.readFullDID("UDID-12345");
```

### For KYC/AML Providers

**Integration Steps:**

1. **Get WRITER_ROLE** - Admin grants role to KYC provider
2. **Create DID** - Upon successful KYC verification
3. **Add Attributes** - Store verification results
4. **Set Expiration** - Based on re-verification schedule
5. **Monitor Events** - Track DID usage and updates

**Attribute Schema Recommendations:**

```solidity
// Standard attributes
"kyc_status" → "VERIFIED" | "PENDING" | "FAILED"
"kyc_date" → timestamp of verification
"kyc_provider" → provider identifier
"accreditation" → "ACCREDITED" | "NON_ACCREDITED"
"jurisdiction" → ISO country code
"aml_check" → "PASSED" | "FAILED"
"sanctions_check" → "CLEAR" | "FLAGGED"
```

### For Offering Issuers

**Validation Rule Creation:**

```solidity
contract CustomDIDRule {
    StoboxDID public did;
    
    function validate(address investor) external view returns (bool) {
        // Get DID
        (string memory udid, uint256 validTo, , bool blocked, ) = did.getUserDID(investor);
        
        // Check DID active
        if (bytes(udid).length == 0 || validTo < block.timestamp || blocked) {
            return false;
        }
        
        // Check required attributes
        (bytes memory accred, , , , uint256 accredValidTo, ) = 
            did.getAttribute(investor, "accreditation");
        
        if (accredValidTo < block.timestamp) {
            return false;
        }
        
        // Custom logic
        bool isAccredited = abi.decode(accred, (bool));
        return isAccredited;
    }
}
```

---

## API Reference

### View Methods (Public)

**DID Information:**
```solidity
function getUserDID(address walletAddress) external view 
    returns (string UDID, uint256 validTo, uint256 updatedAt, bool blocked, address lastUpdatedBy);

function getLinker(address walletAddress) external view 
    returns (string uDID, uint256 joinDate, uint256 updateDate, bool deactivated);
```

**Attribute Information:**
```solidity
function getAttribute(address walletAddress, string attributeName) external view 
    returns (bytes value, string valueType, uint256 createdAt, uint256 updatedAt, 
            uint256 validTo, address lastUpdatedBy);
```

**Access Verification:**
```solidity
function canRead(string uDIDToRead, address addressCanRead) external view returns (bool);

function externalReaderExpirationDate(string uDID, address walletAddress) external view 
    returns (uint256);
```

### Write Methods (Role-Protected)

**DID Lifecycle (WRITER_ROLE):**
```solidity
function createDID(string calldata uDID, uint256 validToData) external;

function prolongateDID(string calldata uDID, uint256 newValidToData) external;

function blockDID(string calldata uDID, string calldata reason) external;

function unBlockDID(string calldata uDID, string calldata reason) external;
```

**Address Management (WRITER_ROLE or Owner):**
```solidity
function linkAddressToDID(string calldata uDID, address addressToLink) external;

function removeLinkedAddress(string calldata uDID, address addressToDelete) external; // WRITER only

function deactivateAddressOfDID(string calldata uDID, address addressToDeactivate) external;

function activateAddressOfDID(string calldata uDID, address addressToActivate) external;
```

**Attribute Management (WRITER_ROLE):**
```solidity
function addOrUpdateAttributes(ParamAttribute[] calldata paramAttributes) external;

function deactivateDIDAttribute(string calldata uDID, string calldata attributeName) external;
```

**External Reader Management (WRITER_ROLE or Owner):**
```solidity
function addOrUpdateExternalReader(string calldata uDID, address reader, uint256 expirationData) external;

function deleteExternalReader(string calldata uDID, address reader) external;
```

**Configuration (DEFAULT_ADMIN_ROLE):**
```solidity
function setMaxLinkedAddresses(uint256 newMax) external;
```

### Access-Controlled Read Methods

**Require ATTRIBUTE_READER_ROLE, DID Owner, or valid External Reader:**
```solidity
function readFullDID(string calldata uDID) external returns (...);

function readAttributeList(string calldata uDID) external returns (string[] memory);

function readLinkedAddresses(string calldata uDID) external returns (address[] memory);
```

---

## Roadmap & Future Enhancements

### Planned Features

**Enhanced Privacy**
- Zero-knowledge proof integration
- Selective disclosure mechanisms
- Encrypted attribute storage
- Privacy-preserving attribute verification

**Advanced Attribute Management**
- Attribute schemas and templates
- Hierarchical attributes
- Composite attributes
- Attribute inheritance

**Cross-Chain Identity**
- Multi-chain DID synchronization
- Bridge integration for identity portability
- Unified DID view across chains
- Cross-chain attribute verification

**Automation & Integration**
- Automated re-verification workflows
- API gateways for off-chain systems
- Webhook integrations for events
- Mobile SDK for identity management

**Governance**
- Decentralized identity governance
- Community-driven attribute standards
- Multi-signature for critical operations
- Proposal and voting mechanisms

**Compliance Tools**
- Regulatory reporting dashboards
- Real-time compliance monitoring
- Automated audit report generation
- Jurisdiction-specific templates

**Interoperability**
- W3C DID standard compliance
- Verifiable Credentials support
- Integration with existing DID systems
- Universal identity resolver

---

## Documentation & Resources

### Technical Documentation
- [GitHub Repository](https://github.com/StoboxTechnologies/ST4DIDSC)
- [Infrastructure Overview](./README.md)
- [Stobox Docs V4](https://docs.stobox.io)

### Smart Contract Verification
- [Mainnet Contract on Arbiscan](https://arbiscan.io/address/0x25E6036178656b1329ee51696407b367D8C6ba84#code)
- [Testnet Contract on Sepolia Arbiscan](https://sepolia.arbiscan.io/address/0xA832662d1E11F2a6cEF706cE54A993E7eeEDC440#code)

### Related Documentation
- [STV3 Product Summary](./STV3-Product-Summary.md)
- [Token Offering Module Summary](./Token-Offering-Module-Summary.md)
- [StoboxProtocolSTV3 Release Notes](./StoboxProtocolSTV3-Release-Notes.md)
- [StoboxRWAVaultFactory Release Notes](./StoboxRWAVaultFactory-Release-Notes.md)

### Educational Resources
- Identity verification in blockchain
- Privacy-preserving KYC/AML
- Regulatory compliance for tokenized assets
- Decentralized identity standards

---

## Support & Contact

For integration support, technical questions, or partnership inquiries:

- **GitHub Repository:** [ST4DIDSC](https://github.com/StoboxTechnologies/ST4DIDSC)
- **Infrastructure Repo:** [Stobox-WEB3-Infrastructure](https://github.com/StoboxTechnologies/Stobox-WEB3-Infrastructure)
- **Documentation:** [Stobox Docs](https://docs.stobox.io)
- **Network Status:** [arbiscan.freshstatus.io](https://arbiscan.freshstatus.io)

---

## License

This project is licensed under the terms specified in the [LICENSE](./LICENSE) file.

---

## Summary

StoboxDID provides the **identity foundation** for compliant tokenized asset ecosystems:

✅ **Decentralized** - User-controlled identity on-chain  
✅ **Flexible** - Support diverse attribute types and use cases  
✅ **Privacy-Centric** - Controlled data sharing with time limits  
✅ **Compliant** - Built for regulatory requirements  
✅ **Multi-Address** - Single identity across multiple wallets  
✅ **Integrated** - Seamless ecosystem integration  
✅ **Scalable** - Arbitrum L2 for low-cost operations  
✅ **Transparent** - Complete audit trail via events  
✅ **Secure** - Role-based access control  
✅ **Future-Proof** - Extensible architecture for evolution  

---

**Last Updated:** December 2025  
**Status:** Production (Arbitrum One Mainnet)

