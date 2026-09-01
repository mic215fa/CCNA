# Next Hop

tags: #concept #acting-ccna #routing

## Definition

Next Hop 是 packet 前往目的地途中，下一個應交給的 Layer 3 device，通常以下一台 router 的 IP address 表示。

## Why It Exists

Router 不需要知道整條路徑的每個細節；它只需要知道下一步要交給誰，讓 packet 一跳一跳往目的地前進。

## Prerequisites

- [[Router]]
- [[Routing Table]]
- [[Address Resolution Protocol]]

## Related Concepts

- [[Exit Interface]]
- [[Static Route]]
- [[Default Gateway]]
- [[Packet Life Cycle]]

## Mechanism

Router 做 route lookup 後，若 route 指向 next-hop IP，router 會透過 ARP 學習該 next-hop IP 的 MAC address，將 packet 封裝到送往 next hop 的 frame 中。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-186_687_1415_181_222.jpg)
*Source: [[00_Source/Chapter 10 - 封包的一生]], Figure 10.3 — R1 performs a route lookup and uses ARP to learn the next hop MAC address.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
