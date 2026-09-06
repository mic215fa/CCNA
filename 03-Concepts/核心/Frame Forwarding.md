# Frame Forwarding

tags: #concept #acting-ccna #switching #layer2

> [!info] Concept role
> [[Ethernet Switching]] 的決策子機制：依 destination MAC 與 VLAN context 選擇 forward、filter 或 flood。

## Definition

Frame Forwarding 是 switch 將 frame 從正確 port 送往 destination MAC address 所在方向的動作。

## Why It Exists

Forwarding 讓 switch 能把 LAN 內資料有效送到目標，而不必每次都讓所有 hosts 收到。

## Prerequisites

- [[MAC Address Table]]
- [[Ethernet Frame]]
- [[MAC Address]]

## Related Concepts

- [[Frame Flooding]]
- [[Switch]]
- [[Forwarding]]

## Mechanism

Switch 完成 source-MAC learning 後，在同一 VLAN context 中查詢 destination MAC，再執行下列決策。

### Known Unicast Decision

- Destination entry 位於另一個 forwarding port：只從該 port 送出。
- Destination entry 指向 ingress port：filter，不把 frame 送回同一 segment。
- Destination port 不在 forwarding state：不應繞過該狀態強行送出。

### Unknown Unicast Decision

若 destination 是單一 host MAC，但 [[MAC Address Table]] 沒有 entry，switch 使用 [[Frame Flooding]]，將 frame 複製到同 VLAN 的其他適用 forwarding ports。目標回覆後，switch 從 reply 的 source MAC 學到位置，後續 traffic 可轉為 known-unicast forwarding。

### Broadcast Decision

Destination MAC 為 `ffff.ffff.ffff` 時，switch 在同一 broadcast domain 內 flooding。Broadcast 不會因為 MAC table 已學滿而變成單點 forwarding。

## Decision Table

| Destination 狀態 | Switch action | 範圍 |
|---|---|---|
| Known unicast，entry 在其他 port | Forward | 一個 egress port |
| Known unicast，entry 在 ingress port | Filter | 不送出 |
| Unknown unicast | Flood | 同 VLAN 的其他適用 ports |
| Broadcast | Flood | 同 broadcast domain 的其他適用 ports |

## Depends On

- [[MAC Address Learning]] 提供動態可達資訊。
- [[MAC Address Table]] 提供 destination lookup。
- [[VLAN]] 與 port state 限制 forwarding/flooding boundary。

## Common Confusions

- Unknown unicast 仍是 unicast；「unknown」描述 switch table state，不是 frame address type。
- Flooding 不等於 broadcasting；broadcast 是 frame type，flooding 是 switch action。
- Switch 轉送 frame 不等於 router forwarding packet；switch 不執行 Layer 3 route selection。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-105_883_1399_179_207.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.5 — PC3 replies to PC1, and switches forward the frame using learned MAC table entries.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Knowledge Maps

- [[04_Maps/Ethernet Switching 與 ARP 地圖|Ethernet Switching 與 ARP 地圖]]

## Unit04 Additions

- Chapter 10 shows [[Frame Forwarding]] as the switch behavior that carries ARP requests/replies and data frames inside each LAN segment.
- Frame forwarding supports [[Packet Life Cycle]], but does not itself perform Layer 3 [[Route Selection]].

## Additional Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
