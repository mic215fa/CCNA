# Duplex

tags: #concept #acting-ccna #interface #ethernet

## Aliases

- Half duplex
- Full duplex
- 雙工模式

## Definition

Duplex 描述一條 Ethernet link 是否能同時傳送與接收資料。Half duplex 不能同時雙向傳輸；full duplex 可以同時傳送與接收。

## Why It Exists

在 shared medium 或 hub 環境中，設備必須避免同時傳送造成 collision；在 switch-based link 中，每個 host 通常有自己的 collision domain，因此 full duplex 更有效率。

## Related Concepts

- [[Collision Domain]]
- [[CSMA-CD]]
- [[Autonegotiation]]
- [[Duplex Mismatch]]
- [[Interface Error]]

## Mechanism

若 link 兩端 duplex 不一致，可能仍然 up，但會產生 collisions、late collisions、input errors 等症狀。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-150_533_1433_177_220.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Section 8.1.3 — Manually configured full duplex shown in interface status output.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
