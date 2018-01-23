title: Ethereum Mining Fundamentals
date: 2017-11-14 12:35:47
tags:
- manual
---

So you've got yourself a rig... now what?

# Rig Placement

An Ethereum mining rig is a depreciating asset. Though it may be assembled from the most cutting-edge, energy efficient components, the requirements of the Ethereum platform are always growing. Any rig you assemble will be quickly outdated. If you hope to earn returns from your investment, you need to get your rig up and operational ASAP.

There are a few important things to consider when finding a home for your rig:

1. Temperature
2. Network connectivity
3. Dust
4. Amperage

Ideally, your rig should be located in a cool, clean, low traffic, well-ventilated room with a wired internet connection. It is unwise to operate your rig in extremely low, or sub-zero temperatures. Also, be aware that a typical household circuit puts out 15 amps. You'll find out pretty quick if your rig exceeds the circuit's capacity (the breaker will trip). 

For home-based miners, a clean basement storage room is ideal. Naturally, it must be equipped with outlets for both power and internet. Wireless connections to the Ethereum network have not been tested and as such, cannot be recommended. Place your rig on a stable surface off the floor and away from the the wall (10 cm clearance should be ample). This will help ensure adequate airflow and extend the life of your GPUs.

Remember: the longer your rig is able to mine, the more likely you are to earn real profits. The decision as to where to place your rig is not one to be taken lightly.

# Rig Operation

With the mining hardware safely situated and powered on, it's time to join a mining pool! 

In a nutshell, pooled mining focuses the resources of many miners on finding the ultimate hash on the block currently being mined. To prove that your rig is doing its work, it will submit _shares_ to the pool. Once the block has been successfully mined, payout is usually determined by the number of contributed shares.

There are many Ethereum mining pools from which to choose. Generally pool operators make it as easy to join as possible. Assuming an Ubuntu 16.04.3 Server installation with all third-party dependencies installed, two programs are required:

1. [ethereum](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu)
2. [ethminer](https://github.com/ethereum-mining/ethminer)

`ethereum` provides the implementation of the Ethereum protocol. It contains a program called `geth`, which is our window into the Ethereum network (among other things). 

`ethminer` coordinates your rig's GPUs and talks to the mining pool.

For all that follows, you need access to the mining rig's commandline. You can either attach a monitor and keyboard directly to the machine, or use `ssh` to login remotely.

## Create a new account

You need an account to keep track of your ethers:

```
geth account new
```

You will be asked for a _passphrase_. Write it down somewhere safe. If you lose your passphrase, you will not be able to unlock your account. All ethers contained therein will be lost forever.

Having provided and verified a passphrase, you will have created a _keyfile_ nested in your home directory. The keyfile, like your passphrase, is very important. Execute `ls -al ~/.ethereum/keystore`, and you should see it:

```
total 12
drwx------ 2 kamea kamea 4096 Nov  8 19:10 ./
drwx------ 4 kamea kamea 4096 Nov  9 23:18 ../
-rw------- 1 kamea kamea  491 Nov  8 19:10 UTC--2017-11-09T02-09-59.653286045Z--d24def0856636050cf891befc0fa69ecf96c160b
```

It is very important to keep this file safe. Once your rig is up and running, make a copy of that file for storage in a safe location. If you ever want to get at your ethers, you'll need your passphrase and this keyfile. Don't lose either (you can even print it to paper, if you like).

## Start syncing the blockchain

This isn't strictly necessary, but experience shows that your GPUs hashrates will get a slight performance boost if the Ethereum blockchain is syncing in the background. I use `screen` to execute `geth` because it makes it easier to take a peak at what's going on from an SSH session:

```
screen -dmS geth /usr/bin/geth --rpc --rpcaddr "0.0.0.0"
```

To bring this process to the foreground, execute `screen -x geth`. To return it to the background, hold `Ctrl-a-d`.

## Choose a mining pool

There are a bunch to choose from, and from what I've seen, most strive to be transparent and reputable. [EtheriumPool.co](https://ethereumpool.co/) was the first one I tried, and will use it for this example.

## Connect to the pool

This is where [ethminer](https://github.com/ethereum-mining/ethminer) comes in. Assuming the [standalone executable](https://github.com/ethereum-mining/ethminer/releases) has been unpacked in your home directory, you can verify that it's working by executing:

```
./ethminer/bin/ethminer --help
```

### Benchmark your rig

EtheriumPool.co requires you to specify your hashrate. This can be a bit finicky. Try executing:

```
./ethminer/bin/ethminer -m -G
```

If this command is successful, it will display your total hashrate. Make note of this value, because you'll need it later.

If you get errors or a 0Mh/s rate, try again. Sometimes it's helpful to benchmark the individual GPUs one at a time before benchmarking them all together:

```
./ethminer/bin/ethminer -M -G --opencl-device 0
```

Assuming a 6 GPU rig, the value assigned to the `--opencl-device` param can be 0-5.  In any case, you need a reasonably accurate estimation of your hashrate. It's of no advantage to set it too high or low, though if in doubt, round down.

### Bring `ethminer` online

The general command format looks like this:

```
./ethminer/bin/ethminer -G -F http://ethereumpool.co/?miner=[HASHRATE IN MHASH]@[WALLET ADDRESS]@[OPTIONAL RIG NAME]
```

`-G` means _mine with GPUs_.

`-F` means put the rig into _farm mode_. This requires the address of the pool with the connection string as depicted above.

You obtain your _HASHRATE IN MHASH_ from the benchmarking step described above.

Your _WALLET ADDRESS_ was displayed when you first created your account with `geth`. If you forgot, you can find it in the `~/.ethereum/keystore` directory. Execute `cat` on the keyfile you created (it probably starts with `UTC--`). Copy the value associated with the `address` key in that file.

The _OPTIONAL RIG NAME_ is not required, but does present an opportunity to bring a bit of your own personality to the mining pool.

A real-life example looks like this:

```
./ethminer/bin/ethminer -G -F http://ethereumpool.co/?miner=116@0xd24def0856636050cf891befc0fa69ecf96c160b@dbiddy_in_tha_heezy
```

Given the length and complexity of the command, it's a good idea to execute it within a `bash` script.

Once it's all up and running, you may see some `JSONRPC`-type errors. These errors often pertain to statistical reporting. If you are up and running on a new rig or a new account, there will be no statistics to report. These errors usually resolve themselves, though it may take a couple of hours as your rig's reputation builds.

# Done!

If all went well, you are now mining Ethereum. EthereumPool.co keeps excellent stats. You can see the details of your contribution (and everyone else's) on the site.

