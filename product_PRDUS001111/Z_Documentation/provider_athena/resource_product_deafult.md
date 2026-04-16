

# Resource Product Change Documentation (CLO UPI)

## File in Scope
resource_product_default.yaml

## Purpose of This File
`resource_product` in Athena defines the resource layer that binds one or more form factor products to a single resource product code.

In simple words:
- Form factor = instrument identity type (for CLO UPI this is VPA-based form factor).
- Resource product = container that references those form factors.
- Payment product later points to this resource product.

So this file is the bridge between form factor and payment product.

## Why We Changed It for CLO UPI
Earlier setup was card-oriented and used card form factor mapping.  
For CLO UPI, resource product must reference CLO UPI form factor code instead of card form factor code.

## Changes Applied

### 1) Metadata naming aligned to CLO UPI
- `metadata.name` changed from old 9978/default/card-style naming to CLO UPI naming for product 1111.
- `spec.metadata.id` changed similarly to CLO UPI naming.
- `spec.metadata.name` and `spec.metadata.description` updated to CLO UPI business names.

Reason:
- Clean operational traceability.
- No confusion with old card/default naming.

### 2) Resource product code changed
- `value.code` changed to: `RSPINZZ001111`

Reason:
- Resource product code should clearly map to CLO UPI product identity (`001111`).
- This code is referenced by payment product; consistency is mandatory.

### 3) Form factor mapping changed to CLO UPI form factor
- `value.formFactorProducts` changed from card form factor code to:
  - `FFPINZZ001111CLOU01`

Reason:
- Resource product must point to CLO UPI form factor created in previous step.
- Without this, payment flow still stays tied to card form factor.

### 4) Display name updated
- `viewConfig.displayName` changed to CLO UPI friendly name.

Reason:
- Better readability for product operations teams.

## What Was Not Changed

1. `kind`, `apiVersion`, and YAML structure.
2. `schemaInlineContent` (existing schema already supports required fields).
3. `tenantId`, `tenantCode`, requester module details.
4. `status` remains `ACTIVE`.

## Final Expected Value Block (CLO UPI)
```yaml
value:
  code: RSPINZZ001111
  description: Credit Line on UPI Resource Product for 1111
  formFactorProducts:
    - FFPINZZ001111CLOU01
  name: CLO UPI Resource Product 1111
  status: ACTIVE
```

## Dependency Mapping
This file depends on:
- CLO UPI Form Factor Product code: `FFPINZZ001111CLOU01`

This file is used by:
- Athena payment product via `resourceProductCode: RSPINZZ001111`

## Validation Checklist

1. `value.formFactorProducts` contains only CLO UPI form factor code(s).
2. `value.code` is unique and consistent with product code `001111`.
3. `status` is `ACTIVE`.
4. Metadata naming and display name no longer mention default/card semantics.
5. Next payment-product file uses `resourceProductCode: RSPINZZ001111`.

## Next File to Update
Next dependent file is:
payment_product_default.yaml

It must reference this updated resource product code and CLO UPI channel/payment code mapping.