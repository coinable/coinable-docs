---
sidebar_position: 2
---

# Webhooks

There are two categories of webhooks available.

1. Event Based Webhooks
2. Integration Webhooks

## Event Based Webhooks

This is the webhook that is discussed in the [Getting Started](https://docs.coinablepay.com/developers/checkouts/getting-started) with Checkout API guide, where your application can be notified about all checkout related events.

The following events are emitted:

| Event code                         | Description                                                                                                                                                                                                                                                      |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `checkout.session.completed`       | Checkout is complete and tokens have been transferred to the merchant's wallet.                                                                                                                                                                                  |
| `checkout.session.payment_attempt` | There was a **payment attempt** in the Checkout session which doesn't mean that the payment was completed. **No tokens have been transferred to the merchant accounts yet.** Some developers choose to ignore this event, some use it for internal data metrics. |
| `checkout.session.failed`          | Occurs when there is a mismatch between the checkout amount and transfer amount. Contact customer for further investigation. Typically happens due to a malicious payment attempt.                                                                               |

## Integration Webhooks

### Discord

Currently we support only Discord based webhooks, and can be set up follow the below steps:

1. Create a text channel where the Bot will direct payment notifications.
2. Click on the cog next to your channel name (Edit Channel).
3. Go to `Integrations` and click on `Webhooks`.
4. Create a `New Webhook`, give it a name of your choice and optionally an avatar.
5. Click `Copy Webhook URL` once details are set.
6. Paste the Webhook URL into the `DISCORD WEBHOOK ENDPOINT` field in your Api Access tab on Coinable dashboard.

You are all set to go. The Bot will now update the created channel with payments received through all Checkouts completed through Coinable.

##### Example

```
[New project]: New successful payment for 0.86 SOL for simple test product.
```

More integrations coming soon.
