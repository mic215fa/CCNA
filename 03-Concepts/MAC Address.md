# MAC Address

tags: #concept #acting-ccna #network-fundamentals #layer2

## Aliases

- Media Access Control address
- MAC 位址

## Definition

MAC Address is a Layer 2 network address assigned to each port of a device, used to address messages to the next hop.

## Why It Exists

Layer 2 needs an addressing method for hop-to-hop delivery over the local medium.

## Prerequisites

- [[Data Link Layer]]
- [[Hop]]

## Related Concepts

- [[IP Address]]
- [[Switch]]
- [[Router]]
- [[Ethernet Frame]]
- [[MAC Address Table]]
- [[Address Resolution Protocol]]

## Mechanism

At each hop, the destination MAC address changes to the MAC address of the next hop. This contrasts with the destination IP address, which remains the same through the journey.

Unit03 / Chapter 6 補充：MAC address 是 6 bytes / 48 bits，通常以 12 個 hexadecimal characters 表示。Switch 使用 source MAC learning 建立 [[MAC Address Table]]，並依 destination MAC forwarding frame。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-065_339_1165_1637_318.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.2 — At each hop, the message is addressed to the next hop's MAC address.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-066_394_1260_1366_350.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.3 — The destination MAC address changes at each hop, while the destination IP address remains the same.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-097_384_1031_891_320.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.2 — Ethernet frame Destination and Source fields contain MAC addresses.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
