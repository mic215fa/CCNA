# Layer 2 Segmentation

tags: #concept #acting-ccna #vlan #layer2

## Definition

Layer 2 Segmentation 是用 VLAN 等機制分隔 Ethernet forwarding / flooding 範圍，建立多個 broadcast domains。

## Why It Exists

即使 hosts 已在不同 IP subnets，如果 switch 仍把它們放在同一 broadcast domain，broadcast traffic 仍會跨群組擴散。Layer 2 segmentation 解決這個問題。

## Related Concepts

- [[VLAN]]
- [[Broadcast Domain]]
- [[Access Port]]
- [[Trunk Port]]

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-219_974_1197_826_318.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.3 — Hosts are in different subnets but still share one Layer 2 broadcast domain without VLANs.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-220_873_1321_1052_223.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.4 — VLANs divide the switch into separate broadcast domains.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
