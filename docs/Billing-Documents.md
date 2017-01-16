# Billing Document Specifications

The billing process is supported by the invoice document (six variants) and the response document (four status codes) as described in the [billing process](Billing-Process.md) model.  This page specifies the required invoice and response document models.

## Invoice Document

Is defined by the [Invoice Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/spec/v1.0.0/Invoice.json) which is a simple single root JSON Schema that is a semantically equivalent representation of the DBC [CoreInvoice XML Schema](https://github.com/ausdigital/dbc-specs/blob/master/DBC-AU/xsd/maindoc/CoreInvoice-1.0.xsd) library.

* **[Browsable Invoice Schema](http://ausdigital.org/docson.html#https://raw.githubusercontent.com/ausdigital/ausdigital-bill/master/spec/v1.0.0/Invoice.json)**
* **[Invoice Samples](https://github.com/ausdigital/ausdigital-bill/tree/master/samples)**

## Response Document

Is defined by the [Response Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/spec/v1.0.0/Response.json) which is a simple single root JSON Schema that is a semantically equivalent representation of the DBC [Document Response XML Schema](https://github.com/ausdigital/dbc-specs/blob/master/DBC-AU/xsd/maindoc/Response-1.0.xsd) library.

* **[Browsable Response Schema](http://ausdigital.org/docson.html#https://raw.githubusercontent.com/ausdigital/ausdigital-bill/master/spec/v1.0.0/Response.json)**
* **[Response Sample](https://github.com/ausdigital/ausdigital-bill/blob/master/samples/SampleResponse-ConformantResponse.json)**

The response document is a generic structure for all UBL document responses.  The generic response becomes a meaningful invoice response via the correct population of two key fields:
* "profileID" which MUST be one of the 6 invoice profiles define in the [billing process](Billing-Process.md#invoice-document-profiles)
* "statusReasonCode" which MUST contain one of the 4 values defined in the [invoice response codes](Billing-Process.md#document-response-codes)

# Validation Rules

Validation rules from the [DBC e-invoice semantic model](https://github.com/ausdigital/dbc-specs/blob/master/eInvoicing_Semantic_Model_v1.0.pdf) are re-stated here but are broken down according to the invoice document usage context as identified by the ProfielID element.  

## Validation Rules - Common
tbd

## Validation Rules - "ProfileID":"Invoice"
tbd

## Validation Rules - "ProfileID":"rcti"
tbd

## Validation Rules - "ProfileID":"taxreceipt"
tbd

## Validation Rules - "ProfileID":"adjustment"
tbd

## Validation Rules - "ProfileID":"creditnote"
tbd

## Validation Rules - "ProfileID":"debitnote"
tbd




