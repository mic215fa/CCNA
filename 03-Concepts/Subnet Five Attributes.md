# Subnet Five Attributes

tags: #concept #acting-ccna #subnetting #ipv4

## Definition

Subnet Five Attributes 是 CCNA subnetting 題常要求計算的五個值：network address、broadcast address、first usable address、last usable address、maximum number of hosts。

## Why It Exists

它把 subnetting 從抽象 bit 切分轉成可直接用於 IP 配置、路由與驗證的具體結果。

## Prerequisites

- [[IPv4 Network Address]]
- [[IPv4 Broadcast Address]]
- [[Usable IPv4 Address Range]]
- [[Prefix Length]]

## Related Concepts

- [[Subnetting]]
- [[FLSM]]
- [[VLSM]]
- [[Magic Number Method]]

## Mechanism

Network address 是 host bits 全 0；broadcast address 是 host bits 全 1；first/last usable 分別是 network+1 與 broadcast-1；maximum hosts 通常是 `2^host_bits - 2`。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-196_636_1353_946_223.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.4 — Five attributes of the 192.168.1.64/26 subnet.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
