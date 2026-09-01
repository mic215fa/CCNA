# Forwarding

tags: #concept #acting-ccna #network-fundamentals

## Aliases

- 轉送

## Definition

Forwarding means sending a message to the next node in the path to the destination, whether that node is the final destination or the next router.

## Why It Exists

Messages often cannot reach the final destination in one step. Network devices must forward messages along the path.

## Prerequisites

- [[Node]]
- [[Hop]]
- [[Data Link Layer]]

## Related Concepts

- [[Router]]
- [[Switch]]
- [[MAC Address]]
- [[IP Address]]
- [[Frame Forwarding]]
- [[Frame Flooding]]
- [[IPv4 Header]]

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-065_339_1165_1637_318.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.2 — Layer 2 forwarding moves a message from hop to hop toward SRV1.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-131_527_1414_181_192.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.11 — PC1 sends a packet to PC3 via R1; Layer 2 MAC destination changes at each segment while Layer 3 destination remains PC3.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit02-TCPIP-網路模型 REVIEW|Unit02-TCPIP-網路模型 REVIEW]]

## Unit04 Additions

- Unit04 distinguishes Layer 2 frame forwarding by switches from Layer 3 packet forwarding by routers.
- Router forwarding depends on [[Routing Table]], [[Route Selection]], [[Next Hop]], and [[Exit Interface]].
- Switch forwarding depends on [[MAC Address Table]] and [[Ethernet Frame]] destination MAC address.

## Additional Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
