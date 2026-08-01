---
name: Create and track a reactive work order
description: Create a reactive (repair/maintenance) work order on the Lessen One Platform, update it, and poll its status.
api: One Open API v2 - Client
operations: [createReactiveWorkOrder, updateReactiveWorkOrder, reactiveWorkOrders]
source: https://developers.lessen.com/docs/#/client/guides/residential/reactive-work-order/create-reactive-work-order
---

# Create and track a reactive work order

Use the Client GraphQL API to open a repair/maintenance work order for a
property and follow it to completion.

## Auth
1. POST HTTP Basic Auth credentials to the environment token endpoint
   (`https://meshstage.lessen.com/one-graph-client/token` for sandbox) to get an
   `access_token`. Note `expires_in` (Unix epoch) and refresh before it lapses.
2. Send the token as a bearer token on every request to
   `/one-graph-client/graphql`. Unauthorized calls return 401.

## Steps
1. Create the work order with the `createReactiveWorkOrder` mutation. Supply the
   location, service/trade, and optional `clientNTE`, `dueDate`, and
   `woRefNum` (your external identifier). `sourceOfWOId` is optional.
2. Capture the returned Lessen work order id (`woId`) / number (`wONum`).
3. Update it as needed with `updateReactiveWorkOrder` (e.g. set/adjust
   `woRefNum` or access information).
4. Poll status with the `reactiveWorkOrders` query, filtering by `ids`, `woIds`,
   or `serviceRequestIds`; sort/paginate as needed.

## Rules
- Test only in the sandbox (meshstage) environment before Production.
- Handle the GraphQL `errors[]` envelope: read `extensions.code` (e.g.
  `INVALID_OPERATION`) rather than assuming HTTP status alone. See
  errors/sms-assist-problem-types.yml.
- No idempotency key is available; avoid blind retries of create mutations.
- Subscribe to webhook topics (Status Changed, Rating Created, etc.) instead of
  tight polling where possible — see asyncapi/sms-assist-webhooks.yml.
