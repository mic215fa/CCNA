# Next Hop

tags: #concept #acting-ccna #routing

> [!info] Concept role
> [[Routing Table]] 的 forwarding-instruction 子概念：指出 packet 下一個交付的 Layer 3 neighbor。

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

Next hop 必須能透過另一條 route 解析成可用的 [[Exit Interface]]。在 Ethernet link 上，router 還需要將 next-hop IPv4 address 解析成 destination MAC，才能建立 egress frame。

## Contrast With

- [[Exit Interface]]：next hop 回答「交給哪台 neighbor」；exit interface 回答「從本機哪個介面離開」。
- [[Default Gateway]]：default gateway 是 host 對 remote networks 使用的預設 next hop；router route entries 可以為不同 prefixes 使用不同 next hops。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-186_687_1415_181_222.jpg)
*Source: [[00_Source/Chapter 10 - 封包的一生]], Figure 10.3 — R1 performs a route lookup and uses ARP to learn the next hop MAC address.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
