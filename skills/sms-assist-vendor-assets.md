---
name: Look up vendor location assets
description: As a Lessen vendor/affiliate, look up location assets for a client and pull asset details tied to a work order.
api: One Open API v2 - Vendor
operations: [getLocationAssetPagedList, getLocationAssetDetails, getLocationAssetDetailsByWorkOrder]
source: https://developers.lessen.com/docs/#/vendor/guides/asset/get-location-asset-paged-list
---

# Look up vendor location assets

## Auth
Obtain a bearer `access_token` from the Vendor token endpoint
(`/one-graph-vendor/token`) via HTTP Basic Auth (separate credentials from the
Client surface). Send it on every Vendor API request.

## Steps
1. List location assets for a selected client with
   `GET /asset/location-asset/paged-list`. Note the hidden default filters: when
   `is_active` is omitted only active assets return; when `service_category_ids`
   is omitted the client's Facilities service categories apply; when
   `is_test_data_excluded` is omitted test locations are excluded.
2. Pull full details for one asset with
   `GET /asset/location-asset/{assetId}/details` (includes `qrCode`,
   `damperTypeName`, factory-charge `typeName`).
3. For work-order context, use
   `GET /asset/location-asset/reactive/{workOrderId}/asset-details` to get the
   assets linked to a reactive work order (metadata, uptime, warranty dates,
   `qrCode`, `installationDate`, attachments).

## Rules
- Endpoints are tenant-scoped: requesting resources outside your affiliate
  returns a uniform 404. Do not treat 404 as "does not exist" globally.
- Subscribe to vendor webhook topics 114 (Asset Added) / 115 (Asset Removed) to
  track asset changes on a work order. See asyncapi/sms-assist-webhooks.yml.
