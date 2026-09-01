# Static Route

tags: #concept #acting-ccna #routing

## Definition

Static Route 是由管理者手動設定的 route，用來告訴 router 如何前往特定 destination network。

## Why It Exists

Router 預設只知道直接連接的 networks；若要前往遠端 networks，需要靜態設定或後續 dynamic routing protocol 提供 route。

## Prerequisites

- [[Routing Table]]
- [[Route Selection]]
- [[Next Hop]]
- [[Exit Interface]]

## Related Concepts

- [[Recursive Static Route]]
- [[Fully Specified Static Route]]
- [[Default Route]]
- [[Proxy ARP]]

## Mechanism

常見設定形式包括指定 next-hop IP、指定 exit interface，或同時指定兩者。只設定單向 static route 不一定能雙向通訊，回程路由也需要存在。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-173_488_1400_828_201.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Figure 9.9 — Static routes configured on R1, R2, and R3 to enable two-way communication between PC1 and PC3.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]

## Questions

- 為什麼 PC1 到 PC3 的 static route 只設定去程不夠？
