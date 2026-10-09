---
name: karbonhq-find-client
description: Look up a Karbon client (Contact, Organization or Client Group) by name, email or your own identifier, then retrieve its full record.
api: openapi/karbonhq-openapi.yml
source: https://developers.karbonhq.com/guides/searching-clients.md
operations:
  - getAllContacts
  - getContactByID
  - getContactByUDI
  - getAllOrganizations
  - getOrganizationByID
  - getOrganizationByUDI
  - getClientGroups
  - getClientGroupByID
  - getClientGroupByUDI
---
# Find a client in Karbon

1. Pick the client type. People are Contacts (`getAllContacts`), businesses are Organizations (`getAllOrganizations`) and households or groups are Client Groups (`getClientGroups`).
2. Filter with OData `$filter`. For example, `FullName eq 'Priya Sharma'` or `contains(FullName, 'Sharma')` on Contacts. Contacts support `FullName`, `EmailAddress`, `PhoneNumber` and `ContactType`. Organizations support `FullName`, `EmailAddress` and `ContactType`. Client Groups support only `FullName eq`.
3. If you hold your own identifier, call `getContactByUDI`, `getOrganizationByUDI` or `getClientGroupByUDI`.
4. Fetch the full record with `getContactByID`, `getOrganizationByID` or `getClientGroupByID`. Keep the returned key: other entities use it as `ClientKey`.

Send `Authorization: Bearer {token}` and `AccessKey: {JWT}` on every call (https://developers.karbonhq.com/guides/authentication.md). On HTTP 429, back off as described in https://developers.karbonhq.com/guides/rate-limits.md. List endpoints page with OData `$top`/`$skip`; follow `@odata.nextLink` until it is absent.
