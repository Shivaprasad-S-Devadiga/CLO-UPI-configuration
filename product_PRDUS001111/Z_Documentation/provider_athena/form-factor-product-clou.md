
# Athena Form Factor Product Change Note (CLO UPI)

## Document Purpose
This document explains the changes made in Athena Form Factor Product configuration while converting from card-based setup to Credit Line on UPI (CLO UPI), so any team member can understand what changed and why.

## Scope
Configuration discussed:
form_factor_product_card.yaml

Target product identity used in this update:
- Product reference: PRDUS001111
- Business name: CLO UPI Form Factor Product 1111

## Business Context
Earlier form factor was card-based.  
For CLO UPI, the payment instrument should be UPI-handle based (VPA), not card PAN based.

That is why provider and type were changed to UPI-compatible values.

## Summary of Changes

1. Product naming updated to CLO UPI
- spec.metadata.name changed to CLO UPI Form Factor Product 1111
- spec.metadata.description changed to Credit Line on UPI Form Factor Product for 1111
- viewConfig.displayName changed to CLO UPI Form Factor Product 1111

Why:
- Makes product intent clear in operations and admin views.
- Avoids confusion with legacy card naming.

2. Form factor code updated
- value.code changed to FFPINZZ001111CLOU01
- value.skuID changed to SKUINZZ001111CLOU01

Why:
- Introduces product-specific code for CLO UPI.
- Ensures clean mapping from resource product to this new form factor.

3. Instrument provider changed from card provider to UPI provider
- value.provider changed from CMS to UPIMS

Why:
- CMS is card management provider.
- UPIMS is the expected provider family for UPI form factors.

4. Instrument type changed from card to vpa
- value.type changed from card to vpa

Why:
- CLO UPI instrument is based on Virtual Payment Address.
- This is the core technical switch from card instrument to UPI instrument.

5. Status values kept active
- value.issuanceStatus remains ACTIVE
- value.paymentStatus remains ACTIVE

Why:
- Product should remain deployable/usable after conversion.
- No business ask to disable issuance or payment status.

## What Was Not Changed

1. API wrapper and resource kind
- kind, apiVersion, and structural schema wrapper were not changed.

2. Schema model block
- schemaInlineContent already supports provider UPIMS and type vpa, so no schema edits were required.

3. Tenant and requester context
- tenantId, tenantCode, requester.module, requester.moduleVersion were not changed.


## Final Effective CLO UPI Values
- value.code: FFPINZZ001111CLOU01
- value.name: CLO UPI Form Factor Product 1111
- value.description: Credit Line on UPI Form Factor Product for 1111
- value.provider: UPIMS
- value.type: vpa
- value.skuID: SKUINZZ001111CLOU01
- value.issuanceStatus: ACTIVE
- value.paymentStatus: ACTIVE

## Validation Checklist

1. Provider check
- Confirm provider is UPIMS.

2. Type check
- Confirm type is vpa.

3. Code mapping readiness
- Confirm this value.code is referenced in Athena resource product formFactorProducts list.

4. Naming clarity
- Confirm displayName, spec metadata name, and description clearly indicate CLO UPI.

5. No card dependency left in value block
- Confirm no card-only value remains in provider or type.

## Next Dependency
After this form-factor update, next file to align is Athena resource product:
resource_product_default.yaml

It must reference FFPINZZ001111CLOU01 in formFactorProducts.