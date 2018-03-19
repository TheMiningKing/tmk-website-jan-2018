title: Tor-Proofing the World's First Open Source Crypto Shopping Cart
tags:
- tor
- shopping
---

The earliest version of the cryptocart deploys beautifully through traditional means. I quickly learned, however, that it could not be deployed anonymously via Tor.

The reason was simple... the email ordering system - awesome though it may be - was inherently insecure.

Based on this realization and the feedback offered by my early human test subjects, I had to redesign the checkout process for two reasons:

1. It's a break from the norm
2. It doesn't work on Tor anyway

In adopting this feedback, _Thanks y'all!_ I aim to:

1. Preserve mutual anonymity between buyer and seller
2. Minimize record collection and facilitate record encryption
3. Minimize database dependency

These are challenges huge in their subtleties. They haven't been met yet, yet I bet they do get met.

With that, I present...

# How to Tor-Proof Your Crypto Shop
 
Bad news... if you _really_ want anonymity, then buyer-seller email communication becomes a manual exercise. You'll need something like [mail2tor.com](https://mail2tor.com) to talk to your customers.

Moreover, the cart application sends email notifications to the seller, which probably shouldn't leave the server. Consider this _healthy paranoia_...

## Assumptions

These steps are executed on an updated Ubuntu 16.04 Server _(sorry, haven't tried 18 yet)_. Docker, Docker Compose, Node, Git, etc., should be set up and ready go. If none of that makes sense to you, [buy a t-shirt](https://shop.theminingking.com) and maybe the Mining King will help you set up shop.

I'm also assuming a production deployment. The steps that follow are the _minimum_ required to set up a Tor shop. The [crypto-shopping-cart](https://github.com/TheMiningKing/crypto-shopping-cart) is open-source and free for everyone to use. Adapt it to your purposes. Contributors are welcome!

## Set up shop

Obtain the cryptocart software and install its dependencies:

```
git clone https://github.com/TheMiningKing/crypto-shopping-cart
cd crypto-shopping-cart
NODE_ENV=production npm install
```

Create a `.env` file and adapt the following for your purposes:

```
FROM=your@email.com
PASSWORD=secret
WALLET=0xd24def0856636050cf891befc0fa69ecf96c160b
CURRENCY=ETH 
```


## Email

In minimizing database dependence, I decided to handle all orders via email early on. No orders are recorded apart from this. This also, I believe, makes the managing and destruction of private data more manageable. 

### Setup Postfix

In the interest of reducing your Tor server's profile, all emails should be caught and stored on the server itself. No outgoing, identifiable transmissions. I use `postfix` to intercept the shopping carts emails and deliver them to the host machine's `app` user.

`postfix` setup on Ubuntu 16.04 isn't always a straight-forward process. It's best to remove all traces of potentially problematic software before proceeding with re-installation and configuration. In this case, the problematic software is `sendmail`:

```
sudo service sendmail stop
sudo apt purge --auto-remove sendmail
```

Since we've come this far, why not remove any existing `postfix` install too:

```
sudo service postfix stop
sudo apt purge --auto-remove postfix
```

Now reinstall `postfix`:

```
sudo apt update
sudo DEBIAN_PRIORITY=low apt-get install postfix
```

Use the following information at the prompts:

- *General type of mail configuration?*: For this, select _Local only_ because we don't want mail coming in or out
- *System mail name*: I use the default system name, which in my case is _ubuntu_
- *Root and postmaster mail recipient*: I use the current user name, which is _app_
- *Other destinations to accept mail for*: The suggested default should be: _$myhostname, ubuntu, localhost.localdomain, localhost_
- *Force synchronous updates on mail queue?*: _No_ 
- *Local networks*: Leave the default: _127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128_
- *Mailbox size limit*: Set to _0_ to disable size restrictions
- *Local address extension character*: I leave this empty
- *Internet protocols to use*: Choose _all_

This is the best configuration I've discovered so far. If there are any potential security vulnerabilities, I'm open to solutions.

### Setup Alpine

`alpine` is a terminal-based email client.


# Future Work

How do we preserve both anonymity and build trust?

