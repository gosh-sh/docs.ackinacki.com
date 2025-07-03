---
description: How it works and why it is recommended to use it
---

# Proxy service

## Overview

**Broadcast Proxy** — is a specialized network service designed to optimize block exchange between participants in the Acki Nacki network. Its primary purpose is to reduce overall network traffic between nodes and to improve the scalability and stability of the network.

## How the Proxy Works

The IP addresses of the Proxies are registered on the network using the Gossip protocol and will be stored on-chain in the future.

Each BK node **without Proxy:**

* Sends blocks to all the other BK nodes' Proxies and directly to the BK nodes without Proxies
* Receives blocks  from all the other BK nodes' Proxies and directly from the BK nodes without Proxies

Each BK node **that is connected to the network via Proxy**:

* Sends blocks only to its designated Proxy.
* Receives blocks only from its designated Proxy.

Each **Proxy**:

* Forwards blocks from its own BK nodes to Proxies of other node providers and BK nodes without Proxies.
* Receives blocks from other Proxies and nodes without Proxies and broadcasts them to its own BK nodes.

## Why Everyone Should Connect via a Proxy

In the network, there may be from several hundred to thousands of nodes, and the number of threads (and accordingly, Block Producers) and the load on them can dynamically change. A Block Producer must deliver its blocks to every Block Keeper, and each Block Keeper, in turn, must receive blocks from all Block Producers.

### What does this mean for the Block Keeper’s network requirements?

It implies two key points:

*   Incoming data volume is equal to BLOCK\_SIZE × 3 × THREADS\_NUMBER.

    Assuming an average block with 300 transactions is \~100 KB, and the network card limit is 1 Gbit (125 MB/s), the maximum number of threads a Block Keeper with such a card can handle is:

    125 / (0.1 × 3) = 416.
*   Outgoing data volume depends on the number of nodes in the network.

    In a non-proxied network, the outgoing traffic equals BLOCK\_SIZE × 3 × NODES\_NUMBER.

    In a 250-node network, this results in 75 MB = 0.6 Gbit.

    If the network grows to 500 nodes, this exceeds 1 Gbit.

    Furthermore, if multiple nodes are deployed in a single datacenter, router limitations on outbound traffic and internet bandwidth costs must be considered — without proxies, all traffic is routed via the public internet, making it inefficient and expensive for all participants.

The Proxy Service aggregates all or part of the outbound traffic from its datacenter and forwards it directly to other proxies, reducing inter-datacenter traffic by a factor equal to the number of Block Keeper nodes behind the proxy in that datacenter.

{% hint style="warning" %}
Therefore, if a node operator runs more than one node, it is strongly recommended to run them behind a proxy to optimize traffic across the network.\

{% endhint %}

## Proxy network requirements

The network requirements for a proxy are equal to the network requirements of a single Block Keeper multiplied by the number of Block Keepers behind it.

For example, for 100 nodes behind a proxy, the required bandwidth is 100 Gbit.

## Fault Tolerance and limitations

{% hint style="danger" %}
For fault tolerance, **you must run at least two instances** of the Proxy service in each data center: **one master, and one failover.**
{% endhint %}

{% hint style="warning" %}
It is **not recommended** to run more than **100 BK nodes** behind a Proxy instance.
{% endhint %}

## Proxy System requirements

<table><thead><tr><th width="155">Configuration</th><th width="483.1015625">Recommended</th></tr></thead><tbody><tr><td>CPU</td><td>8c/16t</td></tr><tr><td>RAM</td><td>32 GB</td></tr><tr><td>Storage</td><td>500 GB NVMe</td></tr><tr><td>Network</td><td><p>1Gb *(multiply) the number of nodes behind proxy.  </p><p>For example, for 100 nodes behind a proxy, the required bandwidth is 100 Gbit.</p></td></tr></tbody></table>

## Proxy Deployment

Instructions for deploying a Proxy service are available [here](https://github.com/ackinacki/ackinacki/tree/main?tab=readme-ov-file#deployment-with-ansible-1).
