title: Ethereum Mining Rig Prototyping and Market Investigation
date: 2017-11-09 16:55:04      
tags:
- research
---

# Abstract

This report presents summary findings of my investigation into the Ethereum crypto currency market. It was propelled by the construction and deployment of a custom-built Ethereum mining rig . The [Ethereum Blockchain App Platform](https://ethereum.org/) is fascinating technology with untold potential. Establishing a leadership position in blockchain industries means diversifying investments across infrastructure and Ethereum-based services. Establishing a demand for these services, naturally, is key to overall industry success. Though this investigation was primarily focused on participating in the mining process itself, a broader understanding of the industry quickly emerged.

# Industry Overview

The blockchain landscape is evolving at a dizzying pace. Success in any capacity means constantly striving to be on the leading edge.

Naturally, the very nature and success of Ethereum-based industry depends on how the technology is applied. Infrastructure requirements grow as demand for Ethereum services grow. Ultimately it will be the adoption of services that determine the success of the platform, though the opportunity to profit from infrastructure cannot be ignored.

## Infrastructure

Apart from manufacturing computer hardware, _mining_ currently is the most obvious way to profit from the the Ethereum platform. That said, the market is volatile and this situation will likely change soon anyway...

The term _mining_ as applied to Ethereum reflects conventional mining in that the acquired resource (_ether_ tokens) are only valuable if there is demand. At the moment, Ethereum depends upon mining as a way to maintain the integrity of the blockchain. Successfully mining a block results in the generation of 5 ether (or _gas_). This ether, in turn, is how services built upon the platform _pay_ for the use of distributed computational resources. 

The current method of blockchain verification (through mining) is termed _Proof of Work_. Ethereum may soon switch to a different method called _Proof of Stake_. This will mean that the burden of maintaining the integrity of the blockchain is offloaded to stakeholders who build their services on the Ethereum platform. This looks to be a [gradual process](https://www.coindesk.com/ethereums-big-switch-the-new-roadmap-to-proof-of-stake/) which may result in a complete conversion or some hybrid POS/POW type system.

Whatever the case, investment in infrastructure fills an essential role in the delivery of Ethereum services. Whether this infrastructure is applied to mining tokens to validate the blockchain, or in maintaining stakeholder blockchains, participation is incentivized.

## Application

[Ethereum](https://ethereum.gitbooks.io/frontier-guide/content/ethereum.html) bills itself as:

- A World Computer
- An Internet Service Platform
- Opt-in Social Contracts
- Part of the Decentrailzation Revolution

As a platform for internet services and contracts, opportunities to build the industry are found in the problems that Ethereum can potentially solve. Any investment in this area must be informed by this simple metric.

From my perspective, the most immediate and accessible problem that Ethereum could solve pertains to the establishment of contracts. A blockchain contract mitigates the need for legal consultation. As the limits of the Ethereum platform are pushed, peripheral business services like accounting and payroll could potentially be eliminated.

Potential blockchain applications are beyond the scope of this report. This will require future investigation.

# Aloha, 6x 8GB GPU Ethereum Mining Rig

The rig pictured below is hand-made and comprised entirely of locally-sourced components. 

{% asset_img aloha-mining-rig.jpg Aloha %}

## Hardware

The components were chosen with an eye toward electrical efficiency.

- Custom built aluminum frame
- Gigabyte H110-D3a BTC ATC motherboard
- Intel Core i3 71000 3.9GHz CPU
- 4G Kingston D.HyperX FuryBK RAM
- 120 GB Kingston A400 SSD
- EVGA Supernova 1200W Platinum power supply
- 6x 8G MSI Radeon RX 580 GPUs

The 6 GPUs are cutting edge and supported with partially open source drivers.

## Software

The software chosen for the system is almost exclusively open source. The Radeon GPUs are the only exception. Though the base operating system, Ubuntu 16.04, offers unprecedented open source support from the vendor, certain functionality can only be obtained in the proprietary _wrapper_ around the open source drivers. This is a regrettable, but improving situation in the Linux world of graphic card drivers.

Out of the box, without optimization or over-clocking, solo mining efforts with this rig easily produce 117Mh/s hashrates. 

### Specs

- Ubuntu Server 16.04.3
- Linux kernel v4.13
- [AMDGPU-PRO 17.40](http://support.amd.com/en-us/kb-articles/Pages/AMDGPU-PRO-Install.aspx)
- [ethereum](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum)
- [ethminer](https://github.com/ethereum-mining/ethminer)

# Conclusion

It comes as no surprise that the overall industry success of the Ethereum platform depends upon establishing demand for Ethereum-based services. Successful players will be those who educate consumers about blockchain technologies and establish demand in doing so. Though services will ultimately make or break the platform, investment in infrastructure and application go hand-in-hand in the diversified blockchain investment portfolio. Services depend on infrastructure and the trend toward decentralized computing networks necessitates the proliferation of physical equipment among organizations and individuals.

## Future Work

Further investigation into the nuts-and-bolts details surrounding services built on the Ethereum platform is required. That is, we must uncover the pragmatic issues pertaining to development, deployment, usability, and cost. Establishing and enforcing a simple services-rendered contract is a potential first step in that direction.

