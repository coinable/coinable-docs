---
sidebar_position: 1
---


# Add products

## Create a new product

Products allow users to create products to use with invoices, checkouts, and your online Web3 storefronts. When creating a new product, a user will be able to specify details, inventory if a product is a physical product, as well as gate the product by different NFT collections.

**Click the** `+ Create product` **to start.**

<div style={{textAlign: 'center', padding: '20px'}}>

![Products](/img/guides/store-products-page.png)

</div>



### 1. Select the product type
Users can select between service, digital, and physical products. The fields required will be dependent on the product type.

### 2. Add media and set access

Upload product images, including jpgs, png, and gifs with sizes restricted to 1MB. Select an image to use as a thumbnail. This will be used as the cover if there are multiple images.

:::tip

Make sure to toggle **Show on Storefront** to display your product in your store.

:::

<div style={{textAlign: 'center', padding: '20px'}}>

![One Click Start](/img/guides/store-products-media.png)

</div>

### 3. Set pricing and inventory

Set the pricing and amount available. The ability to add options and create variants of products, e.g Large Blue T-Shirt, is available for physical products. You can set the pricing for inventory independently for each variant.

**Pricing currency** will determine what your customers will see on your online store. If priced in `USD`, they will see products priced in `USD`, while payments will be made in the tokens you have approved through the **Settings** page.

:::warning

At the moment, only products priced in `USD`, i.e. where **Pricing currency** is set to `USD` below, will be displayed in your online store. More options available in 3Q/2022.

:::

<div style={{textAlign: 'center', padding: '20px'}}>

![One Click Start](/img/guides/store-products-pricing.png)

</div>


Hit the **Create** button. **That's it.**

## Gate products by NFT collections

Users will be able to gate their products by an NFT collection. This can be done at creation time, or by editing the product later. Coinable automatically checks for the NFT in the consumers wallet as a proof of membership and handles onchain verification on behalf of the user.

:::note

Adding a gate will require a wallet approve and a small amount of SOL for rent. The rent is used to add information on chain to represent that you have gated the product by a specific collection for a given project. The onchain data is used for onchain verification at every customer checkout. **The rent will be returned, when you remove the gate.**

:::

<div style={{textAlign: 'center', padding: '20px'}}>

![Gate](/img/guides/products-gate-products.png)

</div>

**If you don't see your NFT collection listed, reach out to the team and make a request.**