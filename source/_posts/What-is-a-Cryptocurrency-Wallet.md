title: What is a Cryptocurrency Wallet?
date: 2017-12-21 13:44:14
tags:
- wallets
---

When we talk about _wallets_, we're usually thinking of the leather kind into which we stash our cash and cards. These kind of wallets provide a convenient way of keeping all that important stuff together.

The _cryptocurrency wallet_, by contrast, works very differently from the wallets that we're used to. While a leather wallet gets crammed with cards, the crypto wallet doesn't actually _contain_ anything at all. It's really just a string of letters and numbers like this:

```
{"address":"05b40c95fc3f9c03a6cac78d359099acd78f2023","crypto":{"cipher":"aes-128-ctr","ciphertext":"b16b282dfb7ba96ecbb0203b165223698a6011c9f7ec176efb73ed17ab9491d6","cipherparams":{"iv":"edcd3b46a1d9310cd54a368d56d1c91c"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":262144,"p":1,"r":8,"salt":"d19309c53e39b5e5ce07776ec3e97c0a3b0047f17556cf284693a42076104215"},"mac":"42f5bbfdc90015ac8058ff9fccbab8bad78af088548aebe4db6d4ce9b3532628"},"id":"16ebce77-5b67-43bc-a247-0b00eca479e0","version":3}
```

That weird mess of text _is your wallet_.

If you're going to do anything with cryptocurrencies, whether investing or buying things online, you need to have a firm grasp on what it means to have a wallet.

Remember... the crypto wallet doesn't _contain_ anything. It's more like an _address_. No matter what kind of wallet software you use, that string of text is at the heart of it. It points to someone's personal ledger of transactions recorded on a blockchain. It is these transactions that determine the total amount of _currency_ credited to a wallet, or _account_.

In the case of Ethereum, the currency is called _ether_ (sometimes people pluralize it as _ethers_). Anyone who knows the address of your Ethereum wallet can send you ether. Feel free to post your public address everywhere!

In the above case, the _public address_ you share is:

```
0x05b40c95fc3f9c03a6cac78d359099acd78f2023
```

Can you spot where I got it from?

Receiving ether is easy. The trickiness is in sending it out from your wallet to another. For every public wallet address, there is a corresponding _private key_. The private key is what unlocks your wallet and allows you to send ether to another account.

### Beware!

Don't give your private key to anyone! If you know the private key to a wallet, you have full access to its ether.

## So how do I get a wallet?

To be clear, that weird looking string of text above is a wallet. Wallets are plentiful and free to anyone who wants one (or one hundred).

There are many web and mobile apps that give nice, easy-to-use interfaces for sending and receiving ether. They all basically just wrap themselves around that string of text.

Wallet software is meant to make using your wallet a bit easier. What would you do with all that gibberish otherwise?

#### Caution!

If you create a wallet on your phone or computer, you need to figure out how to make a backup of that _gibberish_.

If you lose your wallet, you lose all its ether too. There is no getting it back. You need to save it to a USB or, better yet, print it out to paper...

## A paper wallet?

Yes indeed. Your computer may crash, or your phone may get stolen. Print your wallet to paper and hide it somewhere good. Your wallet and its ether will exist as long as the Ethereum blockchain itself exists.

It's worth the effort to have a paper backup hidden in a place safe from water, fire, and burglars.

## So what's easiest for me?

You can either go online or download an app...

### Web Apps

The wallet web app I recommend is [MyEtherWallet](https://www.myetherwallet.com/).

This service provider really appreciates security concerns and education. Not only will they create a wallet that you can download and print, they enable you to unlock your account and send ether anonymously. Nothing about you or your wallet is stored on their system.

If you're skeptical (which is good), their software is all [open source](https://github.com/kvhnuke/etherwallet). So if you're hyper-concerned about security, you can download their code, inspect it, and execute it on an isolated computer to eliminate all chance of hackers eavesdropping.

### Mobile Apps

There are also plenty of mobile apps that give nice, easy-to-use interfaces for managing your wallets. I can't attest to privacy or security, but I've tried [Jaxx](https://jaxx.io/). It's a nice, native mobile app that also lets you create brand new wallets and send ether.

### Open Source

Whether you go online or download an app, be mindful of the information you are providing when unlocking your wallet. At a minimum, find out if a wallet application is _open source_. Closed-source software can easily be made to steal your private keys.

Even if you don't understand how to read computer code, you should be suspicious of any vendor unwilling to show you how their software works.

## So how do _you_ do it?

The Mining King uses [geth](https://github.com/ethereum/go-ethereum) on Ubuntu 16.04.

### Supplementary Material

If you can't make sense of what follows, just go to [MyEtherWallet](https://www.myetherwallet.com/) and set up your wallet there.

This process is for more advanced users. It is presented here to show what your Ethereum wallet application is doing at a low level.

First, from your home directory, create an Ethereum account (i.e., a wallet):

```
cd
geth account new
```

You will be asked for a passphrase. It should be a phrase sufficiently long, but easy for you to remember. For example, you might pick a line out of a book:

- _it was the best of times it was the blurst of times_

Or, you could list your favourite animals in order:

- _beefalo skink owl skunk hippo dung beetle platypus_

Once set, `geth` will create your _keyfile_. You can see it on your computer's hard drive by executing this:

```
ls -al ~/.ethereum/keystore
```

The contents of your `keystore` directory will look something like this:

```
total 12
drwx------ 2 miner miner 4096 Nov 18 11:50 .
drwx------ 4 miner miner 4096 Nov 14 13:46 ..
-rw------- 1 miner miner  491 Nov 18 11:50 UTC--2017-11-18T18-50-36.909516860Z--05b40c95fc3f9c03a6cac78d359099acd78f2023
```

That crazy long file starting with `UTC--` is your _keyfile_. Remember this mess?

```
{"address":"05b40c95fc3f9c03a6cac78d359099acd78f2023","crypto":{"cipher":"aes-128-ctr","ciphertext":"b16b282dfb7ba96ecbb0203b165223698a6011c9f7ec176efb73ed17ab9491d6","cipherparams":{"iv":"edcd3b46a1d9310cd54a368d56d1c91c"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":262144,"p":1,"r":8,"salt":"d19309c53e39b5e5ce07776ec3e97c0a3b0047f17556cf284693a42076104215"},"mac":"42f5bbfdc90015ac8058ff9fccbab8bad78af088548aebe4db6d4ce9b3532628"},"id":"16ebce77-5b67-43bc-a247-0b00eca479e0","version":3}
```

It's not nice to look at, but this is your wallet. It's also your _encrypted private key_! This is one file you definitely want to back up and print out. 

_Note_: you might recognize your wallet's public _address_ in this file, but you won't see its associated private key (it's encrypted, remember). `geth` makes it purposefully difficult to see your unencrypted private key. This is a good thing, because all you need to import and send money with a wallet application is the _keyfile_ and the _passphrase_ you use to unlock it. You can also use `geth` to unlock your wallet on the commandline, but that topic may be better left for another day.

Here your passphrase is just as vital as your private key. If you lose your passphrase, you cannot unlock your wallet.

## Conclusion

Cryptocurrency wallets don't actually _contain_ anything. They consist of a _public key_ (or _address_) and a _private key_. People send ether to your public address and the transaction is written onto the Ethereum blockchain. You unlock your wallet with the private key or passphrase to send ether to other wallets.

When backing up your wallet, there are two things you need to protect:

1. Your _keyfile_
2. And your _passphrase_ or _private key_ (depending on how you created your wallet in the first place)

Print it all to paper, laminate it, and bury it in a fireproof lockbox in the forest.

