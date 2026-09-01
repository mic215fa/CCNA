# VLAN

tags: #concept #acting-ccna #vlan #layer2

## Aliases

- Virtual LAN
- Virtual Local Area Network
- 虛擬區域網路

## Definition

VLAN 是在同一 physical switching infrastructure 中建立的 virtual LAN；每個 VLAN 形成獨立的 Layer 2 broadcast domain。

## Why It Exists

只用 subnetting 做 Layer 3 segmentation 不一定能阻止 Layer 2 broadcast 在 shared switch 上擴散。VLAN 讓 switch 只在同一 VLAN 內 forward/flood frames。

## Prerequisites

- [[Switch]]
- [[Ethernet Frame]]
- [[Broadcast Domain]]
- [[Layer 2 Domain]]

## Related Concepts

- [[Layer 2 Segmentation]]
- [[Layer 3 Segmentation]]
- [[Access Port]]
- [[Trunk Port]]
- [[VLAN ID]]
- [[Inter-VLAN Routing]]

## Mechanism

Switch ports 被指派到不同 VLAN 後，switch 不會在 VLAN 之間直接 forward/flood frames；不同 VLAN 的 hosts 必須透過 Layer 3 routing 才能互通。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-220_873_1321_1052_223.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.4 — A physical switch divided into three virtual switches / VLANs, each a separate broadcast domain.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Questions

- 為什麼 VLAN 可以降低 broadcast domain 範圍，但不會自動提供 VLAN 間通訊？
