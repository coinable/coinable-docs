---
sidebar_position: 1
---

import PayButton from '@coinable/pay-button';

# React

Currently we offer a React based component integration that will let you setup a pay button anywhere you want it in your app with the ability to style it inlined with your website, see [Customization](/developers/checkouts/integrations/react#customization) section for details.

## Live example

<PayButton productId="EKbTyXz5cPCspocqbRvFrE" onSuccess={(url) => {window.location.href = url;}} onFailure={(msg) => console.error(msg)} />

## Installation & Usage

Install via yarn using the following command (recommended)

```bash
yarn add @coinable/pay-button
```

via NPM

```bash
npm install @coinable/pay-button
```

Basic usage is as follows

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

#### Variants

Coinable products have this term called Variant which is an ability to checkout with a product variant for example, if we are selling a T-shirt and it has 5 different sizes, and 2 different colors we have 10 variations of the shirt `2 * 5 = 10`. Take a look at how Coinable structured your variants and their names, this is the `variant` name that you will use to populate the `variant` prop so if we have a **L / Black** variant of your t-shirt this is what we will use. As an example this would be my implementation of a varianted product.

```js
<PayButton
  // Previous necessary props here
  variant="L / Black"
/>
```

## Example

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

<div style={{textAlign: 'center', paddingTop: '20px'}}>

<video width="100%" height="auto" controls>

<source src="/videos/guides/react-example.mp4" type="video/mp4" />
</video>

</div>

## Customization

For customiztion of our freshly created buttons there are two approaches at the moment giving you the max possible styling ability.

### CSS/SCSS

Override the `coinable-pay-button` className and style the button CSS however you see fit for your website.

:::note
This approach will override all of the CSS that is provided out of the box. Its the implementors responsibility to rewrite styling of the button to their preference.
:::

```js
<PayButton
  // Previous necessary props here
  className="my-own-classname"
/>
```

### Styled Components

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

### Customization Props

If you don't want to touch the underlying CSS of the pay button component we allow to change the basic styling like color of your button, some implementors find it sufficient for their need, the available props are `backgroundColor` and `textColor`. See example below.

```js
<PayButton
  // Previous necessary props here
  backgroundColor="#1FC055"
  textColor="rgb(204, 252, 162)"
/>
```

## Props

Available props for the component are

| Prop name          | Description                                                                                                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `productId`        | The product id you get from the product created through Coinable.                                                                                                                                      |
| `onSuccess`        | Callback which passes the url as the parameter which is used to redirect user to his generated checkout session.                                                                                       |
| `onFailure`        | Callback which passes the error message which should hint to while the checkout session generation failed.                                                                                             |
| `variant?`         | If product utilizes variant the name of the variant can be passed here and the checkout session will be initiated for that product's variant. **Defaults to `undefined` if not provided.**             |
| `quantity?`        | How much of the product is the user about to buy. **Defaults to `1` if not provided.**                                                                                                                 |
| `requestCurrency?` | In which currency will the checkout session will be shown, can be token mint or a fiat currency such as `USD`, `EUR`, `JPY` etc, supporting all currencies. **Defaults to `USD` if not provided.**     |
| `backgroundColor?` | Background color of the pay button. **Defaults to `black` if not provided.**                                                                                                                           |
| `textColor?`       | Text color of the pay button. **Defaults to `white` if not provided.**                                                                                                                                 |
| `children?`        | If you want a different content inside of the pay button, instead of the default Pay text. **Defaults to `Pay` if not provided.**                                                                      |
| `className?`       | Lets you override the default className of the component which will give the ability to redefined the Buttons CSS, overrides the default style. **Defaults to `coinable-pay-button` if not provided.** |
| `style?`           | We also provide the style `CSSProperties` object for your to add inlined JSS. **Defaults to `{}` if not provided.**                                                                                    |
| `ref?`             | We also provide you with access to ref if DOM access is needed in some cases, if you don't know what this for you probably don't need to touch it.                                                     |
