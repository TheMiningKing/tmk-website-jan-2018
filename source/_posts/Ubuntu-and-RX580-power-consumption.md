title: Ubuntu and RX580 power consumption
date: 2018-01-06 10:47:41
tags:
- rx580
- ubuntu
- wattage
---

It came as a shock to the good folk down at [Memory Express](https://www.memoryexpress.com/), but the RX580 GPUs draw roughly 200-250W each when running under Ubuntu 16.04. Under Windows, a 1200W power supply is sufficient. Under Ubuntu, the rig will reboot after 5-10 minutes when power requirements outpace the PSU. 


Sadly (and _gladly!_), with [Ethereum](https://coinmarketcap.com/) trading as high as C$1300, miners have cleaned out stock in Calgary. Memory Express isn't expecting new 1500W PSUs until February and I can't fit two smaller units in my existing frames (see the specs on my [aloha](/blog/2017/11/09/Ethereum-Mining-Rig-Prototyping-and-Market-Investigation/) and [bula](/blog/2017/12/16/Hello-Bula-6-GPU-Ethereum-Rig/) rigs).

The rigs are stable with 5 of 6 GPUs attached. Each card is getting roughly 22 Mh/s under [current software configurations](https://github.com/TheMiningKing). This is all in accordance with manufacturer specs (no BIOS mods).

Apart from adding a PSU, options include:

- Installing Windows (not a chance)
- Undervolting (difficult in Ubuntu, but apparently it's possible)
- Repurposing two GPUs (mining [Monero](https://getmonero.org/) might be interesting)
- Waiting for driver updates (preferred)

_aloha_ and _bula_ are chugging along just fine at 108 Mh/s at five-sixths capacity. Ether is being mined, but not at the pace anyone would like (0.013-15 ETH/day on Dwarfpool).

{% asset_img 5-gpus-at-the-wall.jpg "5 Red Dragon RX580s. Power consumption at the wall" %}

Interestingly, the Red Dragon RX580s draw slighly less current.

{% asset_img aloha-and-bula.jpg "aloha and bula, together at last" %}

Sister rigs bunking in a salvaged server cabinet.
