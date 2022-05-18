---
sidebar_position: 0
---

# Checkouts

Checkouts are the essential part of every business that wants to generate revenue and accepting payments.
Coinable provides a Web3 checkout integration which helps buisnesses accept token based payments by integrating via our API. With Coinables
Checkout API every business can expand their revenue making channels by starting accepting Solana eco-system tokens, such as USDC (Stable coin that is pegged to $1 at all times) and SOL.

Please do not skip any parts of the tutorial so no missing data or step will occur in the process.

## Getting Started

To start accepting payments we need to setup ourselves a wallet, If you don't already have a wallet we recommend
going with the [Phantom](https://phantom.app/) wallet, which is easy enough to setup by following the
steps described on their website.

After you have done setting up our wallet, you need to follow our [Start here](http://localhost:3000/products/start-here) article
and whitelist yourself the tokens you want to accept. We recommend going with **Wrapped SOL** and **USDC** as your first tokens,
you can add more tokens later after you've done your own research.

## Initial Setup

Before we can start using the API we will need to sort out 2 things.

1. How do we acces the Checkout API and call the endpoints it provides
2. Setup a webhook on our server to handle different events emitted by the checkout session

### Private Access Key (API Key)

First thing first, API access key is used to access the checkout session creation API.
Your API is located in the [API Access](https://coinable.dev/dashboard/api) tab in your Dashboard.

You will be greeted with 2 fields once you are in the API Access tab

- API Key
- Webhook Endpoint (We will talk about this in a bit)

### Get your api key

Click on the button `Click to see API key` in the [API Access](https://coinable.dev/dashboard/api) tab in your dashboard, your key should appear. This is the key that you will use to access Checkouts API. Please keep it somewhere
safe and never share it with anyone else. This one of the things you usually keep as an enviornment variable and never add to your git commit history.

Copy over your api key to somewhere safe, will use this access key shortly.

## Create your first checkout session

Now we will continue to the creation of our first ever checkout session, We are one step closer with providing our customers with token payment gateway 🎉.

### Checkout session creation endpoint

To create your first checkout session you will create a `POST` request to the following endpoint, replace the `<YOUR_API_KEY>` with your own api
key you copied earlier. If you haven't copied your key yet, you should be able to do that by following [this guide](/developers/checkouts-intro#get-your-api-key).

```
https://api.coinable.dev/v1/api/checkouts?api_key=<YOUR_API_KEY>
```

Checkout session creation endpoint accepts the following data to be sent along side with it.
Now, we will explain each field and its responsibility.
After that we will show a payload example that can be sent along side the request.

### Checkout session creation payload parameters

`cancel_url` - Url page which the user will be redirected to if he decides to cancel the checkout or go back to your store, maybe to add some more items.

`success_url` - Once user has successfuly paid for the invoice, the user will be redirected to this page. This param can optionally accept a checkout session id as such `{ORDER_NUMBER}`. You will see an example later in the documentation once we will send a payload example.

`items` - This is an array of objects `Item`, which define the items from your customer cart.

- `price` - a price of the item the user selected in the cart.
- `quantity` - amount of the item that the user has selected.
- `display_name` - the title of the item.
- `description` - an optional description for the item.
- `image_url` - opitonal image of the item, a placeholder will be used if no url is provided.

`shipping_options` - an array of objects `ShippingOption`, which define the shipping options available for the customer. Optional field, if no field not added item doesn't require shipping e.g. a game key.

- `price` - the cost of shipping the items to the customer.
- `display_name` - the title of the shipping method for example "Free shipping"
- `delivery_estimate` - an object `DeliveryEstimate` responsible for defining the min and max values of the shipping.
  - `min_unit` - the minimum unit of the estimation value, defaults to `business_day` (Not editable at the moment).
  - `max_unit` - the maximum unit of the estimation value, defaults to `business_day` (Not editable at the moment).
  - `min_value` - the numerical representation of minimum days until shipping.
  - `max_value` - the numerical representation of maximum days until shipping.
- `type` - optional the type of payment, defaults to `fixed_amount`(Not editable at the moment).
- `currency` - optional the currency in which the payment is calculated, defaults to `USDC` (Not editable at the moment).

### Creating the first checkout session

Lets build our first `curl` and send the request to Checkouts API, you could use Postman which may be more comfortable
for some of you, or do a direct call through your code with your http client, its up to you really.

```json title="Checkout session creation"
curl --location --request POST 'localhost:4000/v1/api/checkouts?api_key=5600324db80841548a360f44b851fcf4' \
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
      "price": 20.0,
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

Once called successfully you should get a response with a single field `redirect_url` which is the link redirecting to the checkout session on Coinable
This is the link you should provide to the user which tries to checkout once he clicked the "Pay with Coinable" or however you want to call it button.

```json title="200 Success"
{
  "redirect_url": "https://coinable.dev/checkout/3qXPdcTkFjaQyfEn48FWfn"
}
```

If you forget any of the fields required for session creation the response will return a `400` with description of whats missing e.g.

```json title="400 Bad request"
{
  "errors": {
    "success_url": ["can't be blank"]
  }
}
```

## Webhook setup

Now we need to setup our webhook which is going to listen to events and act accordingly to what it receives, a typical webhook would look like this.

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

    // Handle payload data accordingly, maybe call a function
    // to remove 1 item from the database and add the item to the user

    response.status(200);
  }
);
```

### Possible webhook checkout events

Lets get familiar with the events that are available for the Checkouts API. You can handle each event in your code however you see fit.

| Event code                         | Description                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `checkout.session.completed`       | Checkout is complete, money is transferred to the merchants wallet.                                                                                                                                                                                                                                                                                                         |
| `checkout.session.payment_attempt` | There was a **payment attempt** in the checkout session this doesn't mean that the payment was completed. **No tokens has been transferred to the merchant accounts yet.** Some developers choose to ignore this event, some use it for internal data metrics.                                                                                                              |
| `checkout.session.failed`          | A customer has tried to pay the checkout session, Solana blockchain has accepted the transaction and some tokens may have been transferd to the merchants wallet address, but the amount of tokens paid by the customer is not the same as what the checkout has requested. Contact customer for farther investigation. Typically happens due to malicious payment attempt. |

### Improving our webhook

So now that we know what type of events are emitted to us, our webhook now can look like this.
It will execute the appropriate block of code depending on what has been emitted.

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
      // or whatever works for you
    }

    response.status(200);
  }
);
```
