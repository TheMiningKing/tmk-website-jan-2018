title: Buying and Selling Currency with Ethereum
date: 2017-12-22 15:34:58
tags:
- commerce
- exchange
---

Bitcoin took a nosedive today. It's down around $12,000 CDN from where it was six days ago.

Sounds like a good time to buy!

It just so happens I've got a bit of ether sitting in a [wallet](https://etherscan.io/address/0xd24def0856636050cf891befc0fa69ecf96c160b). I wonder how much Bitcoin I can get?

## Open an account on a currency exchange

I've opened a bunch of accounts all over the place... [Coinsquare](https://coinsquare.io), [Kraken](https://www.kraken.com/), and a couple I don't even remember.

No matter where you sign up, you'll need to provide some identification.

1. Government issued ID
2. A recent utility bill
3. And sometimes, a selfie holding your government ID

I like Coinsquare, because it has a nice interface. But sadly, they don't let you fund your account with ether, and their excuses for this inability are running pretty thin.

I eventually ended up at [QuadrigaCX](https://www.quadrigacx.com/). Like Coinsquare, they're based in Canada. Unlike Coinsquare, they've got their acts together when it comes to putting ether into your account.

This is my first goal...

## Get the ether into my QuadrigaCX account

I want to do this so I can trade it for Bitcoin.

How _you_ do this depends entirely on what kind of wallet you're using. In a [previous post](/blog/2017/12/21/What-is-a-Cryptocurrency-Wallet/) I recommended [MyEtherWallet](https://www.myetherwallet.com/). If you're unfamiliar with wallets, go read that previous post first.

### Beware!

If you mess up an address or an amount, you will never see your ether again. Start with a small amount and wait to see if the transfer was successful before transferring larger amounts.

### Let's get started...

When you're funding your QuadrigaCX account with ether, you are provided with a destination wallet address. If you send ether to this address, it will be added to your QuadrigaCX account balance. I can learn a few things about this address by entering it at [etherscan.io](https://etherscan.io/address/0xf7a5a3c18e0f81d853874d404d20a1900037ccdb).

No matter what kind of wallet you're using, you're basically doing the same as what follows. Sending money from your wallet's address to a destination address (QuadrigaCX's wallet address, in this case). It's up to you to find a wallet you're comfortable with and use it correctly.

I'm going to do the whole wallet thing the crazy computer programmer way. That is, on the Ubuntu 16.04 commandline with the [geth](https://github.com/ethereum/go-ethereum) Javascript console. Steps adapted from [here](http://blog.bradlucas.com/posts/2017-08-17-send-eth-from-geth-console/).

I'm using a machine running an Ethereum node, so I want to attach to an existing `geth` process:

```
geth attach
```

If there is no node running, you can execute `geth console` which will start it all up for you. Either way, you end up at a prompt that looks like this:

```
Welcome to the Geth JavaScript console!

instance: Geth/v1.7.3-stable-4bb3c89d/linux-amd64/go1.9
coinbase: 0xd24def0856636050cf891befc0fa69ecf96c160b
at block: 4779310 (Fri, 22 Dec 2017 16:31:07 MST)
 datadir: /home/miner/.ethereum
 modules: admin:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

> 
```

### Finding my account balance

I want to know how much ether I have, so I type the following and hit _Enter_:

```
> web3.fromWei(eth.getBalance(eth.coinbase));
```

This outputs my wallet's balance:

```
0.072641769357074608
```

Not a huge amount, but worth about $50 CDN at the moment. I may just get me a small slice of Bitcoin.

### Unlock my wallet

Still on the Javascript console...

```
> personal.unlockAccount(eth.coinbase)
```

This will ask for a passphrase.

### Send a bit of ether

If you entered your passphrase correctly, you are now able to send ether. I'm just going to send a bit so that I know I'm doing it right.

The following command sends a small amount (1000000000000 wei or 0.000001 ether) to the QuadrigaCX wallet provided to me on the site:

```
> web3.eth.sendTransaction({from: eth.coinbase, to: '0xf7a5a3c18e0f81d853874d404d20a1900037ccdb', value: web3.toWei(0.000001, "ether"), gas: 200000})
```

This will return a transaction code:

```
"0xbe23280cbbe862179b603b7308933aef267bd8c44b81ec871571bd9cede91859"
```

You can view the [details of the transaction](https://etherscan.io/tx/0xbe23280cbbe862179b603b7308933aef267bd8c44b81ec871571bd9cede91859) and even see when the ether is credited to the [destination account](https://etherscan.io/address/0xf7a5a3c18e0f81d853874d404d20a1900037ccdb).

The funds will show up in your QuadrigaCX account after a sufficient number of nodes have verified the transaction.

### Send more ether

When you're comfortable and confident to move more ether, send a larger amount. I'm going to send half of my remaining stash, because it's good to keep a little set aside.

```
> web3.eth.sendTransaction({from: eth.coinbase, to: '0xf7a5a3c18e0f81d853874d404d20a1900037ccdb', value: eth.getBalance(eth.coinbase)/2, gas: 200000})
```

Once again, the [transaction](https://etherscan.io/tx/0x5b666478817f1ce19b70620a6dfb1864fc0c6131cc99a9569ac552147305ef48) can be viewed with a blockchain explorer.


## Trade ether for Bitcoin

QuadrigaCX actually makes [this very easy](https://www.quadrigacx.com/trade/eth/btc). I was able to trade my 0.03506 ETH (roughly) for 0.00173058 BTC. At the moment, my small amount of Bitcoin is worth about $31 CDN.



