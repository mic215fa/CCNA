# Broadcast Frame

tags: #concept #acting-ccna #switching #layer2

## Definition

Broadcast Frame 是 destination MAC address 為 `ffff.ffff.ffff` 的 Ethernet frame，目標是同一 LAN / broadcast domain 內所有 hosts。

## Why It Exists

某些機制需要向同一 LAN 的所有 hosts 詢問或通知，例如 [[Address Resolution Protocol]] 的 ARP request。

## Related Concepts

- [[Frame Flooding]]
- [[Broadcast Domain]]
- [[Address Resolution Protocol]]
- [[Unicast Frame]]

## Mechanism

Switch 會 flood broadcast frames，讓同一 broadcast domain 內的所有 hosts 都收到。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-108_698_1401_183_225.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.6 — ARP request is sent to the broadcast MAC address and flooded through the LAN.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

