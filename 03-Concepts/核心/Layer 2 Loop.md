# Layer 2 Loop

tags: #concept #acting-ccna #ethernet #switching #stp

> [!abstract] Concept role
> Layer 2 Loop 是 redundant switched LAN 的核心 failure model，也是 [[Spanning Tree Protocol]] 存在的直接原因。

## Aliases

- Switching loop
- Bridging loop

## Definition

Layer 2 Loop 是 Ethernet frames 因 switched topology 存在多條 active paths，而被 switches 持續重複轉送的狀態。

## Why It Exists

Redundant links 消除 single point of failure，但 [[Frame Flooding|flooded frames]] 會沿所有可用方向傳播。Ethernet header 沒有像 [[Time To Live|IPv4 TTL]] 的 hop limit，因此 looping frame 不會自行過期。

## Prerequisites

- [[Ethernet Switching]]
- [[Frame Flooding]]
- [[Broadcast Frame]]
- [[MAC Address Learning]]

## Mechanism

```text
Redundant active Layer 2 paths
  + broadcast / unknown-unicast / multicast flooding
  + no frame TTL
  ↓
Frames circulate and multiply
  ├── broadcast storm
  ├── MAC address flapping
  └── CPU / bandwidth / host exhaustion
```

## Failure Model

- **Broadcast storm**：looping frames 快速消耗 link bandwidth 與 device CPU。
- **MAC address flapping**：同一 source MAC 從不同 ports 反覆被學到，MAC table entry 持續移動。
- **Duplicate delivery**：end hosts 反覆收到相同 frames。
- **LAN-wide outage**：BUM traffic 使問題從單一 link 擴散至整個 Layer 2 domain。

## Solution

[[Spanning Tree Protocol]] 將部分 redundant ports 置於不轉送一般 traffic 的狀態，保留 physical backup path，同時讓 logical topology 無迴路。

## Contrast With

- Layer 3 routing loop：IP packet 的 TTL 最終會降至 0；Layer 2 frame 沒有等價欄位。
- Redundancy：redundancy 是設計目的；loop 是多條 paths 同時 active 且 flooding 未受控制的失敗結果。

## Verification and Troubleshooting

- 查看 `show spanning-tree`，確認 redundant topology 中確實存在 blocking／discarding ports。
- 查看 MAC address table 是否在多個 ports 間快速變動。
- 尋找 broadcast traffic、介面利用率與 CPU 的異常上升。
- 檢查近期新增 cable、switch、trunk 或被停用的 STP protection。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-266_559_1233_1170_225.jpg)
*Source: [[00_Source/Chapter 14 - STP|Chapter 14 - STP]], Figure 14.1 — A flooded broadcast frame loops in both directions through redundant switch links.*

## Appears In

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06：DTP、VTP、STP 與 RSTP]]
- [[00_Source/Chapter 14.1 - The need for STP]]

## Knowledge Maps

- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map]]

## Questions

- 為什麼 redundancy 本身不是錯誤，卻會使 Layer 2 loop 成為必須處理的風險？
- MAC address flapping 如何從 control symptom 反映實體 topology 中的 loop？
