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

## API Usage

Before we cant start using the API we will need to sort out 2 things.

1. How do we acces the Checkout API and call the endpoints it provides
2. Setup a webhook on our server to handle different events emitted by the checkout session

### Private Access Key (API Key)

API access key is used to access the checkout session creation API.
Your API is located in the [API Access](https://coinable.dev/dashboard/api) tab in your Dashboard.

You will be greeted with 2 fields once you are in the API Access tab

- API Key
- Webhook Endpoint (We will talk about this in a bit)

Click on the button `Click to see API key` and your key should appear. This is the key that you will use to access Checkouts API. Please keep it somewhere
safe and never share it with anyone else. This one of the things you usually keep as an enviornment variable and never add to your git commit history.

Copy over your api key to somewhere safe, will use this access key shortly.

### Webhook setup

Now we need to setup our webhook which is going to listen to events and act accordingly to what it receives, a typical webhook would look like this

```javascript
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

```javascript
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

## Create your first checkout session

Now that we have our webhook setup so we can intercept different events and act accordingly to them we can now explore the session creation API
