# Trunk Port

tags: #concept #acting-ccna #vlan #trunk

## Definition

Trunk Port 是能在同一 physical link 上承載多個 VLAN traffic 的 switch/router interface。

## Why It Exists

若兩台 switches 都有多個 VLAN，為每個 VLAN 拉一條獨立 access link 會浪費 ports 與線材；trunk link 用一條 link 承載多個 VLAN。

## Prerequisites

- [[VLAN]]
- [[IEEE 802.1Q Tag]]

## Related Concepts

- [[Access Port]]
- [[Allowed VLAN List]]
- [[Native VLAN]]
- [[12.4-Router on a Stick]]

## Mechanism

Switch 送 frame 出 trunk port 時通常加入 802.1Q tag，接收端根據 tag 判斷 VLAN。Native VLAN traffic 通常例外，不加 tag。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-225_744_1366_183_190.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.5 — SW1 and SW2 connected by a trunk link carrying traffic in multiple VLANs.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
