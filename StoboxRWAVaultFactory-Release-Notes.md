## **StoboxRWAVaultFactory v1.0.3 Release**
**introduces enhanced token management capabilities, maximum issuance controls, trust management system, validation rule linking, and new package architecture with production deployment to Arbitrum One.**

## What's Changed
**Enhanced STV3TokenManagementFacet:** Added comprehensive token control functions including pause/unpause, trust/distrust management, and validation rule linking capabilities.

**Trust Management System:** Added dynamic trusted address management with `trustForSTV3Token` and `distrustForSTV3Token` functions.

**Validation Rule Management:** Implemented flexible rule linking/unlinking with `linkRuleForSTV3Token` and `unlinkRuleForSTV3Token` functions.

**Package System Upgrade:** Introduced new `1_3_0_STV3Pack` (version 1.3.0) with updated facet organization and improved validation capabilities.

**Production Deployment:** Successfully deployed to Arbitrum One (42161) with multi-phase deployment strategy and full verification.

**Full Changelog**: https://github.com/StoboxTechnologies/ST4TokenFactory/compare/v1.0.2...v1.0.3

---

## **StoboxRWAVaultFactory v1.0.2 Release**
**introduces advanced features: role-based access control, improved management of token facets, new scripts and library versions, ability to pause tokens, and ShadowVersion deployment to production.**

## What's Changed
**Factory Roles Added:** Introduction of roles for factory management and access control.

**Access Control to Factory Functionality:** Improved mechanisms for managing access to various factory functionalities.

**Token Facets Update Functionality:** Added and improved the ability to update token facets.

**New Facets Script & Libraries:** Updates to scripts and facet library versions.

**Pause Token Feature:** Added pause-token functionality to the STV3TokenManagementFacet.

**Shadow Version Deployment:** Deployment of ShadowVersion to ProdFactory.

**Full Changelog**: https://github.com/StoboxTechnologies/ST4TokenFactory/compare/v1.0.1...v1.0.2

---

## **StoboxRWAVaultFactory v1.0.1 Release**
**focuses on the initial setup, foundational architecture, and preparing for production deployment.**

## What's Changed
**Initial Foundry Project Setup:** The project was initialized with Foundry.

**Diamond Factory Implementation:** Added the diamond factory structure.

**Factory Base Version:** Established the base version of the factory.

**Production Preparation and Deployment:** Work towards preparing for production deployment, including setting up the prod blueprint package.

**Full Changelog**: https://github.com/StoboxTechnologies/ST4TokenFactory/commits/v1.0.1
