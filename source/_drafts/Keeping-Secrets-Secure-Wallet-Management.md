title: Keeping Secrets - Secure Wallet Management
date: 2018-01-06 17:35:21
tags:
- wallets
- security
---

No one has to know you have a crypto wallet. Your holdings are no one's business until you decide to make a transaction.

Wallet management is a hassle, and if you're like me, you don't like to keep stuff out in the open. No hot wallets, web wallets, or otherwise for the Mining King. When creating a wallet, I like to keep things close to home, on the command line, and ultimately hard-copied to paper.

For that last step, I really like [WalletGenerator.net](https://walletgenerator.net/). It produces a PDF wallet you can print, fold, and hide under the hardwood.

Here's the thing... I don't want anyone to even know I was online creating wallets. There are sneaky people all over the place.

Who's to say you can even trust WalletGenerator.net? 

As it happens, _you_ can. The same wallet-generating software you see on the site is [open source](https://github.com/MichaelMure/WalletGenerator.net)! This means that you can download it to your computer, inspect the code, and make sure it's not secretly sending your private keys to a spy.

I always use the latest Ubuntu LTS, though the steps will be similar no matter your operating system. Here's how you do it... 

From the commandline, use `git` to download the project:

```
git clone https://github.com/MichaelMure/WalletGenerator.net.git
cd WalletGenerator.net
```

If you type `ls -al`, you'll see this:

```
total 600
drwxrwxr-x 8 daniel daniel   4096 Jan  6 17:58 ./
drwxrwxr-x 4 daniel daniel   4096 Jan  6 17:57 ../
drwxrwxr-x 8 daniel daniel   4096 Jan  6 17:58 .git/
-rw-rw-r-- 1 daniel daniel    105 Jan  6 17:58 .gitignore
-rw-rw-r-- 1 daniel daniel   3640 Jan  6 17:58 Gruntfile.js
drwxrwxr-x 2 daniel daniel   4096 Jan  6 17:58 images/
-rw-rw-r-- 1 daniel daniel 552918 Jan  6 17:58 index.html
drwxrwxr-x 2 daniel daniel   4096 Jan  6 17:58 l10n/
drwxrwxr-x 2 daniel daniel   4096 Jan  6 17:58 logos/
-rw-rw-r-- 1 daniel daniel    612 Jan  6 17:58 package.json
-rw-rw-r-- 1 daniel daniel  10431 Jan  6 17:58 package-lock.json
-rw-rw-r-- 1 daniel daniel   3145 Jan  6 17:58 README
drwxrwxr-x 2 daniel daniel   4096 Jan  6 17:58 src/
drwxrwxr-x 2 daniel daniel   4096 Jan  6 17:58 wallets/
```

Here's your chance to verify nothing fishy's going on.

Developers will recognize this as a `node` project, but you don't need to install any dependencies. You don't even really need to download the whole repository.

All that's needed is to open that `index.html` file listed above in your favourite web browser. The application's functionality is encompassed entirely within this file. When you load it in your browser you will discover it to be self-explanatory and easy to use.

For the extra paranoid, unplug the internet before you create your wallet. When you're done, close the browser and delete the entire application before plugging the internet back in.

Even these modest privacy steps are overkill. WalletGenerator.net is an excellent, secure application you can use to generate wallets for dozens of different coins (no [Ethereum](https://www.myetherwallet.com/), sadly). Downloading it and using it on your disconnected home computer erases any doubts about your privacy.

I can attest that no data was transmitted out from my computer at the time of writing this post.

{% asset_img no-data-transferred.jpg "Verified with Chrome Developer Tools" %}


