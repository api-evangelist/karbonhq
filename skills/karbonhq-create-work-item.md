---
name: karbonhq-create-work-item
description: Create a Karbon work item for a client, optionally from a work template, using valid work types and statuses.
api: openapi/karbonhq-openapi.yml
source: https://developers.karbonhq.com/guides/creating-work-items.md
operations:
  - getTenantSettings
  - getAllWorkTemplates
  - getWorkTemplateByID
  - createWorkItem
  - getAllWorkItems
  - getWorkItemByID
  - patchWorkItemByID
---
# Create a work item in Karbon

1. Resolve the client key with the `karbonhq-find-client` skill.
2. Call `getTenantSettings` to read the firm's valid work types and statuses.
3. Optionally find a template with `getAllWorkTemplates`, for example `$filter=Title eq 'FY Income Tax Return'`.
4. Call `createWorkItem` with the required fields `Title`, `AssigneeEmailAddress`, `ClientKey`, `ClientType` (`Contact`, `Organization` or `ClientGroup`) and `StartDate` (ISO 8601). To use a template, add its `WorkTemplateKey`.
5. Confirm with `getWorkItemByID`. Make later changes with `patchWorkItemByID`.
6. `createWorkItem` is a plain POST with no documented idempotency key. Before retrying a failed create, check for a duplicate with `getAllWorkItems`, filtering on `ClientKey` and title.

Send `Authorization: Bearer {token}` and `AccessKey: {JWT}` on every call (https://developers.karbonhq.com/guides/authentication.md). On HTTP 429, back off as described in https://developers.karbonhq.com/guides/rate-limits.md. List endpoints page with OData `$top`/`$skip`; follow `@odata.nextLink` until it is absent.
