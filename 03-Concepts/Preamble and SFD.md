# Preamble and SFD

tags: #concept #acting-ccna #ethernet #layer1

## Aliases

- Start Frame Delimiter
- SFD

## Definition

Preamble and SFD 是每個 Ethernet frame 前送出的 Layer 1 位元序列，用來讓接收端同步 clock 並準備接收 frame。

## Why It Exists

接收設備需要判斷 incoming electrical signals 中每個 bit 的精確長度；Preamble / SFD 幫助接收端完成同步。

## Related Concepts

- [[Ethernet Frame]]
- [[Physical Layer]]
- [[Bit and Byte]]

## Contrast With

- 它們會跟 frame 一起送出，但不被視為 Ethernet frame 的一部分，因為它們是 Layer 1 功能，不影響 Layer 2 forwarding decision。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-097_384_1031_891_320.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.2 — Preamble and SFD are shown before the Ethernet frame fields but are not considered part of the frame.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

