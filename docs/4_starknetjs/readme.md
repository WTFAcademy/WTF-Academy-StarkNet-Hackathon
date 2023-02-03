---
title: 4. StarkNet.js
tags:
  - cairo
  - starknet
  - starknetjs
  - wallet
  - contract
  - wtfacademy
---

## Introduction to StarkNet.js

[StarkNet.js](https://www.starknetjs.com/) is a JavaScript library to interact with [StarkNet](https://starknet.io/), typically in script or a decentralized applicatoin. StarkNet.js is inspired by [Ethers.js](https://github.com/ethers-io/ethers.js), so it is easy if you have experience on it. 

> If you are not familiar with Ethers.js, check [WTF Ethers Tutorial](https://github.com/WTFAcademy/WTF-Ethers).

## Install with `npm`

First, get your [VSCode](https://code.visualstudio.com/download) and [Node.js](https://nodejs.org/en/download/) ready. Then open your terminal to install starknet.js with npm.


```shell
npm install starknet
```

## Provider

`Provider` allows you to interact with the StarkNet network, without signing transactions or messages.

```js
const provider = new Provider({ sequencer: { network: 'goerli-alpha' } }) // for testnet 1
// output chainId
console.log("Chain ID: ", await provider.getChainId());
```

## Account

`Account` extends `Provider` and allows you to create and verify signatures. 
It is similar to `Wallet` in ethers.js, but different since all accounts are abstract on StarkNet. You need a `KeyPair` to create an account instance.

```js
const privateKey = process.env.PRIVATE_KEY;
console.log(privateKey)
const accountAddr = "0x06b59aEC7b1cC7248E206abfabe62062ba1aD75783E7A2Dc19E7F3f351Ac3309";
const starkKeyPair = ec.getKeyPair(privateKey);
const account = new Account(provider, accountAddr, starkKeyPair);
```

## Get Account Nonce

Gets the nonce of the account with respect to a specific block.

```js
const addr = "0x06b59aEC7b1cC7248E206abfabe62062ba1aD75783E7A2Dc19E7F3f351Ac3309"
const nonce = await provider.getNonceForAddress(addr)
console.log(Number(nonce));
```

## A Simple Contract

We deployed a simple contract to interact with. It has a storage variable `balance`, an event `log_data`, and 2 functions: `read_balance()` and `set_balance()` to read and set the `balance`. You can find the contract on starkscan [here](https://testnet.starkscan.co/contract/0x05844982dc2e548395fb4fc6e4abd16f893ff1b5baaea80bd8de522f784473ef#overview).

```python
%lang starknet

from starkware.cairo.common.cairo_builtins import HashBuiltin

// starage variable: balance
@storage_var
func balance() -> (res: felt) {
}

// event
@event
func log_data(value: felt) {
}


// set balance.
@external
func set_balance{
    syscall_ptr: felt*,
    pedersen_ptr: HashBuiltin*,
    range_check_ptr,
}(amount: felt) {
    balance.write(amount);
    // 释放事件
    log_data.emit(amount);
    return ();
}

// read balance.
@view
func read_balance{
    syscall_ptr: felt*,
    pedersen_ptr: HashBuiltin*,
    range_check_ptr,
}() -> (amount: felt) {
    let (res) = balance.read();
    return (amount=res);
}
```

## Read Contract

You can create a contract instance with `Contract` method, and pass ABI, contract address, and provider to it. Then you can interact with the contract on StarkNet.

```js
const testAddress = "0x0352654644b53b008b9fd565846cca116c0911d0eeabb57df00b55ed77ad211e";
// Read ABI from contract address
const { abi: testAbi } = await provider.getClassAt(testAddress);
if (testAbi === undefined) { throw new Error("no abi.") };
// create contract instance
const myTestContract = new Contract(testAbi, testAddress, provider);
// call read_balance method
const bal1 = await myTestContract.read_balance();
// you can also use call method
// const bal1 = await myTestContract.call("read_balance");
console.log("Current Balance =", bal1.toString());
```

## Write Contract

To write on the blockchain, you need to connect the contract instance with account.

```js
// Connect account with the contract
myTestContract.connect(account);
// or you can use invoke
// const result = await myTestContract.invoke("set_balance", [888]);
const result = await myTestContract.set_balance(999);
await provider.waitForTransaction(result.transaction_hash);
const bal2 = await myTestContract.read_balance();
console.log("New Balance =", bal2.toString());
```

## Write Contract with Verification

If you interact with a function that need the proof that you have the private key of the account, you have to invoke this function with `account.execute`, and pass following variables:

- `contractAddress`: address of the contract to invoke.
- `entrypoint`: name of the function to invoke.
- `calldata`: array of parameters for this function.


```js
// account.execute: when you interacat with the function that need the proof that you have the private key of the account.
const executeHash = await account.execute(
    {
      contractAddress: myContractAddress,
      entrypoint: 'transfer',
      calldata: stark.compileCalldata({
        recipient: receiverAddress,
        amount: ['10']
      })
    }
  );
await provider.waitForTransaction(executeHash.transaction_hash);
```

## Read Events

It is easy to read event from transaction receipt. But a transaction can contain multiple events, so you need to filter out the one you care.

```js
// Events
// there are multiple events in the tx, because ERC20 and argent tx also emit events.
// we need to filter out the event that we care    
const events = txReceiptDeployTest.events;
const event = events.find(
    (it) => number.cleanHex(it.from_address) === number.cleanHex(testAddress)
  ) || {data: []};
console.log("event: ", event);
```

## Summary

In this tutorial, we introduced how to use StarkNet.js, including provider, account, read/write contract, and read events.