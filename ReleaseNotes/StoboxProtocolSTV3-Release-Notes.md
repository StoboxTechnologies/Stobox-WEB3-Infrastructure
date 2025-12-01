# StoboxProtocolSTV3 v1.4.0
### MonetaryFacet v1.1.0 Enhancement
- **New Issuance Controls**: Added `setMaxIssuance()`, `maxIssuance()`, and `totalIssued()` functions
- **Enhanced Treasury Management**: Improved token issuance limits and tracking
- **Production Deployment**: MonetaryFacet v1.1.0 deployed to `0xFC9415a3f72CC1645f9C9994475f1482832622B3`

### Technical Details
- **Backward Compatible**: No breaking changes to existing functionality

For a complete list of changes, see the [full changelog](https://github.com/StoboxTechnologies/STV3_protocol/compare/v1.3.0...v1.4.0)

---

# StoboxProtocolSTV3 v1.3.0
This release introduces enhanced token management controls and validation capabilities to the STV3 Protocol:

- **ValidationFacet v1.0.1** - Adds comprehensive token control and validation management with deployer-only access restrictions.
- **Token Pause/Unpause Controls** - Implements emergency stop functionality allowing deployers to halt and resume token transfers for security or maintenance purposes.
- **Trust Management System** - Enables granular control over trusted addresses with the ability to add/remove addresses from trusted lists and query trust status.
- **Validation Rule Framework** - Provides comprehensive rule-based validation system with support for linking/unlinking validation rules and retrieving rule descriptions.
- **Enhanced Security** - All administrative functions are protected by deployer-only access control, ensuring secure management of critical token operations.
- **Production Deployment** - Successfully deployed to Arbitrum One mainnet with full factory integration and backward compatibility.
- **Emergency Response Capability** - Introduces immediate response mechanisms for security incidents through token pause functionality and validation controls.

For a complete list of changes, see the [full changelog](https://github.com/StoboxTechnologies/STV3_protocol/commits/v1.3.0)

---

# StoboxProtocolSTV3 v1.0.0

**This is the first major release of the STV3 Protocol, introducing several key features and improvements:**

- **DiamondCutFacet** - Allows upgrades and modifications of contract facets.
- **DiamondLoupeFacet** - Enables inspection and querying of available facets.
- **EmergencyFacet** - Implements emergency controls and pause mechanisms.
- **MonetaryFacet** - Adds comprehensive monetary functions and logic. Handles monetary operations and value transfers.
- **RolesFacet** - Manages user roles and access permissions.Implements robust role-based access control within the protocol.
- **ValidationFacet** - Enforces advanced validation and rules for contract actions.
- **Treasury Implementation** - Integrates treasury management and operations.
- **Factory Setup** - Adds tools for setting up protocol factories.
- **Blueprint Package** - Prepares the protocol for production deployment with blueprint packaging.


For a complete list of changes, see the [full changelog](https://github.com/StoboxTechnologies/STV3_protocol/commits/v1.0.0).
