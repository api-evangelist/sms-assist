---
name: Create and query recurrent work orders
description: Create recurrent (scheduled) work orders on the Lessen One Platform and query them with filtering, sorting, and pagination.
api: One Open API v2 - Client
operations: [createWorkOrder, updateWorkOrder, recurrentWorkOrders]
source: https://developers.lessen.com/docs/#/client/guides/residential/recurrent-work-order/create-recurrent-work-order
---

# Create and query recurrent work orders

## Auth
Obtain a bearer `access_token` from `/one-graph-client/token` via HTTP Basic
Auth and send it on every `/one-graph-client/graphql` request.

## Steps
1. Create a recurrent work order with the `createWorkOrder` mutation (added
   2026-07-07).
2. Update the customer reference number and access information with
   `updateWorkOrder` (added 2026-06-23).
3. List recurrent work orders with the `recurrentWorkOrders` query using
   filtering, sorting, and pagination arguments (added 2026-06-09).

## Rules
- Subscribe to recurrent work order webhook topics to stay in sync: 300
  (Created), 302 (Status Changed), 303 (Schedule Changed), 304 (Service
  Combination Updated), 305 (Check-In/Out Time Changed). See
  asyncapi/sms-assist-webhooks.yml.
- Read `extensions.code` on GraphQL errors; refresh the token on 401.
- Validate flows in the sandbox (meshstage) before Production (meshlive).
