# Subnetting

tags: #concept #acting-ccna #subnetting #ipv4

## Aliases

- IPv4 subnetting
- 子網劃分

## Definition

Subnetting 是把一個 IPv4 address block / network 切成多個較小 subnets 的方法。

## Why It Exists

單一大型 network 通常無法符合實務上的部門、地點、broadcast domain 或 routing boundary 需求；subnetting 讓管理者能把 address space 分配給多個較小 networks。

## Prerequisites

- [[IPv4 Address]]
- [[Prefix Length]]
- [[Netmask]]
- [[Network Portion and Host Portion]]
- [[Binary Number System]]

## Related Concepts

- [[Borrowed Bits]]
- [[FLSM]]
- [[VLSM]]
- [[Subnet Five Attributes]]
- [[Magic Number Method]]
- [[VLAN]]

## Mechanism

Subnetting 透過增加 prefix length、向 host portion 借 bits 形成新的 network/subnet portion。每多借 1 bit，subnets 數量加倍，但每個 subnet 的 address 數量減半。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-193_807_1391_902_190.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.1 — 192.168.1.0/24 divided into smaller subnets as prefix length increases.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Questions

- 為什麼 prefix length 每增加 1 bit，subnet 數會加倍但每個 subnet 的大小會減半？
