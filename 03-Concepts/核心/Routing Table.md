# Routing Table

tags: #concept #acting-ccna #routing

> [!info] Concept role
> IP routing 的核心狀態：保存 destination prefixes 與可用 forwarding instructions。

## Definition

Routing Table 是 router 已知目的地與 forwarding 指示的資料庫。

## Why It Exists

Router 需要根據 destination IP address 決定 packet 下一步要往哪裡送；routing table 提供這些決策依據。

## Prerequisites

- [[Router]]
- [[IPv4 Address]]
- [[Network Portion and Host Portion]]

## Related Concepts

- [[Route Selection]]
- [[Connected Route]]
- [[Local Route]]
- [[Static Route]]
- [[Default Route]]
- [[Next Hop]]
- [[Exit Interface]]

## Mechanism

Router 收到送給自己的 frame 後，取出 IPv4 packet，查看 destination IP，並由 [[Route Selection]] 在 routing table 中尋找 matching route。若沒有可用 route，packet 會被 drop。

## Concept Structure

```text
Routing Table（核心狀態）
├── Route Sources
│   ├── Connected Route：interface 所連的 network
│   ├── Local Route：router 自己的 interface address
│   ├── Static Route：管理者手動建立
│   └── Dynamic Route：後續由 routing protocol 學習
├── Route Selection（核心決策）
│   └── Longest Prefix Match
└── Forwarding Instructions
    ├── Exit Interface
    ├── Next Hop
    └── Exit Interface + Next Hop
```

## Route Entry Anatomy

一條可用 route 至少要回答部分或全部問題：

| Field | 回答的問題 |
|---|---|
| Destination prefix | 哪些 destination IP 由這條 route match？ |
| Source/code | Route 從 connected、local、static 或 routing protocol 哪裡來？ |
| Prefix length | Match 有多 specific？ |
| Next hop | Packet 下一個交給哪台 Layer 3 device？ |
| Exit interface | Packet 從本 router 哪個 interface 離開？ |

並非所有 entry 都同時顯示 next hop 與 exit interface。例如 local route 表示目的地是 router 本身；connected route 已直接指出 egress network。

## Population

- Interface configured and up/up → 產生 [[Connected Route]] 與 [[Local Route]]。
- `ip route ...` → 建立 [[Static Route]] 或 [[Default Route]]。
- Dynamic routing protocol → 未來可學習 dynamic routes。

Routing table 是結果，不是來源本身。設定 static route、介面恢復或 routing protocol 收斂，都可能改變 table 內容。

## Lookup vs Forwarding

```text
Destination IP
  ↓ match routing-table prefixes
Route Selection
  ↓ choose longest prefix
Next hop / exit interface
  ↓ resolve Layer 2 neighbor when required
Build new frame and forward
```

找到 route 不等於 packet 一定能成功送達；interface state、next-hop reachability、ARP、return route 與下游 routing 都可能繼續造成失敗。

## Failure Model

| 現象 | 優先檢查 |
|---|---|
| 沒有 matching route | Connected/static/dynamic route 是否存在，是否需要 default route |
| Route 存在但 next hop 無法解析 | Next hop 是否 on-link、recursive lookup、ARP 與 interface state |
| 去程成功、回程失敗 | Remote router 是否有 return route |
| 選到非預期 route | Destination prefix 與 longest-prefix match |
| Connected route 消失 | Interface address、administrative/line protocol state |

## Verification

```text
show ip route
show ip route <destination>
show ip route connected
show ip route local
show running-config | include ^ip route
show ip interface brief
show ip arp
```

排錯應區分三層：route 是否安裝、route 是否被選中、選中的 forwarding instruction 是否可實際執行。

## Contrast

- Routing table vs forwarding action：table 保存可達資訊；router 對每個 packet 執行 lookup 並 forward。
- [[Connected Route]] vs [[Local Route]]：前者代表直接連接的 subnet；後者代表 router 自己的 interface IP。
- Specific route vs [[Default Route]]：default `/0` match 所有 IPv4 destinations，但只在沒有更 specific match 時使用。
- Routing table vs ARP cache：前者把 destination prefix 導向 next hop/interface；後者把 on-link next-hop IPv4 address 解析成 MAC。

## Knowledge Maps

- [[04_Maps/Unit04-RouterSwitch-Packet 知識地圖|Routing Table、Static Route 與 Packet Flow Map]]

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-165_578_1429_181_190.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Section 9.2.1 — Routing table output after configuring router interfaces.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-169_818_1227_392_318.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Figure 9.6 — R1 receives a packet and selects the best route for that packet.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]

## Unit05 Additions

- Inter-VLAN routing creates or uses routes between VLAN subnets.
- [[Switch Virtual Interface]]s on a [[12.4-Multilayer Switch]] can create connected and local routes in the switch routing table, similar to router interfaces.

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Questions

- 為什麼 routing table 已有 route，packet 還是可能無法到達 destination？
- Connected route 與 local `/32` route 為什麼需要同時存在？
- Route lookup、next-hop resolution 與 frame forwarding 分別回答哪一個問題？
