# Network Layer

tags: #concept #acting-ccna #network-fundamentals #layer3

## Aliases

- Layer 3
- L3

## Definition

Network Layer provides end-to-end delivery of messages from the original source host to the final destination host.

## Why It Exists

Layer 2 可以把訊息送到下一個 hop，但 source host 還需要一種方式指定 final destination host。這是 Layer 3 的角色。

## Prerequisites

- [[Data Link Layer]]
- [[IP Address]]

## Related Concepts

- [[Router]]
- [[Protocol Data Unit]]
- [[Transport Layer]]
- [[IPv4 Address]]
- [[IPv4 Header]]
- [[Time To Live]]

## Mechanism

Layer 3 uses [[IP Address]] to address a message to the destination host. The destination IP address remains the same throughout the journey, while the destination MAC address changes at each hop.

Unit03 / Chapter 7 補充：Network Layer 在 IPv4 中具體表現為 [[IPv4 Header]] 與 [[IPv4 Address]]，router 使用 Layer 3 header 資訊進行 forwarding。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-066_394_1260_1366_350.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.3 — Layer 3 is responsible for end-to-end delivery; the destination IP address remains the same while the destination MAC address changes at each hop.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-073_630_670_190_316.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.7 — A segment encapsulated in a Layer 3 header is a packet (L3PDU).*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-116_245_637_185_350.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.2 — IPv4 header is the Layer 3 header inside a packet, which must be encapsulated in a Layer 2 frame.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
