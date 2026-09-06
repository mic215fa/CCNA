# IPv4 Address

tags: #concept #acting-ccna #ipv4 #layer3

> [!info] Concept role
> [[IPv4 Addressing]] 的核心：描述 IPv4 interface 的 32-bit Layer 3 address 與 network/host boundary。

## Definition

IPv4 Address 是 32-bit Layer 3 address，用來識別 host 或 router interface 在 IPv4 network 中的位置。

## Why It Exists

Layer 3 需要 end-to-end address，讓 packet 可以跨越 LAN，由 router 在不同 network 間轉送。

## Prerequisites

- [[IP Address]]
- [[Network Layer]]
- [[Binary Number System]]
- [[IPv4 Addressing#Representation|Octet]]

## Related Concepts

- [[IPv4 Header]]
- [[IPv4 Addressing#Representation|Dotted Decimal Notation]]
- [[Prefix Length]]
- [[Netmask]]
- [[Network Portion and Host Portion]]
- [[Address Resolution Protocol]]
- [[Router]]

## Mechanism

IPv4 address 以四個 8-bit octets 的 dotted-decimal notation 表示。Address 必須搭配 [[Prefix Length]] 或 [[Netmask]] 才能劃分 network portion 與 host portion；同一 subnet 內 interfaces 共享 network portion，而可指派的 host portion 必須唯一。

## Address Interpretation

```text
IPv4 address + prefix
  ↓ bitwise network boundary
Network address / host identifier
  ↓
Local-vs-remote decision and route-prefix matching
```

## Depends On

- [[IPv4 Addressing#Representation|32-bit／octet representation]]
- [[Prefix Length]] 或 [[Netmask]]
- [[Network Portion and Host Portion]]

## Leads To

- [[IPv4 Network Address]]、[[IPv4 Broadcast Address]] 與 [[Usable IPv4 Address Range]]
- [[Subnetting]]、route-prefix matching 與 address planning

## Common Confusions

- Interface address 與 network address 不同；例如 `192.168.1.10/24` 屬於 `192.168.1.0/24`。
- `/24` 是 prefix length，不是 address 的一部分；它描述如何解讀 32 bits。
- Destination IPv4 address 通常維持端到端意義，Ethernet destination MAC 則逐 link 改變。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-123_261_1300_1144_318.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.7 — IPv4 address split into network portion and host portion using a prefix length.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-124_445_1374_1275_225.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.8 — Two LANs use different IPv4 network portions and communicate through router R1.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼同一 LAN 內 hosts 要有相同 network portion，但不同 host portion？

## Unit05 Additions

- Unit05 extends [[IPv4 Address]] from address representation into address planning through [[Subnetting]].
- Subnetting changes the boundary between network/subnet bits and host bits by increasing [[Prefix Length]].
- VLAN design often pairs one IPv4 subnet with one [[VLAN]], but this is a design pattern rather than an absolute rule.

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Knowledge Maps

- [[04_Maps/IPv4 Addressing 基礎地圖|IPv4 Addressing 與 Subnetting 地圖]]
