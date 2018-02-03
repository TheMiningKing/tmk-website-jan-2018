title: 2Miners.com - an Ethereum Mining Pool Setup and Review
date: 2018-02-03 09:10:39
tags:
- review
- ethereum
- mining
---

Nick from [2miners.com](https://2miners.com) reached out to the Mining King a couple of weeks back. He wants to promote his mining pool, and I'm happy to oblige by giving it a test run.

A new pool is a welcome change of pace for me, because it's easy to fall into a mining pool rut. Don't fix what ain't broke, afterall.

So far I've had success mining with:

- [ethereumpool.co](https://ethereumpool.co/)
- [dwarfpool.com](http://dwarfpool.com)

EthereumPool is relatively small. I once mined for a week without any credit to my account. This wasn't their fault, it's just that the entire pool never found a block within that timeframe.

Dwarfpool, by comparison, generates a predictable, steady flow of ether. No complaints here, though my block rewards don't seem to be worth as much these last few weeks (presumably because of the influx of new Ethereum miners looking to cash in).

## It's 2Miners turn!

The benchmark I'm applying is very simple. I'm mining to make money. Currently, I'm drawing 0.1 ETH from Dwarfpool every ten days on the [bula](/2017/12/16/Hello-Bula-6-GPU-Ethereum-Rig/) rig.

Let's see if I can do better mining Ethereum on 2Miners.com...

## But first!

Remember the Mining King's motto: _Only suckers pay for software_.

The [Claymore Dual Miner](https://github.com/nanopool/Claymore-Dual-Miner) is all the rage these days and is the software [recommended](https://eth.2miners.com/#/en/help) by 2Miners. It's supposed to be more cost-effective than free mining software like [ethminer](https://github.com/ethereum-mining/ethminer), even with the royalty they charge.

With all love and respect to the Claymore people, homie don't play that!

That said, deviating from the norm can invite more work upon yourself. It was a bit tricky connecting to [eth.2miners.com](https://eth.2miners.com/). For all would-be mining kings, this is what finally got `ethminer` working under [this Ubuntu system configuration](https://github.com/TheMiningKing/ethereum-miner-bula)...

### Configuration and connection

Like most miners, I like to set everything up in a script. In its simplest form, my script looks like this:

```
#!/bin/bash

####################
# `2miners-start.sh`
####################

export GPU_FORCE_64BIT_PTR=1
export GPU_MAX_HEAP_SIZE=100
export GPU_USE_SYNC_OBJECTS=1
export GPU_MAX_ALLOC_PERCENT=100
export GPU_SINGLE_ALLOC_PERCENT=100

# Plain connection
ethminer --farm-recheck 200 -SP 1 -G -S eth.2miners.com:2020 -O 0x7e5533116dbd23b113d3288aacbf4d2122f88ad3.bula
```

This allows me to start mining by simply executing:

```
./2miners-start.sh
```

#### Break it down...

Experienced miners will recognize the script above as a fairly typical configuration. Those with sharp eyes might see something slightly out of the ordinary...

Notice:

```
export GPU_FORCE_64BIT_PTR=1
```

Most mining pools recommend you set this variable like so:

```
export GPU_FORCE_64BIT_PTR=0
```

Set this way (i.e., the _wrong_ way for 2Miners), `ethminer` will report:

```
  ✘  11:04:01|cl-1      clCreateCommandQueue ( -6 )
  ✘  11:04:01|cl-2      OpenCL Error: clEnqueueWriteBuffer -36
  ✘  11:04:01|cl-1      OpenCL Error: clEnqueueWriteBuffer -36
  m  11:04:03|ethminer  Speed   0.00 Mh/s    gpu/0  0.00  gpu/1  0.00  gpu/2  0.00  gpu/3  0.00  gpu/4  0.00  [A0+0:R0+0:F0] Time: 00:00
  ℹ  11:04:03|stratum   Received new job #0x3968c3  seed: #3aa8f28cac16bdd858f2a726a06d1217  target: #0000000112e0be826d694b2e
  m  11:04:05|ethminer  Speed   0.00 Mh/s    gpu/0  0.00  gpu/1  0.00  gpu/2  0.00  gpu/3  0.00  gpu/4  0.00  [A0+0:R0+0:F0] Time: 00:00
  m  11:04:07|ethminer  Speed   0.00 Mh/s    gpu/0  0.00  gpu/1  0.00  gpu/2  0.00  gpu/3  0.00  gpu/4  0.00  [A0+0:R0+0:F0] Time: 00:00
```

You'll see those `OpenCL Error: clEnqueueWriteBuffer -36` messages and the GPUs will stick at `0.00 Mh/s`.

Also make note of the _stratum protocol_ option (`-SP`) on the `ethminer` command:

```
ethminer --farm-recheck 200 -SP 1 -G -S eth.2miners.com:2020 -O 0x7e5533116dbd23b113d3288aacbf4d2122f88ad3.bula
```

That `-SP 1` option is similarly critical to getting up and mining on 2Miners. Set incorrectly (to `0` or `2`), `ethminer` will report this error:

```
  ℹ  10:59:36|stratum   Connecting to stratum server eth.2miners.com:2020
  ℹ  10:59:36|stratum   Connected to stratum server eth.2miners.com : 2020
  ℹ  10:59:36|stratum   Subscribed to stratum server
  ✘  10:59:36|stratum   Read response failed: End of file
  ℹ  10:59:36|stratum   Reconnecting in 3 seconds...
```

### In a nutshell

If you're going to use `ethminer` on 2Miners, there are two points of which to be mindful:

1. The `GPU_FORCE_64BIT_PTR` environment variable (needs to be set to `1`)
2. And `ethminer`'s stratum protocol option (set like so `-SP 1`)

## Review

So, with a bit of trial and error, the _bula_ rig is ready to join the 2Miners Ethereum mining pool. After my next Dwarfpool payout, I'm going to plug in and see how long it takes to earn 0.1 ETH. If it takes less than ten days, 2Miners will be declared the _winner_ and I'll fall back into a nice, comfortable mining pool rut.

### Check back soon!


