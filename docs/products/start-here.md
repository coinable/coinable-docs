---
sidebar_position: 0
---

# Start Here

A user will be able to create many **projects**, where a project can represent a restaurant, e-shop, store, or a freelance business.

In order to start accepting crypto payments, each project will need to specify:

1. A wallet in which all token accounts will be associated.
2. Tokens to accept as payment, e.g. SOL, USDC.


## Set wallet address

Each project will need to set a wallet address. The wallet address will be used to derive token accounts, where customers will directly transfer tokens accepted for payment.

One can set the **wallet address** by going to the **Settings** tab and inputting the address into the wallet address field.



<div style={{textAlign: 'center', padding: '20px'}}>

![Settings Wallet Address](/img/products/settings-wallet-address.png)

</div>

## Whitelist tokens

You will need to set the tokens that you will accept for each project. This can be accomplished by whitelisting tokens, and will restrict the type of token that potential customers can send to you allowing for increased control.

If a token account for the whitelisted token does not exist, Coinable will create an instruction to open a token account on your behalf when you whitelist the token. This token account will need to remain open in order for you to receive the whitelisted token as payment. A token account is comparable to having a checking account for different currencies accepted in your non-crypto business, e.g. USD, EUR, JPY.

:::tip

If you find that a token account is missing for a whitelisted token, removing, and re-adding will create a new token account.

:::

Once the tokens to accept have been set, customers will be able to pay in any tokens that have been whitelisted for a given project. For example, if only USDC is whitelisted, customers will only be able to pay in USDC.


To whitelist tokens for a given project, go to **Settings** and connect wallet. Click on the tokens that you would like to accept as payment and click the **Update whitelist** button, and hit approve to reflect the changes on the block chain.


<div style={{textAlign: 'center', padding: '20px'}}>

![Whitelist Tokens](/img/products/whitelist-tokens.png)

</div>
