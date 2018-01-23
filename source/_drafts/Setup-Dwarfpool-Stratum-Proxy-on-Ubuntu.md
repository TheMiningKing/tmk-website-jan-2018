title: Buying and Selling Currency with Ethereum
date: 2017-12-22 15:34:58
tags:
- research
---


Clone:

```
git clone https://github.com/Atrides/eth-proxy.git
```

Configure:

```
vi eth-proxy/eth-proxy.conf
```

Change the following:

```
WALLET = "0xF7a5A3c18e0f81d853874d404d20a1900037CCdb"^M
ENABLE_WORKER_ID = True
LOG_TO_FILE = False
```

Install dependencies:

```
sudo apt install python-twisted
```

Start proxy:

```
(cd eth-proxy/ && python eth-proxy.py)
```

You can test by visiting `http://192.168.2.9:8080` in a browser.



