# Local Route

tags: #concept #acting-ccna #routing

> [!info] Concept role
> [[Routing Table]] 的自動 route-source 子概念：以 `/32` 代表 router 自己的 IPv4 interface address。

## Definition

Local Route 是代表 router interface 自己 IP address 的 route，通常以 /32 prefix 出現在 routing table。

## Why It Exists

它讓 router 明確識別「送到這個 interface 自己 IP」的 traffic，而不是把它視為整個 connected network 的一般 host traffic。

## Prerequisites

- [[Connected Route]]
- [[Routing Table]]
- [[Prefix Length]]

## Related Concepts

- [[IPv4 Address]]
- [[Route Selection]]

## Mechanism

設定 interface IP 後，IOS 會為 connected network 建立 connected route，也會為 interface 自己的 IP 建立 local /32 route。

## Why `/32`

Local route 只 match 一個 address，因此使用最 specific 的 `/32`。送往該 address 的 packet 應由 router 本機處理，而不是當作 connected subnet 中另一個 host 的 traffic 繼續轉送。

## Contrast With

- [[Connected Route]]：connected route 把整個 subnet 導向 interface；local route 識別 interface 本身。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-168_337_1199_350_350.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Figure 9.5 — R1 G0/0 IP address represented as a /32 local route.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
