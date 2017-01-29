# UBL Syntax 2.0 Billing Document Specifications

The billing process is supported by the invoice document (six variants) and the response document (four status codes) as described in the [billing process](Billing-Process.md) model.  This page specifies the required invoice and response document models.

## Invoice Document

Is defined by the [Invoice Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/syn/spec/v1.0.0/Invoice.json) which is a simple single root JSON Schema that is a semantically equivalent representation of the DBC [CoreInvoice XML Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/syn-v1/spec/v1.0.0/maindoc/CoreInvoice-1.0.xsd) library.

* **[Browsable Invoice Schema](http://ausdigital.org/docson.html#https://raw.githubusercontent.com/ausdigital/ausdigital-bill/master/syn/spec/v1.0.0/Invoice.json)**
* **[Invoice Samples](https://github.com/ausdigital/ausdigital-bill/tree/master/syn/samples/Invoice/)**

## Response Document

Is defined by the [Response Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/spec/v1.0.0/Response.json) which is a simple single root JSON Schema that is a semantically equivalent representation of the DBC [Document Response XML Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/syn-v1/spec/v1.0.0/maindoc/Response-1.0.xsd) library.

* **[Browsable Response Schema](http://ausdigital.org/docson.html#https://raw.githubusercontent.com/ausdigital/ausdigital-bill/master/spec/v1.0.0/Response.json)**
* **[Response Sample](https://github.com/ausdigital/ausdigital-bill/tree/master/syn/samples/SampleResponse-ConformantResponse.json)**

The response document is a generic structure for all UBL document responses.  The generic response becomes a meaningful invoice response via the correct population of two key fields:
* "profileID" which MUST be one of the 6 invoice profiles define in the [billing process](Billing-Process.md#invoice-document-profiles)
* "statusReasonCode" which MUST contain one of the 4 values defined in the [invoice response codes](Billing-Process.md#document-response-codes)

# Validation Rules

Validation rules from the [DBC e-invoice semantic model](https://github.com/ausdigital/ausdigital-bill/blob/master/docs/eInvoicing_Semantic_Model_v1.0.pdf) are re-stated here but are broken down according to the invoice document usage context as identified by the ProfielID element.  

## Validation Rules - Common
| Rule | Mandatory | Optional | Extension |
|------|-----------|----------| ----------|
| An Invoice of more than $82.50 (including GST) to a GST-registered Buyer MUST be a Tax Invoice. | X | | |
| An Invoice must contain a Document Type Code | X | | |
| An Invoice MUST contain the Supplier’s Business Name or the ABN of the Supplier. | X | | |
| An Invoice with a Total Amount greater than $1000 MUST have either the Buyer's Business Name or the ABN of the Buyer. | X | | |
| An Invoice MAY contain the ABN plus a GST branch number for Suppliers with GST branches registered with the ATO. | | X | |
| An Invoice MUST contain an Invoice Issue Date. | X | | |
| An Invoice Line MUST have a Description.  | X | | |
| An Invoice MAY contain a Description of Properties of Invoiced Items.  | | X | |
| An Invoice Line MAY contain an Invoiced Quantity.  | | X | |
| An Invoice Line MUST contain the Invoice Line Extension Amount (Net Price multiplied by Invoiced Quantity) (excluding GST) for the Items sold.  | X | | |
| An Invoice MUST contain the sum total of all Invoice Line Extension Amounts. | X | | |
| An Invoice Line MUST contain the GST Amount for the Items sold or indicate the extent to which Items are taxable. | X | | |
| An Invoice Line MUST contain the Amount Payable (Invoice Line Extension Amount plus GST Amount) for the Items sold. | X | | |
| An Invoice Line MAY contain a GST Amount of zero. | | X | |
| An Invoice Line MAY specify a GST Category. | | X | |
| An Invoice MUST contain the Invoice level Tax Amount exclusive of GST. | X | | |
| An Invoice MUST contain the Invoice level GST Total Amount. | X | | |
| An Invoice MUST have an Invoice Identifier. | X | | |
| An Invoice MUST have a Supplier Business Name. | X | | |
| An Invoice MUST have a valid Document Type Code. | X | | |
| An Invoice MUST have an Issue Date. | X | | |
| An Invoice MAY have a Delivery Date. | | X | |
| An Invoice MAY have an Invoice Period. | | X | |
| An Invoice MAY have an Invoice Period Start Date. | | X | |
| An Invoice MAY have an Invoice Period End Date. | | X | |
| An Invoice End Period Date MUST be later or equal to an Invoice Period Start Date. | X | | |
| An Invoice MAY have a Sales Order Identifier. | | X | |
| An Invoice MAY have a Purchase Order Identifier. | | X | |
| An Invoice MAY have a Contact Identifier. | | X | |
| An Invoice MAY have an Electronic Address. | | | X |
| An Invoice MUST have at least one Invoice Line. | X | | |
| An Invoice Line Item MAY have a Suppliers Item Identifier. | | X | |
| An Invoice Line Item MUST have a Description. | X | | |
| An Invoice Line MAY have a Quantity. | | X | |
| An Invoice Line MAY have a Net Amount. | | X | |
| An Invoice Line MAY have a Dispatch Advice Identifier. | | X | |
| An Invoice Line MAY have a Receipt Advice Identifier. | | X | |
| An Invoice Line MAY have a delivery Address. | | | X |
| An Invoice Line Item MAY have a Country of Origin. | | | X |
| An Invoice MAY have one or more Document References. | | X | |
| The Invoice Level Net Amount MUST be equal to the sum of Invoice Line Net Amounts. | X | | |
| The Invoice Level Allowance Amount MUST be equal to the sum of Invoice Line Allowances plus any Invoice Level Allowances. | X | | |
| The Invoice Level Charge Amount MUST be equal to the sum of Invoice Line Charges plus any Invoice Level Charges. | X | | |
| The Invoice Level Net Amount MUST be equal to the Invoice Level Gross Amount - Invoice Level Allowance Amount + Invoice Level Charge Amount. | X | | |
| The Invoice Level GST Amount MUST be equal to the sum of Invoice Line GST Amounts. | X | | |
| The Invoice Level Total Amount MUST be equal to the Invoice Level Net Amount + the Invoice Level Tax Amount. | X | | |
| An Invoice MAY have an Amount Payable. | | X | |
| An Invoice MUST have an Invoice Level Total Amount. | X | | |
| An Invoice MAY have an Invoice Level Net Amount. | | X | |
| An Invoice MAY have a Related Invoice Identifier. | | X | |
| An Invoice MAY have an Amount Payable. | | X | |
| An Invoice MAY have one or more Document References. | | X | |
| An Invoice Level Total Amount MUST be greater than 0. | X | | |
| An Invoice Line MAY have a Net Amount. | | X | |
| An Invoice Line Extended Amount after all allowances and charges MUST NOT be negative. | X | | |
| An Invoice Line Price MUST be 0 or more. | X | | |
| An Invoice Line Item MUST have a Net Price. | X | | |
| An Invoice Line MAY have a Quantity. | | X | |
| An Invoice MAY have an Allowance Rate and Base Amount at Invoice Level. | | X | |
| An Invoice Level Allowance MUST be greater than 0. | X | | |
| An Invoice Level Allowance MAY have a GST Category. | | X | |
| An Invoice Level Allowance Reason Description MUST match the Invoice Level Allowance Reason Code (if any). | X | | |
| An Invoice MAY have a Charge Rate and Base Amount at Invoice Level. | | X | |
| An Invoice Level Charge MUST be greater than 0. | X | | |
| An Invoice Level Charge MAY have a GST Category. | | X | |
| An Invoice Level Charge Reason Description MUST match the Invoice Level Charge Reason Code (if any). | X | | |
| An Invoice Line MAY have an Allowance Rate and Base Amount. | | X | |
| An Invoice Line Allowance MUST be greater than 0. | X | | |
| An Invoice Line Allowance MUST have an Allowance Reason Description. | X | | |
| An Invoice Line Allowance Reason Description MUST match the Invoice Line Allowance Reason Code (if any). | X | | |
| An Invoice Line MAY have a Charge Rate and Base Amount. | | X | |
| An Invoice Line Charge MUST be greater than 0. | X | | |
| An Invoice Line Charge MUST have a Charge Reason Description. | X | | |
| An Invoice Line Charge Reason Description MUST match the Invoice Line Charge Reason Code (if any). | X | | |
| A Payment Means MUST have a valid Payment Means Type Code. | X | | |
| A Payment Means Financial Institution Account Identifier MUST have Financial Institution Identifier. | X | | |
| A Payment Means for a card payment MUST state the last 4 to 6 digits of the Financial Institution Account Identifier. | X | | |
| An Invoice MUST have a Payee Business Name if Payee Business Name is not the same as the Suppliers Business Name. | X | | |

## Validation Rules - "ProfileID":"Invoice"
| Rule | Mandatory | Optional | Extension |
|------|-----------|----------| ----------|
| A Tax Invoice for goods or services that do not all include GST (mixed supplies) shall indicate which goods or services do not include GST. | X | | |

## Validation Rules - "ProfileID":"rcti"
| Rule | Mandatory | Optional | Extension |
|------|-----------|----------| ----------|
| A Recipient Created Tax Invoice MUST contain either the Business Name or the ABN of the Buyer. | X | | |
| A Recipient Created Tax Invoice MUST contain the Payee Name if GST is payable. | X | | |
| A Recipient Created Tax Invoice MAY contain the following statement: “The recipient and the supplier declare that this agreement applies to supplies to which this tax invoice relates. The recipient can issue tax invoices in respect of these supplies. The supplier will not issue tax invoices in respect of these supplies. The supplier acknowledges that it is registered for GST and that it will notify the recipient if it ceases to be registered. The recipient acknowledges that it is registered for GST and that it will notify the supplier if it ceases to be registered for GST. Acceptance of this RCTI constitutes acceptance of the terms of this written agreement. Both parties to this supply agree that they are parties to an RCTI agreement. The supplier agrees to notify the recipient if the supplier does not wish to accept the proposed agreement within 21 days of receiving this document.” | | X | |

## Validation Rules - "ProfileID":"taxreceipt"
tbd

## Validation Rules - "ProfileID":"adjustment"
| Rule | Mandatory | Optional | Extension |
|------|-----------|----------| ----------|
| An Invoice MUST contain a Document Type Code indicating it is an adjustment document. | X | | |

## Validation Rules - "ProfileID":"creditnote"
| Rule | Mandatory | Optional | Extension |
|------|-----------|----------| ----------|
| A Credit Note MAY have a Related Invoice Identifier. | | X | |
| A Credit Note MAY have a Total Amount. | | X | |
| A Credit Note MAY have a Buyer Accounting Reference. | | X | |
| The Credit Note Total Amount MUST be greater than 0. | X | | |
| A Credit Note Line MAY have a Net Amount. | | X | |
| The Credit Note Line Net Price MUST NOT be negative. | X | | |
| A Payment Means Type Code for a Credit MUST have a Financial Institution Account Identifier. | X | | |

## Validation Rules - "ProfileID":"debitnote"
tbd
