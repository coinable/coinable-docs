---
sidebar_position: 0
---

# Start Here

A user will be able to create many **Projects**, where a project can represent a restaurant, e-shop, store, or a freelance business.

In order to start accepting solana payments, each project will need to:

1. Designate a wallet to use as a **Vault** in which all customer payments will be directed.
2. Designate a wallet to use as a **Signer** to update settings which require a signature.
2. Decide which tokens to accept as payment, e.g. SOL, USDC, USDT, DUST, FORGE, etc.


## Set wallet addresses

Each project will need to specify two wallet addresses. One to use as a **Vault** and the other to use as a **Signer**, to update settings requiring a signature. We recommend creating a new wallet for the Signer, to be specifically used for the project. Customer payments will be directed to the wallet address designated as the vault.

This can be completed by going to the **Settings** tab and inputting the address into the wallet address field.

:::note

Any actions that impact the balances of the **Vault** will require the wallet specified as the vault to be connected and sign. For example, the unwrap SOL action found in the **Balances** page will require the signature of the vault.

:::


<div style={{textAlign: 'center', padding: '20px'}}>

![Settings Wallet Address](/img/guides/settings-wallet-address.png)

</div>

## Decide the tokens to accept as payment

You will need to decide which tokens that you want accept as payment. This can be accomplished by whitelisting tokens, and will restrict the type of token that customers can send to you allowing for increased control.



If a token account for the whitelisted token does not exist, Coinable will create an instruction to open a token account on your behalf when you whitelist the token. This token account will need to remain open in order for you to receive the whitelisted token as payment. A token account is comparable to having a checking account for different currencies accepted in your non-crypto business, e.g. USD, EUR, JPY.

:::tip

The **Balances** page will indicate the state of your token accounts. Green indicates your token accounts are open. If a token account that has been whitelisted is inadvertenly closed, you will be prompted to re-open the token account in order to continue accepting payments and avoid distruptions to your business.

:::

<div style={{textAlign: 'center', padding: '20px'}}>

![Settings Wallet Address](/img/guides/settings-balances.png)

</div>

Customers will be able to pay in any token that have been whitelisted by you. For example, if only USDC is whitelisted, customers will only be able to pay in USDC.


To whitelist tokens for a given project, go to **Settings** and connect your **Signer** wallet. Make sure to connect the **Signer** wallet that you specified in the previous step. Click on the tokens that you would like to accept as payment and click the `Update whitelist` button, and hit approve to reflect the changes on the blockchain.


<div style={{textAlign: 'center', padding: '20px'}}>

![Whitelist Tokens](/img/guides/settings-whitelist-tokens.png)

</div>

You are now ready to start accepting Solana payments through [Invoices](/guides/invoices), your [Online Web3 Store](/guides/storefronts), and [Checkouts](../../category/checkouts) integrated into an existing website.