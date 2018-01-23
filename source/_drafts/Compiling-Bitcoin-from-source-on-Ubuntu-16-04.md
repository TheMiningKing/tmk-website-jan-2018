title: Compiling Bitcoin from source on Ubuntu 16.04
date: 2018-01-06 12:49:25
tags:
- bitcoin
- wallets
- ubuntu
---

I want to create a Bitcoin wallet without a third-party application. With Ethereum I can do this with [geth](https://github.com/ethereum/go-ethereum/wiki/geth). With Bitcoin, I can use [bitcoin-cli](https://github.com/bitcoin/bitcoin). This is how I compiled it from source...



```
mkdir -p bitcoin-build/src && cd bitcoin-build/src
git clone https://github.com/bitcoin/bitcoin.git
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
echo '12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef  db-4.8.30.NC.tar.gz' | sha256sum -c
```

The last command should produce this:

```
db-4.8.30.NC.tar.gz: OK
```

Install BerkeleyDB:

```
tar -xvf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix
mkdir -p build
BDB_PREFIX=$(pwd)/build
../dist/configure --disable-shared --enable-cxx --with-pic --prefix=$BDB_PREFIX
make install
cd ../..
#sudo apt install autoconf libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libqt4-dev libqrencode-dev libtool
sudo apt install libboost-all-dev libevent-dev
```

# Compile Bitcoin Core

```
cd bitcoin
#git checkout v0.10.0
./autogen.sh
./configure CPPFLAGS="-I${BDB_PREFIX}/include/ -O2" LDFLAGS="-L${BDB_PREFIX}/lib/"
make
```









# Build the project

The build instructions for Ubuntu can be found in `doc/build-unix.md`. The documenation isn't terrible, but there are a couple of things that are easy to miss.

First generate the configuration script: 

```
./autogen.sh
```

This is where the documentation is a bit murky (at least for me). If you run:

```
./configure
```

You might get an error that looks like this:

```
configure: error: libdb_cxx headers missing, Bitcoin Core requires this library for wallet functionality (--disable-wallet to disable wallet functionality)
```

The whole reason I'm doing this is because I want to create a Bitcoin wallet, so obviously this will never do.

## Dependencies

Bitcoin has a bunch of software dependencies that can be met with this:

```
sudo apt install libboost-all-dev libevent-dev
```

### BerkeleyDB

Bitcoin uses an older version of BerkeleyDB. This is a pain, but it ensures wallet compatability between different installations:

```
sudo apt install software-properties-common
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt update
sudo apt install libdb4.8-dev libdb4.8++-dev
```

There are newer versions of this software, but Bitcoin wants `db4.8` specifically.

# Configure

With the BerkeleyDB dependencies installed, we need to tell the `configure` script where to find the packages:

```
./configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
```

# Make

```
make
```



