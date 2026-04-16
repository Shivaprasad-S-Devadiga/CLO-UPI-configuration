
# Athena Payment Product Change Documentation for CLO UPI

## 1. Document Purpose
This document explains the complete conversion of Athena payment product configuration from default/card-style setup to CLO UPI setup, including why each field was changed and which source files were used for channel and payment-code validation.

## 2. File in Scope
Target file:
payment_product_default.yaml

## 3. Business Goal
Convert payment product mapping to support Credit Line on UPI product PRDUS001111, while keeping configuration structure compatible with existing Athena schema and tenant setup.

## 4. Source of Truth Used for Channel and PMC Mapping
1. Channel source:
channelcode_jazz-onus-channel.yaml

2. Existing validated payment mapping source:
payment-product_jazz.yaml

3. Converted product working file:
payment_product_default.yaml

## 5. Final CLO UPI Values Applied
1. Product identity:
name aligned to CLO UPI for 1111

2. Payment product code:
PYPINZZ001111

3. Resource product reference:
RSPINZZ001111

4. Channel mapping:
CHNINZZ5901

5. Payment codes:
PMCINZZ009912906
PMCINZZ009913906
PMCINZZ009914906
PMCINZZ009914904

## 6. What Changed and Why

1. metadata.name changed to CLO UPI naming
Why:
Improves operational clarity and prevents confusion with legacy default/card naming.

2. spec.metadata.id changed to CLO UPI naming
Why:
Keeps internal identifier consistent with product business identity and avoids mixed naming lineage.

3. spec.metadata.name and spec.metadata.description changed
Why:
Makes product intent explicit for operations, support, and audit users.

4. value.code changed from template-style code to PYPINZZ001111
Why:
Payment product code must be product-specific and match bundle mapping.

5. value.resourceProductCode changed to RSPINZZ001111
Why:
This links payment product to converted CLO UPI resource product. Without this, mapping points to old product graph.

6. value.channelCodePaymentCodeMapping changed
Why:
Transaction routing behavior is defined here. CLO UPI must use correct channel and PMCs that are already validated in tenant config.

7. viewConfig.displayName changed to CLO UPI name
Why:
Improves visibility in consoles and eliminates default/card wording ambiguity.

## 7. Why CHNINZZ5901 Was Chosen
1. It exists in channel config:
channelcode_jazz-onus-channel.yaml

2. It is already used in existing tenant UPI-linked payment setup:
payment-product_jazz.yaml

3. It is already connected to UPI recon/event flow in this tenant’s Athena ecosystem, making it safer for controlled CLO UPI rollout.

## 8. Why These PMCs Were Chosen
1. These PMCs are already mapped with CHNINZZ5901 in an existing working tenant product:
payment-product_jazz.yaml

2. Reusing verified PMCs reduces risk versus inventing new mapping without switch-level validation.

3. They preserve route compatibility with existing channel-code configuration:
channelcode_jazz-onus-channel.yaml

## 9. What Was Not Changed
1. schemaInlineContent structure
Reason:
Current schema already supports channelCodePaymentCodeMapping and all required payment product fields.

2. tenantId, tenantCode, requester metadata
Reason:
Product remains under same tenant and same Athena module context.

3. status kept ACTIVE
Reason:
Product remains deployable and testable after conversion.

## 10. Dependency Chain Validation
The following chain must remain consistent after this change:

1. Form factor product code used by resource product:
FFPINZZ001111CLOU01

2. Resource product code used by payment product:
RSPINZZ001111

3. Payment product code used by bundle mapping:
PYPINZZ001111

