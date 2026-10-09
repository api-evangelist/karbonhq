---
name: karbonhq-invoice-payments
description: List and inspect Karbon invoices, then record, reverse or delete manual payments against them.
api: openapi/karbonhq-openapi.yml
source: https://developers.karbonhq.com/guides/invoices-and-payments.md
operations:
  - getInvoices
  - getInvoiceByKey
  - getPayments
  - getPaymentByPaymentKey
  - createManualPayment
  - reverseManualPayment
  - deleteManualPayment
---
# Invoices and manual payments in Karbon

1. List invoices with `getInvoices`. Filter with `$filter=InvoiceStatus eq 'Approved'` and sort with `$orderby=InvoiceDate desc`.
2. Read one invoice with `getInvoiceByKey`. Use `$expand=LineItems,Payments,Data` here only, because the list endpoint does not support `$expand`.
3. Record a payment with `createManualPayment`, giving the payment method, date and amount.
4. To undo a payment and keep an audit trail, call `reverseManualPayment` with the original `PaymentKey` and a reversal date. The payments list then shows a `Manual Payment Reversed` entry. `deleteManualPayment` removes the record instead.
5. Check the result with `getPayments` or `getPaymentByPaymentKey`.

Send `Authorization: Bearer {token}` and `AccessKey: {JWT}` on every call (https://developers.karbonhq.com/guides/authentication.md). On HTTP 429, back off as described in https://developers.karbonhq.com/guides/rate-limits.md. List endpoints page with OData `$top`/`$skip`; follow `@odata.nextLink` until it is absent.
