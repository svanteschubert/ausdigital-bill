# UBL XML Billing Document Specifications

The billing process is supported by the invoice document (six variants) and the response document (four status codes) as described in the [billing process](Billing-Process.md) model.  This page specifies the required invoice and response document models.

## Invoice Document

Is defined by the DBC [CoreInvoice XML Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/ubl-json/spec/v1.0.0/maindoc/CoreInvoice-1.0.xsd) library.

* **[Invoice Samples](https://github.com/ausdigital/ausdigital-bill/tree/master/ubl-xml/samples/Invoice/)**

## Response Document

Is defined by the DBC [Document Response XML Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/spec/v1.0.0/Response.xsd) library.

* **[Response Sample](https://github.com/ausdigital/ausdigital-bill/tree/master/ubl-xml/samples/Response/SampleResponse-ConformantResponse.xml)**

The response document is a generic structure for all UBL document responses.  The generic response becomes a meaningful invoice response via the correct population of two key fields:
* "profileID" which MUST be one of the 6 invoice profiles define in the [billing process](Billing-Process.md#invoice-document-profiles)
* "statusReasonCode" which MUST contain one of the 4 values defined in the [invoice response codes](Billing-Process.md#document-response-codes)

# Validation Rules

Validation rules are listed in the [DBC e-invoice semantic model](https://github.com/ausdigital/dbc-specs/blob/master/eInvoicing_Semantic_Model_v1.0.pdf).  