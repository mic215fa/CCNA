# Routed Port

tags: #concept #acting-ccna #switching #routing #interface

## Definition

Routed Port 是 multilayer switch 上被設定成像 router interface 一樣運作的 physical port。

## Why It Exists

Multilayer switch 除了在 LAN 內做 inter-VLAN routing，也需要連到外部 network；routed port 提供 Layer 3 point-to-point / uplink interface。

## Prerequisites

- [[12.4-Multilayer Switch]]
- [[Network Interface]]
- [[IPv4 Address]]

## Related Concepts

- [[Switch Virtual Interface]]
- [[Routing Table]]
- [[Inter-VLAN Routing]]

## Mechanism

在 interface configuration mode 使用 `no switchport`，使該 physical port 不再作為 Layer 2 switchport，而可直接設定 IP address。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-242_772_1417_349_220.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.12 — Multilayer switch uses G0/1 as a routed port to connect to external networks via R1.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-243_987_1241_1023_316.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.13 — LAN host reaches an external destination through a multilayer switch routed port.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
