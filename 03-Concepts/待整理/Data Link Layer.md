# Data Link Layer

tags: #concept #acting-ccna #network-fundamentals #layer2

## Aliases

- Layer 2
- L2

## Definition

Data Link Layer prepares data for transmission over the physical medium and provides hop-to-hop delivery of messages.

## Why It Exists

訊息從 source 到 destination 的路徑可能經過多個 intermediate nodes；Layer 2 負責把訊息送到下一個 [[Hop]]。

## Prerequisites

- [[Physical Layer]]
- [[MAC Address]]
- [[Hop]]

## Related Concepts

- [[Ethernet]]
- [[Switch]]
- [[Router]]
- [[Network Layer]]
- [[Protocol Data Unit]]
- [[Ethernet Frame]]
- [[MAC Address Table]]
- [[Address Resolution Protocol]]

## Mechanism

At each hop, the Data Link Layer addresses the message to the next hop's [[MAC Address]]. In encapsulation, it adds a Layer 2 header and trailer, forming a frame.

Unit03 / Chapter 6 補充：Data Link Layer 在 Ethernet LAN 中具體表現為 [[Ethernet Frame]]、[[MAC Address Table]]、[[MAC Address Learning]]、[[Frame Forwarding]] 與 [[Frame Flooding]]。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-065_339_1165_1637_318.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.2 — Layer 2 forwards a message hop to hop; each hop uses the next hop's MAC address.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-073_630_670_190_316.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.7 — A packet encapsulated in a Layer 2 header/trailer is a frame (L2PDU).*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-097_384_1031_891_320.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.2 — Ethernet frame fields used for Layer 2 forwarding and error detection.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit02-TCPIP-網路模型 REVIEW|Unit02-TCPIP-網路模型 REVIEW]]
