# Spanning Tree Protocol

tags: #concept #acting-ccna #stp #ethernet #switching

> [!abstract] Concept role
> STP 將有 redundancy 的 physical topology 計算成 loop-free logical topology，是 switched LAN 可用性與安全性的核心控制機制。

## Aliases

- STP
- IEEE 802.1D Spanning Tree Protocol
- PVST+（Cisco per-VLAN implementation）

## Definition

Spanning Tree Protocol 透過阻擋 redundant Layer 2 paths，使 LAN 中任兩個 nodes 之間只保留一條 active logical path，防止 [[Layer 2 Loop]]。

## Why It Exists

沒有 redundancy，單一 link failure 會中斷服務；有 redundancy 但沒有 loop prevention，BUM flooding 可能在數秒內癱瘓 LAN。STP 在兩者間取得平衡：保留備援 link，但控制哪些 ports 能 forwarding。

## Prerequisites

- [[Ethernet Switching]]
- [[Frame Flooding]]
- [[MAC Address Learning]]
- [[Layer 2 Loop]]
- [[VLAN]] 與 [[Trunk Port]]

## Depends On

- [[Bridge Protocol Data Unit]] exchange
- Stable Bridge ID、port cost 與 port ID comparison
- 所有可能形成 loop 的 Layer 2 paths 都參與一致的 spanning-tree control

## Topology Model

```text
Physical topology: redundant links, possibly cycles
             ↓ STP algorithm
Logical topology: one active path between any two nodes
             + blocked backup paths available after failure
```

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-268_609_1305_1081_221.jpg)
*Source: [[00_Source/Chapter 14 - STP|Chapter 14 - STP]], Figure 14.3 — STP converts a meshed physical topology into a loop-free logical topology.*

## Bridge ID

Bridge ID（BID）是 64-bit value：

```text
16-bit bridge priority + 48-bit switch MAC address
```

在 Cisco PVST+，bridge priority 包含 configurable priority 與 Extended System ID（VLAN ID）。預設 configurable priority 為 32768，且只能以 4096 為增量設定。比較 BID 時先比 priority，平手再比 MAC address；數值較低者較優。

## Root Bridge Election

1. Switch boot 時先宣告自己為 root。
2. Switches 交換 [[Bridge Protocol Data Unit|BPDUs]]。
3. 全 LAN／STP instance 中最低 BID 的 switch 成為 root bridge。
4. Root bridge 是後續所有 path comparisons 的共同參考點。

Root placement 應是設計決策，而不應任由最低 MAC address 偶然決定。可使用：

- `spanning-tree vlan <vlan-id> priority <value>`
- `spanning-tree vlan <vlan-id> root {primary | secondary}`

Source 特別指出直接設定明確 priority 更可預測；`root primary/secondary` 在已有更低 priorities 時不保證達成意圖。

## Port Role Selection

### Root port

每台 non-root switch 恰有一個 root port，依序比較：

1. Lowest root cost
2. Lowest neighbor BID
3. Lowest neighbor port ID
4. 多個 local ports 位於同一 segment 時，lowest local port ID

Root cost 是沿 path 的 port costs 總和，不是只看候選 port 自己的 cost。

### Designated port

每個 segment 恰有一個 designated port，依序比較：

1. Port 所在 switch 的 lowest root cost
2. Port 所在 switch 的 lowest BID
3. 必要時 lowest local port ID

Root port 與 designated port forwarding；classic STP 其餘 ports 為 non-designated 並 blocking。

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-277_776_1414_1153_192.jpg)
*Source: [[00_Source/Chapter 14 - STP|Chapter 14 - STP]], Figure 14.9 — Root-port selection using root cost, neighbor BID, and neighbor port ID.*

## Port States and Timers

| State | Learn MAC | Forward data | 作用 |
|---|---:|---:|---|
| Blocking | No | No | 阻擋 redundant path，仍處理 STP messages |
| Listening | No | No | 決定 role，避免過早 forwarding |
| Learning | Yes | No | 建立 MAC address table |
| Forwarding | Yes | Yes | 正常收送 data frames |
| Disabled | No | No | Port 非 operational；不是正常 topology role |

預設 timers：Hello 2 秒、Forward Delay 15 秒、Max Age 20 秒。間接失去 BPDU 時，blocking port 最長可能經過 20 + 15 + 15 = 50 秒才 forwarding；若實體 port 直接 down，仍可能有 30 秒 listening／learning delay。

## PVST+

Cisco PVST+ 為每個 VLAN 執行獨立 STP instance，使不同 VLAN 可選不同 root 與 active links。優點是可做 per-VLAN load distribution；限制是 VLAN 數量增加時，instances、BPDUs、CPU／memory 與管理複雜度也增加。

## Design Principles

- 明確規劃 primary／secondary root bridge。
- 不要用「目前沒有 loop」推論 topology 安全；blocked path、BPDU flow 與 protection 都要驗證。
- Endpoint ports 使用 PortFast 時，搭配 BPDU Guard 保護 edge 假設。
- Classic STP 的 blocked link 是 standby capacity，不是故障 link。

## Failure Model and Troubleshooting

| 症狀 | 應檢查 |
|---|---|
| Root bridge 不是預期設備 | Root ID、Bridge ID、priority 與 superior BPDU 來源 |
| Path 與預期不同 | Root cost、neighbor BID、neighbor／local port ID tiebreakers |
| Link up 後長時間不能傳資料 | Port state 與 forward-delay timer；是否適合 PortFast |
| Broadcast storm／MAC flapping | 是否有 loop、STP 是否被 filter／停用、recent cabling change |
| Redundant failure 後恢復很慢 | Classic STP timers、failure 是否只停止 BPDU 而非 port down |

## Important Commands

- `show spanning-tree`
- `spanning-tree vlan <vlan-id> priority <value>`
- `spanning-tree vlan <vlan-id> port-priority <value>`

## Leads To

- [[Rapid Spanning Tree Protocol]]
- [[STP Protection Features]]
- EtherChannel 與 hierarchical campus design

## Contrast With

- [[Rapid Spanning Tree Protocol]]：相同 topology-selection algorithm，但 convergence mechanism、states 與 redundant roles 更明確。
- Routing loop prevention：STP 預防 Layer 2 cycles；TTL 只限制 Layer 3 packet lifetime。

## Appears In

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06：DTP、VTP、STP 與 RSTP]]
- [[00_Source/Chapter 14 - STP]]

## Knowledge Maps

- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map]]

## Questions

- 為什麼 root bridge placement 會影響 traffic path，即使 STP 的首要目的只是防 loop？
- Root port 與 designated port 的選擇單位為什麼不同？
- 哪類故障會使 classic STP 接近 50 秒才恢復？
