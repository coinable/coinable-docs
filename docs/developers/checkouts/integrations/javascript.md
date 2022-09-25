---
sidebar_position: 0
---

# JavaScript

Coinable provides a one item checkout session endpoint bound per product id which can be used to initiate checkout sessions without a **api key**, the payment will be bound to the owner of the product. Typically used to integrate into simple websites, or when there is no back-end in the implementors website. With this seamless experience, both the implementer and Web3 user won't feel a difference.

## Endpoint

```
POST https://api.coinablepay/v1/api/checkouts/single
```

This endpoint requires the following payload in every call with self explanatory fields

```json
{
  "request_currency": "USD",
  "product_id": "tdWZE8pgBce7hsYBaTCemC",
  "quantity": 1
}
```

Optional fields are

```json
{
  "variant": "Black / L",
  "success_url": "https://google.com/",
  "cancel_url": "https://yahoo.com/"
}
```

- `variant` can be used if a variant of product is selected, variants are specified by their name as they are saved to by the variants fields on the product page, as an example `Black / L` is a variant where the color is Black and the size is L.
- `success_url` will redirect user to this url when successful payment has been made.
- `cancel_url` will redirect user to this url when user closes the checkout session or goes back by click the back arrow button on the checkout page.

## Usage Example

A basic example initating a checkout session in Vanilla JS using this endpoint could look something like this

```js
var raw = {
  // I want the checkout session to be presented in Solana token
  // so I am passing the Wrapped SOL mint here
  request_currency: 'So11111111111111111111111111111111111111112',
  product_id: 'tdWZE8pgBce7hsYBaTCemC',
  quantity: 1,
  cancel_url: 'https://product_page_url.com/',
  success_url: 'https://your_success_url.com/',
};

var requestOptions = {
  method: 'POST',
  headers: { 'Content-Type', 'application/json' },
  body: JSON.stringify(raw),
};

fetch('https://api.coinablepay.com/v1/api/checkouts/single', requestOptions)
  .then((response) => response.json())
  .then(function (result) {
    // Redirect user to the checkout session he initiated
    // or do anything else you want with the checkout session url you recieve
    window.location.href = result.redirect_url;
  })
  .catch((error) => console.log('error', error));
```

### Success response

A successful response will look like the following:

```json title="200 Success"
{
  "redirect_url": "https://coinablepay.com/checkout/9dGr8ceL4uQTi82SwZiUfk"
}
```

### Error response

An error resposne is also possible and will return the following format

```json title="400 Bad request"
{
  "errors": {
    "message": "No product found or requested quantity exceeds inventory."
  }
}
```

or

```json title="400 Bad request"
{
  "errors": {
    "[payload_field_name]": ["array", "of", "errors", "per", "field"]
  }
}
```
