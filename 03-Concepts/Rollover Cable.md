# Rollover Cable

tags: #concept #acting-ccna #cabling #cli

## Aliases

- Console Cable
- 反轉線

## Definition

Rollover Cable 是用來連接 PC 與 RJ45 console port 的 console cable，其 pin 對應完全反轉：1↔8、2↔7、3↔6、4↔5。

## Why It Exists

它讓電腦可以透過 RJ45 console port 直接存取 Cisco device CLI，進行本機管理與初始設定。

## Related Concepts

- [[Console Port]]
- [[Cisco IOS CLI]]
- [[Straight-through and Crossover Cable]]

## Contrast With

- [[Straight-through and Crossover Cable]]：用於 Ethernet network traffic。
- Rollover Cable：用於 console 管理，不用於一般 Ethernet LAN 傳輸。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-080_316_759_266_360.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], Figure 5.4 — Rollover cable wiring for connecting a PC to an RJ45 console port.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

