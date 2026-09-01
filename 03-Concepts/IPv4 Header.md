# IPv4 Header

tags: #concept #acting-ccna #ipv4 #layer3

## Definition

IPv4 Header 是 IPv4 packet 的 Layer 3 header，包含支援 end-to-end delivery 的欄位，例如 source/destination address、TTL、protocol、checksum 等。

## Why It Exists

Router 需要根據 Layer 3 header 進行 forwarding；目的端也需要 header 欄位判斷 packet 應如何處理。

## Prerequisites

- [[Network Layer]]
- [[IPv4 Address]]
- [[Header and Trailer]]
- [[Protocol Data Unit]]

## Related Concepts

- [[Time To Live]]
- [[Packet Fragmentation]]
- [[Payload]]
- [[Forwarding]]

## Mechanism

IPv4 header 通常最小 20 bytes，若使用 Options 最多可到 60 bytes。Unit03 主要重點是 source/destination IPv4 addresses、TTL、Protocol、Total Length 等欄位如何支援 Layer 3 delivery。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-116_245_637_185_350.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.2 — IHL indicates IPv4 header length, while Total Length indicates the whole packet length; packet is encapsulated in a Layer 2 frame.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Unit04 Additions

- During [[Packet Life Cycle]], routers examine the destination IPv4 address in the [[IPv4 Header]] for [[Route Selection]].
- The Layer 3 source/destination IP addresses remain end-to-end, while Layer 2 MAC addresses change at each hop.
- Routers decrement [[Time To Live]] as they forward packets.

## Additional Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
