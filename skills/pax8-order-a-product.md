---
name: Order a product for a company
description: Create or find a partner company, find the product and its pricing, then place an order on the Pax8 marketplace.
api: openapi/pax8-partner-endpoints-openapi.json
operations: [findCompanies, createCompany, findAllProducts, findPricingByProductId, findProvisionDetailsByProductId, createOrder, findOrdersByOrderId]
---

# Order a product for a company

Automates the Pax8 buy flow: ensure the customer company exists, resolve the product and pricing, then submit an order.

## Auth
- OAuth2 client-credentials. POST to `https://api.pax8.com/v1/token` with `grant_type=client_credentials`, `client_id`, `client_secret`, and `audience=https://api.pax8.com`.
- Send `Authorization: Bearer <token>` on every call. Tokens live ~24h.

## Steps
1. `findCompanies` (GET /companies) to check whether the customer already exists. If not, `createCompany` (POST /companies) with name and address.
2. `findAllProducts` (GET /products) or `findProductByProductId` to locate the product/SKU.
3. `findPricingByProductId` (GET /products/{productId}/pricing) to confirm price, and `findProvisionDetailsByProductId` for required provisioning fields.
4. `createOrder` (POST /orders) with `companyId` and line items (productId, quantity, commitment/billing term).
5. `findOrdersByOrderId` (GET /orders/{orderId}) to confirm the order and read resulting subscription ids.

## Rules
- Errors return `{type,message,instance,status,details[]}` (not RFC 9457). Handle 422 (validation) and 403 (permission) explicitly.
- No idempotency key is available — de-dupe by checking `findOrders` before retrying a create.
- Paginate with `page` + `size`.
