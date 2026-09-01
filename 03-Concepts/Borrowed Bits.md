# Borrowed Bits

tags: #concept #acting-ccna #subnetting

## Definition

Borrowed Bits 是從 IPv4 host portion 借出來、改作 subnet/network portion 的 bits。

## Why It Exists

原本的 address block 只有一個 network boundary；借 bits 可以建立更多 subnet boundaries，讓同一個 address block 分給多個 networks 使用。

## Prerequisites

- [[Network Portion and Host Portion]]
- [[Prefix Length]]
- [[Binary Number System]]

## Related Concepts

- [[Subnetting]]
- [[FLSM]]
- [[VLSM]]

## Mechanism

借 1 bit 可產生 2 個 subnets；借 2 bits 可產生 4 個 subnets。一般來說，借 n bits 可產生 `2^n` 個 subnets。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-194_569_1161_1361_348.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.2 — Borrowing 1 host bit from 192.168.1.0/24 creates two /25 subnets.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-195_577_1108_873_384.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.3 — Borrowing 2 host bits creates four /26 subnets.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
