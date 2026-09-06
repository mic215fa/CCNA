# Default Route

tags: #concept #acting-ccna #routing

> [!info] Concept role
> [[Routing Table]] 的 fallback 子概念：以 `/0` match 所有 IPv4 destinations，但只在沒有更 specific route 時使用。

## Definition

Default Route 是 `0.0.0.0/0` route；當 routing table 中沒有更 specific 的 match 時，router 使用它作為 fallback path。

## Why It Exists

Router 不可能在所有情境中手動列出整個 Internet 的 routes；default route 讓 unknown destinations 可以被送往上游或 Internet edge。

## Prerequisites

- [[Routing Table]]
- [[Route Selection]]

## Related Concepts

- [[Static Route]]
- [[Next Hop]]
- [[Exit Interface]]

## Mechanism

`0.0.0.0/0` 包含所有 IPv4 addresses，但因為 prefix length 最短，所以只有在沒有更 specific route 時才會被選中。

Default route 可以由 [[Static Route]] 建立，也可能由 routing protocol 學習；「default」描述 prefix 與 fallback 角色，不等於一定是 static。

## Failure Model

- Default route 指向錯誤 next hop：所有未知 destinations 被集中送錯方向。
- 只有 default route、缺少必要 internal specific route：內部 traffic 可能被送往上游。
- 上游缺少 return route：forward direction 使用 default 成功，reply 仍無法返回。

## Verification

- `show ip route`：尋找 `0.0.0.0/0` 與 gateway of last resort。
- `show ip route <destination>`：確認實際 selected route，而不是看到 default 就假設一定使用它。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-177_668_1414_1043_188.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Figure 9.11 — R1 has specific routes to internal networks and one default route to the internet.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
