# Rapid Spanning Tree Protocol

tags: #concept #acting-ccna #rstp #stp #switching

> [!abstract] Concept role
> RSTP 保留 STP 的 loop-free topology algorithm，改用主動 BPDU 與 synchronization 加速 topology change 後的安全 forwarding。

## Aliases

- RSTP
- IEEE 802.1w
- Rapid PVST+（Cisco per-VLAN implementation）

## Definition

Rapid Spanning Tree Protocol 是 [[Spanning Tree Protocol]] 的快速收斂演進。它仍選 root bridge、root port 與 designated port，但改良 BPDU handling、port states、redundant roles 與 link-type behavior。

## Why It Exists

Classic STP 為避免 loop 而依賴 timers，間接故障時可能中斷約 50 秒。RSTP 讓相鄰 switches 協商並同步 topology，能在安全條件成立時立即讓 port forwarding。

## Prerequisites

- [[Spanning Tree Protocol]] algorithm
- [[Bridge Protocol Data Unit]]
- Duplex 與 switched-link behavior

## What Stays the Same

- Lowest BID 選 root bridge。
- 每台 non-root switch 選一個 root port。
- 每個 segment 選一個 designated port。
- Cost／BID／port ID 的 decision order 不變。

## Port States

| Classic STP | RSTP | 行為 |
|---|---|---|
| Blocking + Listening | Discarding | 不 learn MAC、不 forward data |
| Learning | Learning | Learn MAC、不 forward data |
| Forwarding | Forwarding | Learn MAC 並 forward data |

如果 synchronization 成功，root／designated port 可從 discarding 直接進入 forwarding；若 neighbor 不支援 RSTP 或 link 不適用，仍可能經歷 timer-based discarding → learning → forwarding。

## Port Roles

- **Root**：到 root bridge 的最佳 active path。
- **Designated**：每個 segment 的 active forwarding port。
- **Alternate**：連到另一台 switch 的 designated port，提供替代 root path；正常為 discarding。
- **Backup**：同一 switch 的另一個 port 經 shared segment 連回本 switch designated port；現代 switched network 很少出現。

Alternate 與 Backup 取代 classic STP 籠統的 non-designated role，使 switch 知道 redundant port 能替代哪一種 path。

## Link Types

| Link type | 判斷／設定 | Rapid behavior |
|---|---|---|
| Point-to-point | Full duplex；可自動判斷 | 可使用 synchronization |
| Shared | Half duplex；典型為 hub segment | 不能使用 sync，依 timer transition |
| Edge | 手動啟用 PortFast 的 endpoint port | 立即 forwarding，不把 edge activity 視為 topology change |

Edge 是唯一不能只靠 duplex 自動辨識的類型；連到 host 並不會自動讓 port 成為 edge。

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-305_582_1119_183_316.jpg)
*Source: [[00_Source/Chapter 15 - RSTP|Chapter 15 - RSTP]], Figure 15.7 — RSTP point-to-point, shared, and edge link types.*

## Rapid Convergence

- RSTP switches 都會每 2 秒從 designated ports 主動送 BPDU。
- 未收到三個 BPDUs（預設約 6 秒）即可判定鄰接資訊失效；classic STP 通常等 Max Age 20 秒。
- Alternate path 能在 sync 成功後立即 forwarding。
- 若 failed port 直接 down，不必等待三個 missed BPDUs，恢復可低於 1 秒。

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-304_613_1418_183_221.jpg)
*Source: [[00_Source/Chapter 15 - RSTP|Chapter 15 - RSTP]], Figure 15.6 — RSTP shortens an indirect topology-change recovery from up to 50 seconds to just over 6 seconds.*

## Versions

| Version | Scope | Convergence model |
|---|---|---|
| STP (802.1D) | Single tree | Classic timers |
| PVST+ | One Cisco tree per VLAN | Classic timers |
| RSTP (802.1w) | Single tree | Rapid synchronization |
| Rapid PVST+ | One Cisco rapid tree per VLAN | Rapid synchronization |
| MSTP (802.1s) | Multiple VLANs grouped per instance | RSTP mechanics |

Per-VLAN instances allow traffic distribution，但 VLAN 很多時會增加 CPU、memory、BPDU volume 與管理複雜度；MSTP 以多 VLAN 共用 instance 改善 scale。

## Compatibility and Limitations

- RSTP 與 STP 相容，但接到 classic STP neighbor 的 port 無法享有 rapid sync，會像 classic STP 運作。
- Shared／half-duplex links 無法使用 rapid synchronization。
- Rapid convergence 不取代 [[STP Protection Features]]；錯誤 BPDU 或錯誤 edge 設定仍能破壞 topology。

## Verification and Troubleshooting

- `show spanning-tree`：`protocol rstp` 代表 Rapid PVST+；檢查 Role、Sts 與 Type。
- `show spanning-tree pathcost method`：確認 short／long cost method，不應從 RSTP mode 推定一定使用 long cost。
- 預期快速但仍等待 30 秒：檢查 neighbor protocol、duplex/link type，以及 endpoint port 是否啟用 PortFast。
- 預期 alternate takeover 卻失敗：確認 alternate role、BPDU reception 與 protection state。

## Important Commands

- `spanning-tree mode rapid-pvst`
- `show spanning-tree`
- `show spanning-tree pathcost method`
- `spanning-tree pathcost method {short | long}`
- `spanning-tree link-type {point-to-point | shared}`

## Related Concepts

- [[STP Protection Features]]
- [[VLAN]]
- [[Trunk Port]]

## Appears In

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06：DTP、VTP、STP 與 RSTP]]
- [[00_Source/Chapter 15 - RSTP]]

## Knowledge Maps

- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map]]

## Questions

- RSTP topology algorithm 沒變，為什麼仍能顯著縮短 convergence？
- Alternate 與 Backup port 分別備援哪一種 path？
- 為什麼 full-duplex 與 RSTP compatibility 都會影響 synchronization？
