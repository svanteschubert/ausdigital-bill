# Billing Process

## Invoice Document Profiles

The Invoice document is used in six different process contexts, indicated using the UBL ProfileID.

 * Standard invoice from seller to buyer ("ProfileID":"dbc:invoice")
 * Updated standard invoice from seller to buyer that replaces a pervious invoice of the same ID ("ProfileID":"dbc:adjustment")
 * Recipient created tax invoice from buyer to seller ("ProfileID":"dbc:rcti")
 * Tax receipt sent from seller to buyer after payment has been made - usually for POS or online purchases ("ProfileID":"dbc:taxreceipt")
 * Credit note sent from seller to buyer that references an earlier standard invoice ("ProfileID":"dbc:creditnote")
 * Debiit note sent from buyer to seller that references an earlier RCTI ("ProfileID":"dbc:debitnote")

The detailed business vlaidation rules for each invoice profile are defined in the billing rules specification.

## Document Response Codes

A UBL document response provides a means for the receiver party to update the sender on the processing state of the invoice.  The set of valid document response codes depends on the business process identified by the ProcessID (which is the same as the customizationID in UBL instances) are

 * Acknowledged - confirms receipt of the invoice (but does not imply approval to pay).
 * Approved - means that the payer has approved the invoice for (future) payment in accordance with payment terms.
 * Disputed - means that the payer has not accepted the inovice and will dispute some or all of the invoice.
 * Rejected - means that the payer has rejected the entire invoice and will not be paying.  

## State Lifecycle

The diagram shows the allowed set of states for an invoice as understood by both parties in the collaborative process.  Every transition from one state to another is triggered by the echange of a business message - which could be either an invoice (one of six profiles) or a response document (with one of four response codes).

![Billing State Lifecycle](Billing-StateLifecycle.png)

 * The TaxReceipt has the simplest lifecycle because it is just a record of a previous payment and so there is only a single invoice (profileID=taxreceipt) sent from seller to buyer and the state is "paid".  No document response needed.
 * The receipt of an RCTI (profileID=rcti) by a seller from a buyer takes the state directly to "approved" because this is a payer initiated transaction.  However the payer may subsequently send an invoice (profileID=debitnote) to make an adjustment to the rcti prior to eventual payment is accordance with payment terms.
 * The standard inovice that is a demand for future payment from seller to buyer is the most complex lifecycle becuase there can be 
  - various response documents indicating buyer processing status (acknowledged, approved, disputed, rejected)
  - a re-issue of the invoice with changes (profileID = adjustment) from seller to buyer - usually in response to a disputed status.
  - an issue of a credit note against an existing invoice (profileID = creditnote) - also usually in response to a disputed status.
  - an outright rejection of the invoice by the buyer which leads to a cancelled end-state.
 * The "success" end state in all cases is "paid" - indicated by the receipt of a payment record from a bank reconcilation file (outside of the scope of this specificaiton).

## Validation Rules

