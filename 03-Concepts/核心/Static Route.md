# Static Route

tags: #concept #acting-ccna #routing

> [!info] Concept role
> IP routing 的核心 route source：由管理者明確指定 destination prefix 與 forwarding instruction。

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

- [[Default Route]]
- [[Proxy ARP]]

## Mechanism

常見設定形式包括指定 next-hop IP、指定 exit interface，或同時指定兩者。只設定單向 static route 不一定能雙向通訊，回程路由也需要存在。

## Static Route Forms

### Recursive Static Route

只指定 next-hop IPv4 address：

```text
ip route <network> <mask> <next-hop-ip>
```

Router 必須再次查 routing table，找出如何到達 next hop，因此稱為 recursive lookup。它清楚描述「交給誰」，但依賴 next hop 本身可由另一條 route 解析。

### Directly Attached Static Route

只指定本地 exit interface：

```text
ip route <network> <mask> <exit-interface>
```

它描述「從哪裡出去」。在 point-to-point link 上通常較直觀；在 Ethernet multiaccess network 上，router 可能對大量 destination addresses 執行 ARP，並依賴 [[Proxy ARP]]，因此需要特別理解平台 forwarding behavior。

### Fully Specified Static Route

同時指定 exit interface 與 next-hop IP：

```text
ip route <network> <mask> <exit-interface> <next-hop-ip>
```

它同時回答「從哪裡出去」與「交給誰」，在 multiaccess link 上比只指定 exit interface 更明確。

### Default Static Route

Destination 使用 `0.0.0.0 0.0.0.0`，形成 [[Default Route]]；它仍是 static route，只是 prefix 為 `/0`。

## Installation Dependencies

- Destination network 與 mask 必須形成正確 prefix。
- Next hop 必須可以 recursive resolve，或 exit interface 必須可用。
- 設定存在不保證一定出現在 routing table；依賴的 forwarding information 不成立時，route 可能無法安裝或使用。

## Reachability Is Bidirectional

```text
Forward route
Source → Destination

Return route
Destination side → Source network
```

Ping 與大多數 session 都需要回程。只在 R1 設定前往 remote LAN 的 route，不能保證 remote router 知道如何回到 R1 的 source LAN。

## Failure Model

| 錯誤 | 結果 |
|---|---|
| Destination/mask 錯誤 | Route 不 match 預期 traffic，或 match 錯誤範圍 |
| Next hop 不可達 | Recursive lookup 失敗 |
| Exit interface 錯誤 | Packet 從錯誤 link 送出或無法封裝 |
| 缺少 return route | 單向 traffic 可離開，但 reply 無法返回 |
| 過度依賴 `/0` | Specific reachability 問題被 default path 掩蓋 |

## Verification

```text
show running-config | include ^ip route
show ip route static
show ip route <destination>
show ip interface brief
show ip arp
ping <next-hop>
traceroute <destination>
```

## Contrast

- Static route vs connected route：static 由管理者輸入；connected 隨 interface address/up-state 自動出現。
- Static route vs dynamic route：static 可預測且無 protocol exchange，但不會自動學習 topology changes。
- Next hop only vs exit interface only vs fully specified：分別強調下一台 router、本地出口，或同時明確指定兩者。

## Knowledge Maps

- [[04_Maps/Unit04-RouterSwitch-Packet 知識地圖|Routing Table、Static Route 與 Packet Flow Map]]

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-173_488_1400_828_201.jpg)
*Source: [[00_Source/Chapter 9 - 路由基礎]], Figure 9.9 — Static routes configured on R1, R2, and R3 to enable two-way communication between PC1 and PC3.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]

## Questions

- 為什麼 PC1 到 PC3 的 static route 只設定去程不夠？
- 為什麼 Ethernet multiaccess link 上只指定 exit interface 可能引發大量 ARP 或 Proxy ARP 依賴？
- Recursive static route 的 next hop 在什麼條件下才能成功解析？
