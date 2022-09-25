---
sidebar_position: 1
---

import PayButton from '@coinable/pay-button';

# React

The React based component integration will let you integrate a pay button anywhere within your application. The ability to apply style will keep the pay button consistent with your websites branding. See [Customization](/developers/checkouts/integrations/react#customization) section for details.

## Live example

<PayButton productId="EKbTyXz5cPCspocqbRvFrE" onSuccess={(url) => {window.location.href = url;}} onFailure={(msg) => console.error(msg)} />

## Installation & Usage

Install via yarn using the following command (recommended).

```bash
yarn add @coinable/pay-button
```

via NPM

```bash
npm install @coinable/pay-button
```

Basic usage is as follows:

```js
<PayButton
  productId="KvD8sieX9t48Lv7AMEwo2b"
  onFailure={(msg) => console.error(msg)}
  onSuccess={(url) => {
    window.location.href = url;
  }}
/>
```

<<<<<<< HEAD

1. Where `productId` we pass the product id of the product we setup in our Dashboard via the [Products](https://coinablepay.com/dashboard/products) tab.
2. Then we have two callbacks that we need to implement

   - `onSuccess` returns the checkout session url for our user to be redirected too and pay for the product.
   - # `onFailure` returns an error message on why did the checkout session initiation has failed.

3. Pass the ID of the product to `productId`. Products are set up through the [Products](https://coinablepay.com/dashboard/products) page on the Dashboard.
4. Two callbacks need to be implemented.
   - `onSuccess` returns the checkout session url for the customer to be redirected to and pay for the product.
   - `onFailure` returns an error message on why the checkout session initiation failed.
     > > > > > > > ab45d1b12b00308abf0c4fc52b92eee482e3eafe

#### Variants


Products created with Coinable may have variants. In order to create a checkout with a variant, pass in the name of the variant which can be obtained from the Product page. For example, if we are selling a T-shirt with 5 different sizes, and 2 different colors, there are 10 variations of the shirt. If an available variant is a large black shirt, and the variant name displayed is **L / Black** on the product page, that is what is passed to the `variant` prop.

```js
<PayButton
  // Previous necessary props here
  variant="L / Black"
/>
```

## Example

A minimum working example is as follows.

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

The example integration looks like this, **just in 33 lines of code**:

<div style={{textAlign: 'center', paddingTop: '20px'}}>

<video width="100%" height="auto" controls>

<source src="/videos/guides/react-example.mp4" type="video/mp4" />
</video>

</div>

## Customization

Two approaches allows integrators the ability to style the pay buttons.

### CSS/SCSS

Override the `coinable-pay-button` className and style the button CSS.

:::note
This approach will override all of the CSS that is provided out of the box. It is the implementors responsibility to rewrite styling of the button.
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

Basic styling can be accomplished by using the available props `backgroundColor` and `textColor`. See example below.

```js
<PayButton
  // Previous necessary props here
  backgroundColor="#1FC055"
  textColor="rgb(204, 252, 162)"
/>
```

## Props

Available props for the component are

| Prop name          | Description                                                                                                                                                                                               |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `productId`        | The product ID you get from the product created through Coinable.                                                                                                                                         |
| `onSuccess`        | Callback which passes the url as the parameter which is used to redirect customers to a checkout session.                                                                                                 |
| `onFailure`        | Callback which passes the error message which should indicate why the generation of a checkout session failed.                                                                                            |
| `quantity?`        | The amount of product being requested for checkout. **Defaults to `1` if not provided.**                                                                                                                  |
| `variant?`         | If product utilizes variant the name of the variant can be passed here and the checkout session will be initiated for that product's variant. **Defaults to `undefined` if not provided.**                |
| `requestCurrency?` | The display currency of the checkout session. Can be a token mint or a fiat currency such as `USD`, `EUR`, `JPY` etc. **Defaults to `USD`.**                                                              |
| `backgroundColor?` | Background color of the pay button. **Defaults to `black` if not provided.**                                                                                                                              |
| `textColor?`       | Text color of the pay button. **Defaults to `white` if not provided.**                                                                                                                                    |
| `children?`        | If you want different content inside of the pay button, instead of the default Pay text. **Defaults to `Pay` if not provided.**                                                                           |
| `className?`       | Lets you override the default className of the component which will give the ability to redefine the Buttons CSS, and overrides the default style. **Defaults to `coinable-pay-button` if not provided.** |
| `style?`           | We also provide the style `CSSProperties` object for you to add inlined JSS. **Defaults to `{}` if not provided.**                                                                                        |
| `ref?`             | Access to ref, if DOM access is needed.                                                                                                                                                                   |
