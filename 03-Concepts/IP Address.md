# IP Address

tags: #concept #acting-ccna #network-fundamentals #layer3

## Aliases

- Internet Protocol address
- IP 位址

## Definition

IP Address is a Layer 3 address used to identify the final destination host for end-to-end delivery.

## Why It Exists

The original source host needs a way to address a message to the final destination host, not merely to the next hop.

## Prerequisites

- [[Network Layer]]

## Related Concepts

- [[MAC Address]]
- [[Router]]
- [[IPv4 Address]]
- IPv6
- [[Address Resolution Protocol]]
- [[Network Portion and Host Portion]]

## Mechanism

The destination IP address remains the same as a message travels from source host to destination host, while the destination MAC address changes at each hop.

Unit03 / Chapter 7 補充：IPv4 address 是 32-bit IP address，以四個 [[Octet]] 的 [[Dotted Decimal Notation]] 表示，並透過 [[Prefix Length]] 或 [[Netmask]] 區分 network portion 與 host portion。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-066_394_1260_1366_350.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.3 — PC1 addresses a message to SRV1's IP address; Layer 3 handles end-to-end delivery.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-123_261_1300_1144_318.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.7 — IPv4 address shown in dotted decimal and binary with network and host portions.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit02-TCPIP-網路模型 REVIEW|Unit02-TCPIP-網路模型 REVIEW]]
