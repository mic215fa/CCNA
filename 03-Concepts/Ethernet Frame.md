# Ethernet Frame

tags: #concept #acting-ccna #ethernet #layer2

## Definition

Ethernet Frame 是 [[Data Link Layer]] 的 PDU，包含 Ethernet header、trailer 與 payload。

## Why It Exists

Frame 提供 Layer 2 傳遞所需資訊，讓 [[Switch]] 能根據 destination [[MAC Address]] 在 LAN 內轉送資料。

## Prerequisites

- [[Ethernet]]
- [[Data Link Layer]]
- [[MAC Address]]
- [[Protocol Data Unit]]

## Related Concepts

- [[Preamble and SFD]]
- [[EtherType]]
- [[Frame Check Sequence]]
- [[Payload]]
- [[Frame Forwarding]]
- [[Frame Flooding]]

## Mechanism

Ethernet header 主要包含 Destination、Source、Type/Length；trailer 包含 [[Frame Check Sequence]]。Source MAC 協助 switch learning，Destination MAC 協助 switch forwarding。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-097_384_1031_891_320.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.2 — Ethernet header and trailer fields, including Destination, Source, Type/Length, and FCS.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Unit05 Additions

- When an Ethernet frame crosses an 802.1Q trunk as non-native VLAN traffic, an [[IEEE 802.1Q Tag]] is inserted into the frame.
- VLAN tagging lets switches preserve VLAN identity across trunk links.

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
