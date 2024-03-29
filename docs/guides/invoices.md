---
sidebar_position: 2
---

# Create an invoice

Coinable's invoice service allows merchants to invoice customers and accept any tokens whitelisted by the merchant. Customers will be able to pay through **a single interface** regardless of if they are paying with native SOL or a whitelisted token with **payments settled into the merchant's wallet instantly**.


:::note

Any native SOL payments will be converted to the tokenized version wSOL to be processed on chain. There is a 1 to 1 relationship, and you will be able to convert wSOL to native SOL at your convenience.

:::


## Create an Invoice

Users will be able to create invoices from the dashboard with a single click.

<div style={{textAlign: 'center', padding: '20px'}}>

![One Click Start](/img/guides/create-invoice.png)

</div>


Specify the amount to invoice, populate the required fields, including discounts and tax rates if applicable, and click **Create**. Share the unique link to a Coinable hosted invoice page with your customer for payment. That's it.

<div style={{textAlign: 'center', padding: '20px'}}>

![Create Invoice](/img/guides/create-invoice-2.png)

</div>

## Customer View

Once the customer accesses the link, they will have the option to select a whitelisted token to use for payment. The fiat equivalent amount of the selected token will be transferred to your wallet instantly.

<div style={{textAlign: 'center', padding: '20px'}}>

![Customer View](/img/guides/customer-view-2.png)

</div>


## Dashboard

The dashboard lets you track the state of the invoice from pending to fulfilled, and raise a notification if the invoice is past due.

<div style={{textAlign: 'center', padding: '20px'}}>

![Dashboard Pending](/img/guides/dashboard-pending.png)

</div>

Once the invoice has been fulfilled the dashboard will indicate which token was used for payment and the amount transferred.

<div style={{textAlign: 'center', padding: '20px'}}>

![Dashboard Fulfilled](/img/guides/dashboard-fulfilled.png)

</div>