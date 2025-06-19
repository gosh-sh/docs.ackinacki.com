# Proxy service

## Overview

**Broadcast Proxy** — is a specialized network service designed to optimize block exchange between participants in the Acki Nacki network. Its primary purpose is to reduce overall network traffic between nodes and to improve the scalability and stability of the network.

## How the Proxy Works

The IP addresses of the Proxies are registered on the network using the Gossip protocol and will be stored on-chain in the future.

Each BK node:

* Sends blocks only to its designated Proxy.
* Receives blocks only from its designated Proxy.

Each **Proxy**:

* Forwards blocks from its own BK nodes to Proxies of other providers.
* Receives blocks from other Proxies and broadcasts them to its own BK nodes.

{% hint style="danger" %}
For fault tolerance, **you must run at least two instances** of the Proxy service in each data center: **one master, and one failover.**
{% endhint %}

{% hint style="warning" %}
It is **not recommended** to run more than **100 BK nodes** per such Proxy server pair.
{% endhint %}

### System requirements

<table><thead><tr><th width="155">Configuration</th><th width="483.1015625">Recommended</th></tr></thead><tbody><tr><td>CPU</td><td>8c/16t</td></tr><tr><td>RAM</td><td>32 GB</td></tr><tr><td>Storage</td><td>500 GB NVMe</td></tr><tr><td>Network</td><td>100 Gbit full-duplex synchronous unmetered Internet connection</td></tr></tbody></table>

## Proxy Deployment

Instructions for deploying a Proxy service are available [here](https://github.com/ackinacki/ackinacki/blob/main/proxy/README.md).
