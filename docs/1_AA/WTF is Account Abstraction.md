# WTF is Account Abstraction

What would happen if you lose your access to the private keys of your wallet?

Nothing good, right?

Because your private key is your account, losing your key means losing your account, your money, your data. Everything.

With AA if a user lose everything with a small mistake we can help them to recover their information, because every account is a smart contract that can contain logic and implement flow.

So allows us to use smart contract logic to specify not just the effects of the transaction, but also the fee payment and validation logic. This allows many important security benefits, such as multisig and smart recovery wallets, being able to change keys without changing wallets, and quantum-safety.


## How accounts works today?


**Ethereum has two different types of "accounts":**

* Contract Accounts
* Externally Owned Accounts (EOAs)

*EOAs is regular user accounts.*

There's some [Key differences](https://ethereum.org/en/developers/docs/accounts/) between EOAs and Contracts.

**Both account types have the ability to:**

* Receive, hold and send ETH and tokens
* Interact with deployed smart contracts




# WTF happens with accounts in AA

Account Abstraction unifies Contract Accounts and EOAs. 

It makes user accounts more ‘programmable’. You remove the logic of signing transactions from the account, and you “abstract” it out, hence account abstraction.

The new experiences you see being built are features made possible by account abstraction but not by account abstraction itself. 

It’s no longer one-account-fits-all-use-cases. Instead, each user can have an account that is adapted to their needs. 

* If you want to use a signig scheme than ECDSA, you can write an account.
* If you want to use multiples keys to authorize transactios, you can write an account.
* If you want to change the signer of your account every week, you can write an account.

Because now you have with AA "programmable transaction validity."


# Starkware & Accout Abstraction

Starkware focused in some key components of the protocol:

1. Signature abstraction:
    * Allow different account contracts to use different signature validation schemes.
2. Payment abstraction:
    * Allow different models of payment for transactions. For example, payment by another party/contract.
    * Better UX: don’t mandate a specific token (native or contract-defined) to be used for paying for transactions.
    
    You can read the hole proposal [here](https://community.starknet.io/t/starknet-account-abstraction-model-part-1/781). They took the [ERC-4337](https://medium.com/infinitism/erc-4337-account-abstraction-without-ethereum-protocol-changes-d75c9d94dc4a) from Ethereum and improved the model.
    
# Which wallets supports AA?

If we talk about wallets and tools that can help us to use this model, we have two main options. Let's talk about it.

# Argent

Argent X is the most popular and only open-source wallet on StarkNet, you can buy and bridge crypto to StarkNet, swap tokens, mint and store NFTs, play blockchain games and much more.

## How to create an Argent X wallet



1.  Go to browser webstore, download Argent X, and add the extension.

![](https://i.imgur.com/pySO51T.png)

2. Click "create a new wallet"

![](https://i.imgur.com/2LjYm2N.png)

3. Read the disclaimers and click "continue"

![](https://i.imgur.com/658AaIu.png)

4. Create a password for your wallet
 
![](https://i.imgur.com/YieazCH.png)

5. Your wallet is ready! Click "finish" and pin the Argent X extension in your browser for quick access if you want.

![](https://i.imgur.com/RIpYmdJ.png)

6. Open your wallet extension and now you're ready to use it

![Uploading file..._oapelc3gp]()



### FYI

*Every transaction on the blockchain requires gas, which is a small amount of crypto, to make transactions. Before you begin exploring StarkNet mainnet, you will need to fund your Argent X wallet, so you can deploy your wallet. This is because every wallet on StarkNet is a smart contract (remember AA class) and requires a small on-chain transaction to activate your wallet.*

After that, you're gonna be able to send, swap and add tokens, etc.


### Argent X Features

The Argent X wallet has features that you would expect from a cryptocurrency wallet like global on-ramp or bridge funds. But Argent X is a smart wallet that has unique features made possible by being built on StarkNet. 

* Fraud monitoring.
* Transaction review.
* Batch transactions together.
* Social recovery.
* Session keys.


Now you know, time to explore! 

# Braavos

Braavos is not just a browser app but a native mobile app, so you can hodl your tokens, transfer, swap and manage your NFT collectibles, but even more so, access and connect to any dApp on StarkNet from your phone.

Your mobile is always with you, allowing you to access crypto whenever and wherever you want with the privacy of your biometric encryption.


## How to create an Braavos wallet


1.  Go to browser webstore, download Argent X, and add the extension.

![](https://i.imgur.com/WSVOM6u.png)


2. Pin the Braavos extension in your browser for quick access and than click "Get Started"

![](https://i.imgur.com/yPPPi6F.png)


4. Create a password for your wallet
 
![](https://i.imgur.com/1TYAk0n.png)


5. Save the secret recovery phrase

![](https://i.imgur.com/7EjinKj.png)

6. Now you're ready to use your wallet

![](https://i.imgur.com/JM9MnK4.png)

7. Remember that you have to deposit tokens in your wallet if you want to use the mainnet network, because you're using a AA wallet, so you have to active the smart contract account.

![](https://i.imgur.com/J1AcwGU.png)