Validation rules from the [DBC e-invoice semantic model](https://github.com/ausdigital/dbc-specs/blob/master/eInvoicing_Semantic_Model_v1.0.pdf) are re-stated here but are broken down according to the invoice document usage context as identified by the ProfielID element.  

### Validation Rules - Common
| Rule | Comment |
|------|---------|
| An Invoice of more than $82.50 (including GST) to a GST-registered Buyer MUST be a Tax Invoice. | |
| An Invoice must contain a Document Type Code | |
| An Invoice MUST contain the Supplier’s Business Name or the ABN of the Supplier. | |
| An Invoice with a Total Amount greater than $1000 MUST have either the Buyer's Business Name or the ABN of the Buyer. | |
| An Invoice MAY contain the ABN plus a GST branch number for Suppliers with GST branches registered with the ATO. | |
| An Invoice MUST contain an Invoice Issue Date. | |
| An Invoice Line MUST have a Description.  | |
| An Invoice MAY contain a Description of Properties of Invoiced Items.  | |
| An Invoice Line MAY contain an Invoiced Quantity.  | |
| An Invoice Line MUST contain the Invoice Line Extension Amount (Net Price multiplied by Invoiced Quantity) (excluding GST) for the Items sold.  | |
| An Invoice MUST contain the sum total of all Invoice Line Extension Amounts. | |
| An Invoice Line MUST contain the GST Amount for the Items sold or indicate the extent to which Items are taxable. | |
| An Invoice Line MUST contain the Amount Payable (Invoice Line Extension Amount plus GST Amount) for the Items sold. |  |
| An Invoice Line MAY contain a GST Amount of zero. |  |
| An Invoice Line MAY specify a GST Category. |  |
| An Invoice MUST contain the Invoice level Tax Amount exclusive of GST. |  |
| An Invoice MUST contain the Invoice level GST Total Amount. |  |
| An Invoice MUST have an Invoice Identifier. |  |
| An Invoice MUST have a Supplier Business Name. |  |
| An Invoice MUST have a valid Document Type Code. |  |
| An Invoice MUST have an Issue Date. |  |
| An Invoice MAY have a Delivery Date. | |
| An Invoice MAY have an Invoice Period. |  |
| An Invoice MAY have an Invoice Period Start Date. |  |
| An Invoice MAY have an Invoice Period End Date. |  |
| An Invoice End Period Date MUST be later or equal to an Invoice Period Start Date | |
| An Invoice MAY have a Sales Order Identifier. | |
| An Invoice MAY have a Purchase Order Identifier. |  |
| An Invoice MAY have a Contact Identifier. |  |
| An Invoice MAY have an Electronic Address. | |
| An Invoice MUST have at least one Invoice Line. |  |
| An Invoice Line Item MAY have a Suppliers Item Identifier. |  |
| An Invoice Line Item MUST have a Description. |  |
| An Invoice Line MAY have a Quantity. |  |
| An Invoice Line MAY have a Net Amount. |  |
| An Invoice Line MAY have a Dispatch Advice Identifier. |  |
| An Invoice Line MAY have a Receipt Advice Identifier. | |
| An Invoice Line MAY have a delivery Address. |  |
| An Invoice Line Item MAY have a Country of Origin. |  |
| An Invoice MAY have one or more Document References. |  |
| The Invoice Level Net Amount MUST be equal to the sum of Invoice Line Net Amounts. |  |
| The Invoice Level Allowance Amount MUST be equal to the sum of Invoice Line Allowances plus any Invoice Level Allowances.| |
| The Invoice Level Charge Amount MUST be equal to the sum of Invoice Line Charges plus any Invoice Level Charges. |  |
| The Invoice Level Net Amount MUST be equal to the Invoice Level Gross Amount - Invoice Level Allowance Amount + Invoice Level Charge Amount. |  |
| The Invoice Level GST Amount MUST be equal to the sum of Invoice Line GST Amounts. |  |
| The Invoice Level Total Amount MUST be equal to the Invoice Level Net Amount + the Invoice Level Tax Amount. | |
| An Invoice MAY have an Amount Payable. | |
| An Invoice MUST have an Invoice Level Total Amount. |  |
| An Invoice MAY have an Invoice Level Net Amount. |  |
| An Invoice MAY have a Related Invoice Identifier. |  |
| An Invoice MAY have an Amount Payable. |  |
| An Invoice MAY have one or more Document References. |  |
| An Invoice Level Total Amount MUST be greater than 0. |  |
| An Invoice Line MAY have a Net Amount. |  |
| An Invoice Line Extended Amount after all allowances and charges MUST NOT be negative. |  |
| An Invoice Line Price MUST be 0 or more. |  |
| An Invoice Line Item MUST have a Net Price. |  |
| An Invoice Line MAY have a Quantity. |  |
| An Invoice MAY have an Allowance Rate and Base Amount at Invoice Level. |  |
| An Invoice Level Allowance MUST be greater than 0. |  |
| An Invoice Level Allowance MAY have a GST Category. |  |
| An Invoice Level Allowance Reason Description MUST match the Invoice Level Allowance Reason Code (if any). |  |
| An Invoice MAY have a Charge Rate and Base Amount at Invoice Level. |  |
| An Invoice Level Charge MUST be greater than 0. |  |
| An Invoice Level Charge MAY have a GST Category. |  |
| An Invoice Level Charge Reason Description MUST match the Invoice Level Charge Reason Code (if any). |  |
| An Invoice Line MAY have an Allowance Rate and Base Amount. |  |
| An Invoice Line Allowance MUST be greater than 0. |  |
| An Invoice Line Allowance MUST have an Allowance Reason Description. |  |
| An Invoice Line Allowance Reason Description MUST match the Invoice Line Allowance Reason Code (if any). |  |
| An Invoice Line MAY have a Charge Rate and Base Amount. |  |
| An Invoice Line Charge MUST be greater than 0. |  |
| An Invoice Line Charge MUST have a Charge Reason Description. |  |
| An Invoice Line Charge Reason Description MUST match the Invoice Line Charge Reason Code (if any). |  |
| A Payment Means MUST have a valid Payment Means Type Code. |  |
| A Payment Means Financial Institution Account Identifier MUST have Financial Institution Identifier. |  |
| A Payment Means for a card payment MUST state the last 4 to 6 digits of the Financial Institution Account Identifier. |  |
| An Invoice MUST have a Payee Business Name if Payee Business Name is not the same as the Suppliers Business Name. |  |

### Validation Rules - "ProfileID":"Invoice"
| Rule | Comment |
|------|---------|
| A Tax Invoice for goods or services that do not all include GST (mixed supplies) shall indicate which goods or services do not include GST. |  |

### Validation Rules - "ProfileID":"rcti"
| Rule | Comment |
|------|---------|
| A Recipient Created Tax Invoice MUST contain either the Business Name or the ABN of the Buyer. |  |
| A Recipient Created Tax Invoice MUST contain the Payee Name if GST is payable. |  |
| A Recipient Created Tax Invoice MAY contain the following statement: “The recipient and the supplier declare that this agreement applies to supplies to which this tax invoice relates. The recipient can issue tax invoices in respect of these supplies. The supplier will not issue tax invoices in respect of these supplies. The supplier acknowledges that it is registered for GST and that it will notify the recipient if it ceases to be registered. The recipient acknowledges that it is registered for GST and that it will notify the supplier if it ceases to be registered for GST. Acceptance of this RCTI constitutes acceptance of the terms of this written agreement. Both parties to this supply agree that they are parties to an RCTI agreement. The supplier agrees to notify the recipient if the supplier does not wish to accept the proposed agreement within 21 days of receiving this document.” |  |

### Validation Rules - "ProfileID":"taxreceipt"
tbd

### Validation Rules - "ProfileID":"adjustment"
| Rule | Comment |
|------|---------|
| An Invoice MUST contain a Document Type Code indicating it is an adjustment document. |  |

### Validation Rules - "ProfileID":"creditnote"
| Rule | Comment |
|------|---------|
| A Credit Note MAY have a Related Invoice Identifier. | |
| A Credit Note MAY have a Total Amount. |   |
| A Credit Note MAY have a Buyer Accounting Reference. |   |
| The Credit Note Total Amount MUST be greater than 0. |   |
| A Credit Note Line MAY have a Net Amount. |   |
| The Credit Note Line Net Price MUST NOT be negative. |   |
| A Payment Means Type Code for a Credit MUST have a Financial Institution Account Identifier. |   |

### Validation Rules - "ProfileID":"debitnote"
tbd
