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

In adopting this feedback _(thanks y'all!)_ I aim to:

1. Preserve as much mutual anonymity as possible between buyer and seller
2. Minimize record collection and facilitate record encryption
3. Minimize database dependencies

These are challenges huge in their subtleties. They haven't been met yet, yet I bet they do get met.

With that, I present...

# How to Tor-Proof Your Crypto Shop
 
Bad news... if you _really_ want anonymity, then buyer-seller email communication becomes a manual exercise. You'll need something like [mail2tor.com](http://mail2tor.com) to talk to your customers.

Moreover, the cart application sends email notifications to the seller, which cannot be allowed to leave the server. Consider this _healthy paranoia_...

## Assumptions

These steps are executed on an updated Ubuntu 16.04 Server _(sorry, haven't tried 18 yet)_. Docker, Compose, Node, Git, etc., should be set up and ready go. If none of that makes sense to you, [buy a t-shirt](https://shop.theminingking.com) and maybe the Mining King will help you set up shop.

I'm also assuming a production deployment. The steps that follow are the _minimum_ required to set up a Tor shop. The [crypto-shopping-cart](https://github.com/TheMiningKing/crypto-shopping-cart) is open-source and free for everyone to use. Adapt it to your purposes. Contributors are welcome!

## First, disable remote password login on your Tor server

At the very least, this will prevent anyone from attempting to guess or brute-force their way in.

Execute the following on your local system, i.e., _not_ on the Tor server:

```
ssh-keygen -t rsa
```

Follow the prompts. The default filename (`rsa_id`) is fine. For extra paranoia, be sure to encrypt your private key with a passphrase.

Now, install the public key on the remote server:

```
ssh-copy-id -i $HOME/.ssh/rsa_id.pub user@myremoteserver.com
```

The passphrase won't make your login any faster. You'll need to enter the passphrase every time you use the key to access your remote server:

```
ssh user@myremoteserver.com
```

Assuming you have successfully logged into the machine, disable root and remote password logins:

```
sudo vi /etc/ssh/sshd_config
```

Find and set the following as shown:

```
ChallengeResponseAuthentication no

PasswordAuthentication no

UsePAM no

PermitRootLogin no
```

Restart the `ssh` server

```
sudo /etc/init.d/ssh reload
```

If you log out and attempt to login with a password via `root` or your user, you will now be denied. Only the holder of the private `rsa_id` file can gain access.


## Setup shop

These instructions are adapted from the [crypto-shopping-cart's](https://github.com/TheMiningKing/crypto-shopping-cart) `README.md`.

Obtain the cryptocart software and install its dependencies:

```
git clone https://github.com/TheMiningKing/crypto-shopping-cart
cd crypto-shopping-cart
NODE_ENV=production npm install
```

Create a `.env` file and adapt the following for your purposes:

```
# Don't change these
FROM=root@localhost
PASSWORD=secret
TOR=true

# Do change these
WALLET=0xd24def0856636050cf891befc0fa69ecf96c160b
CURRENCY=ETH
SITE_NAME=The Mining King
SITE_URL=
```

The _Don't change_ settings are configured so that email does not exit the server hosting your hidden service. Obviously, you'll want to change `WALLET`, `SITE_NAME`, et al.

Execute the Docker composition:

```
docker-compose -f docker-compose.tor.yml up -d
```

You probably don't want to sell [sick mining Ts](https://shop.theminingking.com) on your hidden site. You can set your own product information in the `db/data.json` file. Note that product prices are specified in Gwei, because floating point stuff is a pain.

Once you've set up your product information (pictures go in `public/images/products/`), seed your database:

```
docker-compose -f docker-compose.tor.yml run --rm node node db/seed.js NODE_ENV=production
```

At this point, your shop should be running, but it won't be accessible from Tor or the clear web.

## Setup Tor

These steps were adapted from [here](https://libertyseeds.ca/2017/12/12/Dockerizing-Tor-to-serve-up-multiple-hidden-web-services/).

Tor should be setup in a directory apart from your shop. Assuming you are currently in the `crypto-shopping-cart/` directory,

```
cd ..
mkdir tor-proxy
cd tor-proxy
```

Paste the following to `Dockerfile` in this new directory:

```
FROM debian
ENV NPM_CONFIG_LOGLEVEL warn

ENV DEBIAN_FRONTEND noninteractive

EXPOSE 9050
  
# `apt-utils` squelches a configuration warning
# `gnupg2` is required for adding the `apt` key
RUN apt-get update
RUN apt-get -y install apt-utils gnupg2

#
# Here's where the `tor` stuff gets baked into the container
#
# Keys and repository stuff accurate as of 2017-10-25
# See: https://www.torproject.org/docs/debian.html.en#ubuntu
RUN echo "deb http://deb.torproject.org/torproject.org stretch main" | tee -a /etc/apt/sources.list.d/torproject.list
RUN echo "deb-src http://deb.torproject.org/torproject.org stretch main" | tee -a /etc/apt/sources.list.d/torproject.list
RUN gpg --keyserver keys.gnupg.net --recv A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89
RUN gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get -y install tor deb.torproject.org-keyring

# The debian image does not create a default user
RUN useradd -m user
USER user

# Run the Tor service
CMD /usr/bin/tor -f /etc/tor/torrc
```

Paste the following to `docker-compose.yml` (still in the `tor-proxy/` directory):

```
version: '3'
services:
  tor:
    build: .
    restart: always
    volumes:
      - ./config/torrc:/etc/tor/torrc
```

Hopefully you know a thing or two about Docker and Compose. Notice the shared `volumes`. We need to configure a file called `torrc` contained in the `config` directory located in the `tor-proxy` directory. But before we can do this, we need to know the name of the application container we want to provide as a hidden Tor service...

Execute:

```
docker ps
```

You'll see something similar to this:

```
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS               NAMES
19e55d540706        cryptoshoppingcart_node   "node ./app.js"          20 hours ago        Up 13 hours         3000/tcp            cryptoshoppingcart_node_1
ed3924fb55d2        catatnight/postfix        "/bin/sh -c '/opt/in…"   20 hours ago        Up 13 hours         25/tcp              cryptoshoppingcart_postfix_1
e93b0dce86dd        mongo                     "docker-entrypoint.s…"   20 hours ago        Up 13 hours         27017/tcp           cryptoshoppingcart_mongo_1
```

The `NAMES` column is where you find your crypto-shopping-cart hostname. In this case, it is `cryptoshoppingcart_node_1`. This means the contents of `config/torrc` must look like this:

```
HiddenServiceDir /home/user/.tor/hidden_app_1/
HiddenServicePort 80 cryptoshoppingcart_node_1:3000
```

Execute the `tor-proxy` container:

```
docker-compose up -d
```

Assuming all went well, you can obtain your new `.onion` address like this:

```
docker-compose exec tor cat /home/user/.tor/hidden_app_1/hostname
```

## Retrieving orders

The crypto-shopping-cart's Tor-safe deployment ensures all outgoing emails are intercepted. Emails intended for customers will need to be manually copied and pasted off of your hidden service server and relayed manually through an anonymous mail service like [mail2tor.com](http://mail2tor.com).

For server side email management, I use `mutt`:

```
sudo apt install mutt
```

Intercepted mail is deposited into the `crypto-shopping-cart/mailorders` directory. To see the captured mail, execute the following from the `crypto-shopping-cart/` directory:

```
sudo mutt -f mailorders/root
```

# Future Work

How do we preserve both anonymity and build trust?

It's a ways off yet, but I plan to incorporate Ethereum smart contracts into the order process. My early goal is to withhold payment until Canada Post reports that a package has been delivered.

If you want to take a stab at it, contributions are welcome!

