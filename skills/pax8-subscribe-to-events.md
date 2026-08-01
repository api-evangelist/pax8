---
name: Subscribe to Pax8 events
description: Discover webhook topics, create a webhook subscription with filters, test it, and monitor delivery logs.
api: openapi/pax8-webhooks-api-openapi.json
operations: [getTopicDefinitions, Webhooks_create, addWebhookTopic, testWebhookTopic, WebhookLogs_query, retryWebhookDelivery]
---

# Subscribe to Pax8 events

Automates event-driven integration via the Pax8 Webhooks API (`https://api.pax8.com/api/v2`).

## Auth
OAuth2 client-credentials (`audience=https://api.pax8.com`); `Authorization: Bearer <token>`.

## Steps
1. `getTopicDefinitions` (GET /webhooks/topic-definitions) to browse available topics, their `availableFilters`, and `samplePayload`.
2. `Webhooks_create` (POST /webhooks) with the delivery URL and initial topic(s).
3. `addWebhookTopic` (POST /webhooks/{id}/topics) to attach more topics with `FilterCondition`s so only relevant events deliver (e.g. subscription completed line items, provisioning).
4. `testWebhookTopic` (POST /webhooks/{id}/topics/{topic}/test) to send a test event to your endpoint.
5. `WebhookLogs_query` (GET /webhooks/{webhookId}/logs) filtered by last-delivery status; `retryWebhookDelivery` (POST .../retry) for failures.

## Rules
- Scope with filters to avoid over-delivery; the granular filtering change narrowed default subscription notifications.
- Error `type` is enumerated: NOT_FOUND, UNAUTHORIZED, FORBIDDEN, BAD_REQUEST, INTERNAL_SERVER_ERROR, INVALID_PARAM.
