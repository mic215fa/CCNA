# Magic Number Method

tags: #concept #acting-ccna #subnetting

## Definition

Magic Number Method 是用 subnet mask 中的 interesting octet 快速找出 subnet increment / boundary 的 subnetting 計算方法。

## Why It Exists

CCNA 題目常要求快速判斷某個 IP 屬於哪個 subnet；magic number method 能減少完整 binary 展開的時間。

## Prerequisites

- [[Subnetting]]
- [[Netmask]]
- [[Prefix Length]]

## Related Concepts

- [[Subnet Five Attributes]]

## Mechanism

通常以 `256 - interesting_octet_mask_value` 得到 block size，再用該 block size 找 network boundaries、broadcast address 與 usable range。

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## REVIEW

- Chapter 11 提到此方法作為 additional practice；目前保留為考試技巧，後續可補完整範例。
