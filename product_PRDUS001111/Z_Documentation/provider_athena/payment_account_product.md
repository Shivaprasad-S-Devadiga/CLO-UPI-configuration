# Athena Payment Account Product Change Documentation (CLO UPI)

## 1. Document Purpose
This document explains the creation of Athena Payment Account Product for CLO UPI (`PRDUS001111`), what was configured, why each value was chosen, and how it connects with the rest of the product graph.

## 2. File in Scope
Target file (new):
`product_configurations/product_PRDUS001111/provider_athena/payment_account_product/payment_account_product_clou.yaml`

Reference source patterns:
1. payment-account-product_jazz-ppc.yaml
2. payment-account-product_jazz-prepaidcard.yaml
3. payment-account-product_flexcube-ppc.yaml

## 3. Why This File Is Needed
Even if old `9978` Athena folder does not currently contain this file, bundle definition already has `paymentAccountProduct` mapping.  
To make CLO UPI product complete and explicit, Payment Account Product should be defined with a dedicated CLO UPI code and type.

Related bundle mapping file:
bundle-definition_default.yaml

## 4. Business Role of Payment Account Product
Payment Account Product defines:
1. Payment account type (`UPI`, `CREDIT_CARD`, etc.).
2. Provider code that actually manages payment accounts.
3. Base currency and product identity metadata.

It is the account-level policy binding for the payment product and bundle consumption.

## 5. Final CLO UPI Values Used
1. `code`: `PPDINZZ001111`
2. `name`: `CLO UPI Payment Account Product 1111`
3. `description`: `Credit Line on UPI Payment Account Product for 1111`
4. `type`: `UPI`
5. `baseCurrency`: `INR`
6. `paymentAccountProviderCode`: `PAPUS009002`
7. `status`: `ACTIVE`

## 6. What Changed and Why

1. Added CLO UPI-specific metadata naming
- `metadata.name` and `spec.metadata.id` aligned with `1111.clou-upi.payment-account-product`.

Why:
- Keeps identity consistent with other CLO UPI Athena files.
- Avoids default/card naming confusion.

2. Set product type to `UPI`
Why:
- Product is Credit Line on UPI, not card PAN form factor.
- This ensures downstream systems treat this as UPI account type.

3. Set provider to `PAPUS009002`
Why:
- Existing tenant has this provider active for shadow-ledger style mappings.
- Works as practical default unless a dedicated UPI PAP is assigned by platform team.

4. Set base currency to INR
Why:
- Tenant and use-case are India UPI oriented.

5. Kept status ACTIVE
Why:
- Product should be deployable and testable immediately.

## 7. What Was Not Changed
1. Overall schema structure (`schemaInlineContent`) reused from known Athena payment-account-product pattern.
2. Tenant/requester values kept under same tenant/module context.
3. No tags/vectors/currency restrictions added yet; can be extended later if policy requires.

## 8. Dependency and Mapping Checks

1. Bundle-definition should point to this code:
- `paymentAccountProduct.productCode = PPDINZZ001111`

2. Payment product and resource product chain should already be CLO UPI aligned:
- Payment Product code: `PYPINZZ001111`
- Resource Product code: `RSPINZZ001111`
- Form Factor code: `FFPINZZ001111CLOU01`

3. Provider code existence must be verified in tenant:
- `PAPUS009002` should exist and be active in payment-account-provider configs.

## 9. Validation Checklist

1. File exists in target CLO UPI product folder path.
2. `type` is `UPI`.
3. `paymentAccountProviderCode` resolves successfully in tenant.
4. Bundle mapping uses `PPDINZZ001111`.
5. Product onboarding/resolution does not fail on missing payment-account-product reference.

## 10. Risks and Notes
1. If platform mandates a dedicated UPI provider code, replace `PAPUS009002`.
2. If product strategy models CLO under `CREDIT_CARD` even on UPI rail, type may need to be switched from `UPI` to `CREDIT_CARD` based on architecture decision.
3. Keep naming conventions consistent across all Athena files to simplify maintenance and audits.