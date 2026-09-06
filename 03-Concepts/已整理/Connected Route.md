# Connected Route

tags: #concept #acting-ccna #routing

> [!info] Concept role
> [[Routing Table]] 的自動 route-source 子概念：代表 operational Layer 3 interface 直接連接的 subnet。

## Definition

Connected Route 是 router 因為某個 interface 直接連接到某個 network，而自動加入 routing table 的 route。

## Why It Exists

Router 需要知道哪些 networks 是自己直接連到的，才能直接把 packet 從相應 interface 送出。

## Prerequisites

- [[Network Interface]]
- [[IPv4 Address]]
- [[Routing Table]]

## Related Concepts

- [[Local Route]]
- [[Exit Interface]]
- [[Route Selection]]

## Mechanism

當 router interface 設定 IP address、啟用且 line protocol up，IOS 會加入 connected route。該 route 指向整個 connected network。

## Operational Dependency

Connected route 同時依賴 address/prefix 與 interface up/up。設定仍留在 running-config，但若 interface 或 line protocol down，對應 route 可能從 routing table 消失。

## Contrast With

- [[Local Route]]：connected route 代表整個 on-link subnet；local route 只代表 router 自己的 interface address。
- [[Static Route]]：connected route 由 interface state 自動產生；static route 由管理者明確輸入。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-166_246_1411_1294_222.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Section Connected routes — Routing table showing directly connected networks and their interfaces.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
