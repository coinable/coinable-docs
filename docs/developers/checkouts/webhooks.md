---
sidebar_position: 2
---

# Webhooks

At the time of writing there is two categories of available webhooks

1. Event Based Webhoks
2. Integration Webhooks

## Event Based Webhooks

This is the webhook that is shown on our Getting started with Checkout API guide, where your app can be notified about all the states of each checkout initiated.

The following events are emitted

| Event code                         | Description                                                                                                                                                                                                                                                      |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `checkout.session.completed`       | Checkout is complete and tokens have been transferred to the merchant's wallet.                                                                                                                                                                                  |
| `checkout.session.payment_attempt` | There was a **payment attempt** in the Checkout session which doesn't mean that the payment was completed. **No tokens have been transferred to the merchant accounts yet.** Some developers choose to ignore this event, some use it for internal data metrics. |
| `checkout.session.failed`          | Occurs when there is a mismatch between the checkout amount and transfer amount. Contact customer for further investigation. Typically happens due to a malicious payment attempt.                                                                               |

Please go to [Getting started](/developers/checkouts/getting-started) to learn more about possible implementation.

## Integration Webhook

### Discord

Currently we support only Discord based webhooks that can be setup in the following form:

1. Create a text channel where you want the Bot to notify when new payment occurres.
2. Click on the cog next to your channel name (Edit channel)
3. Click on Webhooks
4. Create a new webhook, give it a name of your choice and optionally an avatar.
5. Copy Webhook URL
6. Pase the Webhook URL into the DISCORD WEBHOOK ENDPOINT field in your Api Access tab on Coinable.

You are all good to go the Bot will now feed any data coming from the Checkouts, where its a Storefront or an API intiated checkout session you will get notified in the following form.

Example

```
[New project]: New successful payment for 0.86 SOL for simple test product.
```

More integrations like Slack to come soon.
