# Broadcast Domain

tags: #concept #acting-ccna #layer2 #broadcast

## Definition

Broadcast Domain 是其中任一成員送出的 broadcast frame 會被其他成員收到的設備集合。

## Why It Exists

Broadcast domain 決定 ARP request 這類 broadcast traffic 能傳到哪些 hosts，也影響 LAN 邊界與網路規模。

## Related Concepts

- [[Broadcast Frame]]
- [[Layer 2 Domain]]
- [[LAN]]
- [[Switch]]
- [[Router]]

## Mechanism

同一 switch 或直接互連 switches 上的 hosts 通常在同一 broadcast domain；router 通常分隔 broadcast domains。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-108_698_1401_183_225.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.6 — All devices shown are in the same broadcast domain for the ARP request.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Unit05 Additions

- [[VLAN]] makes broadcast domain boundaries configurable on switches.
- Chapter 12 shows that IP subnets alone do not necessarily stop Layer 2 broadcast flooding if all hosts remain in the same VLAN/broadcast domain.
- Each VLAN functions as a separate broadcast domain.

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
