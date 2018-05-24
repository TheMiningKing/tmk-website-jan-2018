title: AMD should give you your money back
date: 2018-05-24 12:57:39
tags:
- amd
- review
---

Here's the core issue: the AMD RX580s do not work as advertised on the box. Specifically, they draw way too much power. 

I've built two Ether-centric 6 GPU mining rigs:

1. [aloha](/2017/11/09/Ethereum-Mining-Rig-Prototyping-and-Market-Investigation/), online November 9, 2017
2. [bula](/2017/12/16/Hello-Bula-6-GPU-Ethereum-Rig/), online December 16, 2017

Both have power supply issues. As such, both are currently offline.

A shout out to [Memory Express](https://www.memoryexpress.com/) in Dalhousie! They have been dutiful in fulfilling their end of the warranty coverage deal...

# aloha

After a couple of rounds of diagnosis, Memory Express determined the aloha PSU to be working. As trouble-shooting progressed, a definite problem was found in one of the PCIe slots on the motherboard. I do not suspect this has anything to do with why the PSU keeps flaking out.

To their credit, Memory Express did do a 6 GPU power supply stress test on Windows. This means nothing to me, because I'm using Ubuntu 16.04, [which is supported by AMD](https://www.amd.com/en/support/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580). I also have an [issue](https://community.amd.com/thread/225188) on their support forum. It has been flagged as _answered_, but no answers have been provided.

# bula

The bula PSU never had enough juice to support 6 GPUs in the first place. She's been mining for roughly 5 months on 5 GPUs (approx. 1.2 ETH). Over the last few weeks, the number of sustainable GPUs fell from 5 to 4 to 1. I mined enough for a Dwarfpool payout and shut her down.

### 2018-5-24

I brought the bula PSU to Memory Express for diagnosis. Initial testing with a Thermaltake Power Supply Tester showed no issues. The friendly fellow behind the counter said that the device only measured voltages (or something), so more testing was needed. 

The friendly fellow got a little less friendly when I started talking possible refunds on all the equipment they sold me. Though flustered, he remained composed and I believe he understands my position... i.e., AMD RX580 GPUs don't work as advertised on Ubuntu.

[Here's how everything is configured](https://github.com/TheMiningKing/ethereum-miner-bula). There have been no BIOS mods whatsoever. All components have been used within the limits of the manufacturers specifications. All components are still under warranty.

Something's up... as much as I'd like to see aloha and bula up and mining, it may be time to politely request a refund.


