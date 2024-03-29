---
sidebar_position: 1
---

# On Behalf Of (OBO) System

Any platform can add a Web3 payments layer and earn fees on transactions done on the platform by integrating Coinable in just a few steps.

1. The users of a given platform need to register with Coinable and set a wallet that will be used to accept payments through the Settings page.
2. The users will need to whitelist the same tokens that are whitelisted by the platform.
3. The users need to be able to share their project key with the platform which will be passed to `origin_project_key` field along with the desired `service_charge_rate` in percentage terms (e.g. 0.5% is passed as 0.5).
4. It is the platform's responsibility to persist the `origin_project_key` and initiate
   new checkout sessions per user request.

```json
 "on_behalf_of": {
        "origin_project_key": "4Mmnho8Ys9hzkDBE3cdbWh",
        "service_charge_rate": 0.5
    }
```

A typical flow with your new request may look like the following

```json
{
  "cancel_url": "https://coinablepay.com/cancel_order.html",
  "success_url": "https://coinablepay.com/success?order_number={ORDER_NUMBER}",
  "items": [
    {
      "price": 50,
      "quantity": 1,
      "display_name": "Gucci Stainless-Steel Glasses 2019 Limited Edition"
    },
    {
      "price": 30,
      "quantity": 1,
      "display_name": "Balenciaga Jacket VN-2012"
    }
  ],
  "on_behalf_of": {
    "origin_project_key": "4Mmnho8Ys9hzkDBE3cdbWh",
    "service_charge_rate": 0.5
  }
}
```