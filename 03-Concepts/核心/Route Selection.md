# Route Selection

tags: #concept #acting-ccna #routing

> [!info] Concept role
> IP routing 的核心決策：對 packet destination IP 選擇最 specific 的 matching route。

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

## Longest Prefix Match

Router 對 destination IP 與每條 route prefix 進行 match，再選擇 match bits 最長的 route。

```text
Destination 192.168.10.37

0.0.0.0/0        matches
192.168.0.0/16   matches
192.168.10.0/24  matches ← selected
```

Route source 不是 longest-prefix decision 的第一順位。只要 prefixes 長度不同，更 specific prefix 優先，即使另一條 route 是 connected、static 或 default。相同 prefix 的多來源比較涉及 administrative distance；同一來源內的 path 比較可能涉及 metric，留待 dynamic-routing 單元擴充。

## Decision Stages

1. 找出所有 match destination IP 的 prefixes。
2. 選擇 longest prefix。
3. 若同一 prefix 有多個候選來源，routing process 決定哪些 routes 能安裝進 table。
4. 讀取 selected entry 的 next hop／exit interface。
5. 若需要，recursive lookup next hop 並解析 Layer 2 neighbor。

## Common Confusions

- `/0` 不是「優先權最高」，而是最不 specific 的 fallback。
- Longest prefix match 比的是 destination bits，不是 route 寫在 table 的先後順序。
- Route 已安裝與 packet 最終成功是不同問題；next hop、interface 與 return path 仍需成立。

## Failure Indicators

- 預期 route 存在，但更 specific 的錯誤 route 把 traffic 引到其他方向。
- Default route 掩蓋「缺少 specific route」的事實，使 packet 被送往錯誤上游。
- Next-hop route 消失，使只指定 next hop 的 static route 無法解析。

## Knowledge Maps

- [[04_Maps/Unit04-RouterSwitch-Packet 知識地圖|Routing Table、Static Route 與 Packet Flow Map]]

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-169_818_1227_392_318.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Figure 9.6 — Router selects the best route for a received packet.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
