---
sidebar_position: 3
---

# Integrations

Coinable will offer variaty of possible plug and play integrations for already existing storefronts, websites, or any business that wants to accept crypto in a matter of minutes.

## JavaScript

TODO

## React

Currently we offer a React based component integration that will let you setup a pay button anywhere you want it in your app with the ability to style it inlined with your website, see [Customization](sdsd) section for more info.

<div style={{textAlign: 'center', padding: '20px'}}>

![Settings Wallet Address](/img/guides/react-integration-button.png)

</div>

### Usage

Integration example for the React based pay button component is as follows

```js
<PayButton
  productId="KvD8sieX9t48Lv7AMEwo2b"
  onFailure={(msg) => console.error(msg)}
  onSuccess={(url) => {
    window.location.href = url;
  }}
/>
```

1. Where `productId` we pass the product id of the product we setup in our Dashboard via the [Products](https://coinablepay.com/dashboard/products) tab.
2. Then we have two callbacks that we need to implement
   - `onSuccess` returns the checkout session url for our user to be redirected too and pay for the product.
   - `onFailure` returns an error message on why did the checkout session initiation has failed.

### Example

An example of possible implementation in your React app could be:

```js
import PayButton from '@coinable/pay-button';

import './App.css';

function App() {
  const handleOnSuccess = (url: string) => {
    window.location.href = url;
  };

  const handleOnFailure = (msg: string | undefined) => {
    setError(msg);
  };

  return (
    <div className="App">
      <img
        src="/degen-cat.png"
        width="150"
        height="150"
        className="degen-cat"
        alt="cover"
      />
      <span>Degen - $5.00</span>
      <PayButton
        productId="EKbTyXz5cPCspocqbRvFrE"
        onFailure={handleOnFailure}
        onSuccess={handleOnSuccess}
      />
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}

export default App;
```

Our final integration looks like this, **just in 33 lines of code**:

<div style={{textAlign: 'center', padding: '20px'}}>

<video width="100%" height="auto" controls>

<source src="/videos/guides/react-example.mp4" type="video/mp4" />
</video>

</div>

### Customization

For customiztion of our freshly created buttons there are two approaches at the moment giving you the max possible styling ability.

#### CSS/SCSS

Override the `coinable-pay-button` className and change whatever CSS you want there.

```js
    <PayButton ... className="my-own-classname" />
```

#### Styled Components

```js
const StyledPayButton = styled(PayButton)`
  transition: 150ms linear;
  background: blue;
  border-radius: 16px;

  &:hover {
    transform: scale(1.05);
  }
`;
```

#### Customization Props

If you don't want to touch the underlying CSS of the pay button component we allow to change the basic styling like color of your button, so find it sufficient for their need, the available props are `backgroundColor` and `textColor`. See example below.

```js
    <PayButton ... backgroundColor="#FFF" textColor="#000" />
```

### Props

Available props for the component are

| Prop name          | Description                                                                                                                                                 |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `children?`        | If you want a different content inside of the pay button, instead of the default Pay text.                                                                  |
| `productId`        | The product id you get from the product created through Coinable.                                                                                           |
| `onSuccess`        | Callback which passes the url as the parameter which is used to redirect user to his generated checkout session.                                            |
| `onFailure`        | Callback which passes the error message which should hint to while the checkout session generation failed.                                                  |
| `quantity?`        | How much of the product is the user about to buy. **Defaults to `1` if not provided**                                                                       |
| `requestCurrency?` | In which currency will the checkout session will be shown, can be token mint or a fiat currency such as `USD`, `EUR`, `JPY` etc, supporting all currencies. |
| `backgroundColor?` | Background color of the pay button.                                                                                                                         |
| `textColor?`       | Text color of the pay button.                                                                                                                               |
