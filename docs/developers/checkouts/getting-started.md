---
sidebar_position: 0
---

# Getting Started

The Checkout API allows any business to increase revenue-generating channels and remain competitive in the oncoming digital commerce revolution. Accept Solana token payments online with Coinable's hosted Checkout page with integration only a few steps away. Start accepting Solana eco-system tokens seamlessly, including USDC, from today.

**Checkout pages are fully customizable and optimized for mobile.**

Make sure to follow closely the steps outlined in the following paragraphs.

## Getting Started

In order to accept token payments a wallet address is required. If you do not have a wallet address, follow the steps outlined at [Phantom](https://phantom.app/) wallet.

Once you have a wallet address available, follow the steps described in the [Start here](/guides/start-here) documentation. Once completed, you will have a main wallet set for your project, and tokens whitelisted for payment. We recommend starting with **Wrapped SOL** and **USDC** as your first tokens.

:::note

Whitelisting **Wrapped SOL** will allow your customers to pay in _both_ native SOL, and wSOL, the tokenized version of SOL.

:::

## Initial Setup

### Private Access Key (API Key)

The API access key is required to access the Checkout session API. Your API access key is located in the [API Access](https://coinablepay.com/dashboard/api) tab in your Dashboard. Each project will be assigned a unique API key and should be kept secure.

## Create a checkout session

The following steps will guide you through creating a Checkout session on devnet.

### Checkout session endpoint

To create a checkout session you will create a `POST` request to the following endpoint. Replace `<YOUR_API_KEY>` with your own API key. An example running on devnet will be provided in the following section.

```
https://api.coinablepay.com/v1/api/checkouts?api_key=<YOUR_API_KEY>
```

### Checkout session parameters

`cancel_url` - The URL of the page that the customer will be directed to if they decide to cancel the checkout process.

`success_url` - The URL of the page that the customer will be directed to on a successful checkout. Additional params can be passed such as `{ORDER_NUMBER}`.

`request_currency` - You can specify the checkout currency both in a fiat representation, or a token representation using an appropriate mint.

`products` - An array of `Product` objects. The products array allows two possible ways to define a product object.

1. Use your products created in the Products page from the dashboard by adding`id`, the product ID, and `quantity`, the quantity of the product.

```json
{
  "id": "FRD6rFSPLHfVoziyzY4qun",
  "quantity": 1
}
```

2. An inline product which can be created on demand with the following fields:
   - `price` - The price of the product.
   - `quantity` - The quantity ordered.
   - `title` - The name of the product.
   - `description` - An optional product description.
   - `image_url` - An optional product image. A placeholder will be used if no URL is provided.
   - `currency` - An optional currency mint. If not provided, checkout currency will be used instead.

`shipping_options` - optional. An array of `ShippingOption`, which defines the shipping options available to the customer. If left undefined, shipping is not needed for the respective product, e.g. a game key.

- `price` - The cost of shipping the product.
- `display_name` - The name of the shipping method. e.g. "Free shipping"
- `delivery_estimate` - An object `DeliveryEstimate` responsible for defining the min and max values of the shipping.
- `min_unit` - The minimum unit of time, defaults to `business_day` (Not editable at the moment).
- `max_unit` - The maximum unit of time, defaults to `business_day` (Not editable at the moment).
- `min_value` - The numerical representation of the minimum unit until shipping.
- `max_value` - The numerical representation of the maximum unit until shipping.
- `type` - optional. Type of payment, defaults to `fixed_amount`(Not editable at the moment).

`custom_fields` - optional - An array of `CustomField`, which defines a custom field on your checkout form, which can be used for custom data that may be required for your business.

- `required` - optional. Defaults to `false`. If set to `true`, the custom field will be required in order to continue the checkout.
- `title` - the title of your field which will be used as a title in the checkout form.
- `key` - the key under which the value of this field will be saved. The `key` must be unique, otherwise values will be overidden. The `key` also represents the field under the Order details, which takes the form of `key_name` -> `Key name`.

`metadata` - optional. We provide developers with a custom metadata field which can be populate `key: value` data, for further usage, consumption on the server side. Here is an example of possible metadata that can be passed and recieved later through the webhook via event.

Example metadata field:

```json
{
  ...
  "metadata": {
    "user_id": "abcdefg",
    "another_field": "abcdefg"
  }
}
```

### Creating a checkout session

Lets initiate a new checkout session with what we've learned so far. We will pass the two formats of products accepted by the products array. First, will be using the product created through Coinable, and second will inline a product created externally.

```json title="Checkout session creation"

curl --location --request POST 'https://api.coinablepay.com/v1/api/checkouts?api_key=YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "cancel_url": "https://coinablepay.com/cancel_order.html",
  "success_url": "https://coinablepay.com/success?order_number={ORDER_NUMBER}",
  "request_currency": "USD",
  "products": [
      {
          "id": "FRD6rFSPLHfVoziyzY4qun",
          "quantity": 1
      },
      {
          "price": 15,
          "quantity": 3,
          "title": "Title of product"
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
          "price": 25,
          "type": "fixed_amount"
      }
  ]
}'

```

A JSON-encoded success response will have a single field `redirect_url` which will be the link directing to the checkout session on Coinable. This URL can be linked to a checkout button.

```json title="200 Success"
{
  "redirect_url": "https://coinablepay.com/checkout/3qXPdcTkFjaQyfEn48FWfn"
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

Now we need to set up the applications webhook which is going to listen for notifications related to checkout events.

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

### Coinable webhook checkout events

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
