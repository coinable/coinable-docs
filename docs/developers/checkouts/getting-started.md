---
sidebar_position: 0
---

# Getting Started

The Checkout API allows any business to increase revenue-generating channels and remain competitive in the oncoming digital commerce revolution. Accept Solana token payments online with Coinable's self-hosted Checkout page with integration only a few steps away. Start accepting Solana eco-system tokens, including USDC, from today.

Make sure to follow closely the steps outlined in the following paragraphs.

## Getting Started

In order to accept token payments a wallet address is required. If you do not have a wallet address, follow the steps outlined at [Phantom](https://phantom.app/) wallet.

Once you have a wallet address available, follow the steps described in the [Start here](http://localhost:3000/products/start-here) documentation. Once completed, you will have a main wallet set for your project, and tokens whitelisted for payment. We recommend starting with **Wrapped SOL** and **USDC** as your first tokens.

:::note

Whitelisting **Wrapped SOL** will allow your customers to pay in _both_ native SOL, and wSOL, the tokenized version of SOL.

:::

## Initial Setup

### Private Access Key (API Key)

The API access key is required to access the Checkout session API. Your API access key is located in the [API Access](https://coinable.dev/dashboard/api) tab in your Dashboard. Each project will be assigned a unique API key and should be kept secure.

## Create a Checkout session

The following steps will guide you through creating a Checkout session on devnet.

### Checkout session endpoint

To create a checkout session you will create a `POST` request to the following endpoint. Replace `<YOUR_API_KEY>` with your own API key. An example running on devnet will be provided in the following section.

```
https://api.coinable.dev/v1/api/checkouts?api_key=<YOUR_API_KEY>
```

### Checkout session parameters

`cancel_url` - The URL of the page that the customer will be directed to if they decide to cancel the checkout process.

`success_url` - The URL of the page that the customer will be directed to on a successful checkout. Additional params can be passed such as `{ORDER_NUMBER}`.

`request_currency` - Optional field, You can set the amount in a wide range of currencies such as `HKD`, `ILS`, `USD` and many more. If not set, defaults to currency set in the Settings tab.

`items` - An array of `Item` objects where the `Item` object is defined as having the following members.

- `price` - The price of the product.
- `quantity` - The quantity ordered.
- `display_name` - The name of the product.
- `description` - An optional product description.
- `image_url` - An optional product image. A placeholder will be used if no URL is provided.

`shipping_options` - optional. An array of `ShippingOption`, which defines the shipping options available to the customer. If left undefined, shipping is not needed for the respective product, e.g. a game key.

- `price` - The cost of shipping the product.
- `display_name` - The name of the shipping method. e.g. "Free shipping"
- `delivery_estimate` - An object `DeliveryEstimate` responsible for defining the min and max values of the shipping.
  - `min_unit` - the minimum unit of time, defaults to `business_day` (Not editable at the moment).
  - `max_unit` - the maximum unit of time, defaults to `business_day` (Not editable at the moment).
  - `min_value` - the numerical representation of the minimum unit until shipping.
  - `max_value` - the numerical representation of the maximum unit until shipping.
- `type` - optional. Type of payment, defaults to `fixed_amount`(Not editable at the moment).
- `currency` - optional. The stable coin in which the payment is based, defaults to `USDC` (Not editable at the moment).

### Creating a Checkout session

Send a request to the Checkout API on devnet using the following command.

```json title="Checkout session creation"
curl --location --request POST 'https://api.coinable.dev/v1/api/checkouts?api_key=5600324db80841548a360f44b851fcf4' \
--header 'Content-Type: application/json' \
--data-raw '{
  "cancel_url": "https://<mywebsite.com>/cancel_order.html",
  "success_url": "https://<mywebsite.com>/success?order_number={ORDER_NUMBER}",
  "items": [
    {
      "price": 15,
      "quantity": 2,
      "display_name": "Black summer slippers"
    },
    {
      "price": 49.99,
      "quantity": 1,
      "display_name": "Blue sunglasses with chromatic texture"
    }
  ],
  "shipping_options": [
    {
      "delivery_estimate": {
        "max_value": 2,
        "min_value": 1
      },
      "display_name": "Free shipping",
      "price": 0,
      "type": "fixed_amount"
    },
    {
      "delivery_estimate": {
        "max_value": 1,
        "min_value": 1
      },
      "display_name": "Same day shipping",
      "price": 25.99,
      "type": "fixed_amount"
    }
  ]
}'
```

A JSON-encoded success response will have a single field `redirect_url` which will be the link directing to the Checkout session on Coinable. This URL can be linked to a checkout button.

```json title="200 Success"
{
  "redirect_url": "https://coinable.dev/checkout/3qXPdcTkFjaQyfEn48FWfn"
}
```

If a field is missing from a request, the `400` response will contain information on the missing fields.

```json title="400 Bad request"
{
  "errors": {
    "success_url": ["can't be blank"]
  }
}
```

## Webhook setup

Now we need to set up the applications webhook which is going to listen for notifications related to Checkout events.

```javascript title="Webhook example"
const app = require('express')();

// Just some parser that parses raw json
const bodyParser = require('body-parser');

app.post(
  '/webhook',
  bodyParser.raw({ type: 'application/json' }),
  (request, response) => {
    const payload = request.body;

    console.log('Got payload: ' + payload);

    // Handle payload data accordingly

    response.status(200);
  }
);
```

### Coinable webhook Checkout events

Coinable can notify your application of the following events.

| Event code                         | Description                                                                                                                                                                                                                                                      |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `checkout.session.completed`       | Checkout is complete and tokens have been transferred to the merchant's wallet.                                                                                                                                                                                  |
| `checkout.session.payment_attempt` | There was a **payment attempt** in the Checkout session which doesn't mean that the payment was completed. **No tokens have been transferred to the merchant accounts yet.** Some developers choose to ignore this event, some use it for internal data metrics. |
| `checkout.session.failed`          | Occurs when there is a mismatch between the checkout amount and transfer amount. Contact customer for further investigation. Typically happens due to a malicious payment attempt.                                                                               |

### Improving the webhook example

We can improve the previous example by handling the different events delivered to the applications webhook.

```javascript title="Improved webhook example"
const fulfillOrder = (data) => {
  // TODO: fulfilled order
  console.log('Fulfilling order', data);
};

app.post(
  '/webhook',
  bodyParser.raw({ type: 'application/json' }),
  (request, response) => {
    const payload = request.body;

    // Handle the checkout.session.completed event
    if (payload.type === 'checkout.session.completed') {
      const data = payload.data;

      // Fulfill the purchase...
      fulfillOrder(data);
    } else if (payload.type === 'checkout.session.failed') {
      // addFailedPurchaseAttempt(payload.data);
    }

    response.status(200);
  }
);
```
