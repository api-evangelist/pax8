---
name: Manage a subscription
description: Find a company's subscriptions, inspect one and its history, and apply lifecycle changes (quantity, status, billing term).
api: openapi/pax8-partner-endpoints-openapi.json
operations: [findSubscriptions, findSubscriptionBySubscriptionId, findSubscriptionHistoryBySubscriptionId, updateSubscription, findSubscriptionUsageSummaries]
---

# Manage a subscription

Automates subscription lifecycle changes on the Pax8 marketplace.

## Auth
OAuth2 client-credentials (`audience=https://api.pax8.com`); `Authorization: Bearer <token>`.

## Steps
1. `findSubscriptions` (GET /subscriptions) filtered by `companyId` to list the customer's subscriptions.
2. `findSubscriptionBySubscriptionId` (GET /subscriptions/{subscriptionId}) to read current quantity, status, product, and billing term.
3. `findSubscriptionHistoryBySubscriptionId` for prior changes when reconciling.
4. `updateSubscription` (PUT /subscriptions/{subscriptionId}) to change quantity, status, or billing term. Respect Pax8 timing/billing rules (changes may take effect on the next billing cycle).
5. For usage-based products, `findSubscriptionUsageSummaries` (GET /subscriptions/{subscriptionId}/usage-summaries) to review metered usage.

## Rules
- PUT replaces mutable subscription fields — send the full intended state.
- Handle 404 (unknown subscription) and 422 (invalid change) from the `{type,message,instance,status,details[]}` envelope.
