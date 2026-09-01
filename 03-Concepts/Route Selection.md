# Route Selection

tags: #concept #acting-ccna #routing

## Definition

Route Selection 是 router 根據 destination IP address，在 [[Routing Table]] 中選擇最佳 route 的過程。

## Why It Exists

同一個 destination IP 可能被多條 route match；router 需要一致規則決定使用哪一條 route。

## Prerequisites

- [[Routing Table]]
- [[IPv4 Address]]
- [[Prefix Length]]

## Related Concepts

- [[Connected Route]]
- [[Static Route]]
- [[Default Route]]
- [[Next Hop]]
- [[Exit Interface]]

## Mechanism

Chapter 9 主要強調 longest prefix match：更 specific 的 route 比較不 specific 的 route 優先。例如 /32 比 /24 更 specific，/24 又比 /0 default route 更 specific。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-169_818_1227_392_318.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Figure 9.6 — Router selects the best route for a received packet.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
