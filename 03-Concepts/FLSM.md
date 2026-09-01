# FLSM

tags: #concept #acting-ccna #subnetting

## Aliases

- Fixed-Length Subnet Masking

## Definition

FLSM 是所有 subnets 都使用相同 subnet mask / prefix length 的 subnetting 方法。

## Why It Exists

它讓 subnetting 規則簡單、一致，適合所有子網大小需求相同或考試練習中的固定大小切割。

## Prerequisites

- [[Subnetting]]
- [[Borrowed Bits]]
- [[Prefix Length]]

## Related Concepts

- [[VLSM]]
- [[Subnet Five Attributes]]

## Mechanism

給定 address block 後，選擇要借幾個 host bits，所有產生的 subnets 都使用同一個新的 prefix length。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-203_520_1167_1330_316.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.10 — 172.25.190.0/23 divided into four equal /25 subnets using FLSM.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Contrast With

- [[VLSM]]：不同 subnets 可以使用不同 prefix length，地址使用更有效率。
