---
name: Review proposals and reconcile invoices
description: Query work order proposals and invoices on the Lessen One Platform to review and reconcile maintenance spend.
api: One Open API v2 - Client
operations: [workOrderProposals, invoices]
source: https://developers.lessen.com/docs/#/client/guides/residential/invoice/query-invoices
---

# Review proposals and reconcile invoices

## Auth
Bearer `access_token` from `/one-graph-client/token` (HTTP Basic Auth), sent on
every `/one-graph-client/graphql` request.

## Steps
1. Query proposals for a work order (fields include `purpose`, `smeNotes`).
   Approve or decline per your business rules.
2. Query invoices with the invoices query, filtering by `batchInvoiceIds`,
   `woNums`, or `woIds`. Read `invoiceTotal`, `approveNTE`, and `purpose`
   (e.g. "REPAIR").
3. For batch reconciliation, use the batch invoices query (`invoiceList.woId`
   ties each line back to a work order).

## Rules
- Invoice queries are limited to work orders created within the last 2 years.
- Handle the GraphQL `errors[]` envelope; refresh the token on 401.
- Test in sandbox (meshstage) before Production.
