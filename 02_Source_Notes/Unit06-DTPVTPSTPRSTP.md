# Unit06：DTP、VTP、STP 與 RSTP

tags: #source-note #unit #acting-ccna #vlan #stp #rstp

## Sources

- [[00_Source/Chapter 13 - DTP 與 VTP|Chapter 13 - DTP 與 VTP]]
- [[00_Source/Chapter 14 - STP|Chapter 14 - STP]]
- [[00_Source/Chapter 15 - RSTP|Chapter 15 - RSTP]]

## Core Question

在具有 VLAN、trunks 與 redundant Layer 2 paths 的 switched LAN 中，如何控制自動化範圍、建立無迴路 logical topology，並在 topology 改變時快速且安全地收斂？

## Key Ideas

- [[13-Dynamic Trunking Protocol|DTP]] 協商 port operational mode；[[13-VLAN Trunking Protocol|VTP]] 同步 VLAN database。兩者提供便利，但也可能放大錯誤或信任邊界。
- [[Layer 2 Loop]] 源於 redundant paths 與 BUM flooding；Ethernet frame 沒有 TTL，因此 loop 不會自行終止。
- [[Spanning Tree Protocol]] 保留 physical redundancy，卻阻擋部分 logical paths，使任兩個節點間只有一條 active path。
- [[Bridge Protocol Data Unit]] 攜帶 root、cost、bridge 與 port 資訊，讓 switches 對同一棵 spanning tree 達成一致。
- [[Rapid Spanning Tree Protocol]] 沿用相同 topology algorithm，但利用 synchronization、明確的 alternate／backup roles 與 link types 加速 convergence。
- [[STP Protection Features]] 不是互相替代；每一項防止不同 failure mode。

## Core Concepts

- [[VLAN]]
- [[Trunk Port]]
- [[13-Dynamic Trunking Protocol|Dynamic Trunking Protocol]]
- [[13-VLAN Trunking Protocol|VLAN Trunking Protocol]]
- [[Layer 2 Loop]]
- [[Spanning Tree Protocol]]
- [[Bridge Protocol Data Unit]]
- [[Rapid Spanning Tree Protocol]]
- [[STP Protection Features]]

## Important Definitions

- **Root bridge**：STP topology 的共同參考點，由最低 Bridge ID 決定。
- **Root port**：每台 non-root switch 到 root bridge 的最佳 active path。
- **Designated port**：每個 segment 上負責 forwarding 的單一最佳 port。
- **Non-designated／alternate／backup port**：為避免 loop 而不轉送一般 traffic 的 redundant path。
- **Convergence**：所有 switches 對 topology change 達成一致並恢復正確 forwarding 的過程。

## Why These Concepts Exist

```text
需要可用性 → 建立 redundant Layer 2 links
           → BUM frames 可能無限循環
           → STP 建立 loop-free logical topology
           → Classic STP convergence 太慢
           → RSTP 以協商／同步縮短中斷
           → Protection features 限制錯誤接線與 BPDU 異常
```

## Knowledge Progression

```text
[[VLAN]] + [[Trunk Port]]
  ├── automation → DTP / VTP
  └── redundant Layer 2 paths → [[Layer 2 Loop]]
                                  ↓ prevented by
                             [[Spanning Tree Protocol]]
                                  ↓ exchanges
                             [[Bridge Protocol Data Unit]]
                                  ↓ faster convergence
                             [[Rapid Spanning Tree Protocol]]
                                  ↓ hardened by
                             [[STP Protection Features]]
```

## Mechanisms

### DTP／VTP control scope

- DTP 決定 port 最後是 access 或 trunk；它不建立 VLAN，也不轉送 user frames。
- VTP 同步 VLAN database；它不決定 STP port role，也不能保證 VLAN data path 正常。
- 明確設定 port mode、停用不需要的 negotiation，能縮小意外變更範圍。

### STP algorithm

1. 以最低 BID 選出一台 root bridge。
2. 每台 non-root switch 選一個 root port：最低 root cost → 最低 neighbor BID → 最低 neighbor port ID → 必要時最低 local port ID。
3. 每個 segment 選一個 designated port：所在 switch 最低 root cost → 最低 BID → 必要時最低 local port ID。
4. 其餘 redundant ports 不轉送一般 frames，形成 loop-free topology。

### Classic STP convergence

- Blocking、Listening、Learning、Forwarding 是主要 states。
- Hello 2 秒、Forward Delay 15 秒、Max Age 20 秒為章中預設值。
- 間接故障可能經歷 20 + 15 + 15，最長約 50 秒才恢復 forwarding。

### RSTP convergence

- Blocking／Listening 合併為 Discarding；保留 Learning／Forwarding。
- Alternate port 提供替代 root path；Backup port 備援同一 switch 到同一 shared segment 的 designated path。
- Point-to-point full-duplex link 可用 synchronization；shared link 仍依 timer；edge link 使用 PortFast。
- 未收到三個連續 BPDUs 時，預設約 6 秒開始反應；實體 port down 可更快。

## Cause and Effect

- Redundancy + BUM flooding + no Layer 2 TTL → frame multiplication → broadcast storm、MAC address flapping、LAN outage。
- Lower BID → superior BPDU → root bridge election result。
- Lower accumulated port cost → preferred path to root bridge。
- PortFast bypasses listening／learning → immediate endpoint access；誤用於 switch link → loop risk。
- Superior BPDU on Root Guard port → root-inconsistent；missing expected BPDU on Loop Guard port → loop-inconsistent。
- Interface-level BPDU Filter ignores BPDUs → STP effectively disabled → serious loop risk。

## Prerequisites

- [[Ethernet Switching]]
- [[MAC Address Learning]]
- [[Frame Flooding]]
- [[Broadcast Domain]]
- [[VLAN]] 與 [[Trunk Port]]

## Depends On

- STP decisions depend on valid [[Bridge Protocol Data Unit|BPDUs]] and consistent cost／ID comparisons.
- RSTP rapid transition depends on compatible neighbors and appropriate link type.
- Per-VLAN variants depend on VLAN identity and consume resources per instance.

## Leads To

- EtherChannel：多條 physical links 形成一條 logical link，避免 STP 封鎖平行 links 的個別成員。
- Layer 2 campus design：root placement、failure domains、redundant distribution paths。
- Switching security：edge-port trust、BPDU protection、change control。

## Cross-Document Relationships

- Chapter 13 建立 VLAN/trunk control scope；DTP 造成的 unintended trunk 可能讓原本不受信任的設備參與更多 VLAN 與 STP instances。
- Chapter 14 建立 classic STP algorithm；Chapter 15 明確重用同一 algorithm，只替換 convergence mechanics、states 與 redundant port roles。
- Chapter 14 的「root bridge ports 都 designated」被 Chapter 15 的 shared-segment example 精確化為「root bridge 是各相連 segment 的 designated bridge」。
- Chapter 14 的 PortFast／BPDU Guard 建立 edge protection；Chapter 15 再從 root placement、BPDU continuity 與 filtering 擴充 protection matrix。

## Cross-Unit Relationships

- Unit03 的 [[Frame Flooding]] 與 [[MAC Address Learning]] 分別解釋 loop 為何擴散，以及 MAC flapping 為何發生。
- Unit05 的 [[VLAN]]／[[Trunk Port]] 是 PVST+、Rapid PVST+ 與 Chapter 13 automation 的 data-plane prerequisites。
- Unit03 的 [[Time To Live]] 提供 Layer 3 contrast：IP packets 有 lifetime limit，Ethernet frames 沒有。
- 後續 EtherChannel 應銜接「parallel links 被 STP individually blocked」的容量問題，目前列入 REVIEW。

## Relationships

- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map|Unit06：DTP、VTP、STP、RSTP Knowledge Map]]
- [[04_Maps/Unit06-DTPVTP Operational Decision Map|DTP／VTP Operational Decision Map]]
- [[04_Maps/VLAN Knowledge Map|VLAN Knowledge Map]]

## Contrast

| 對比 | 差異 |
|---|---|
| DTP vs VTP | 協商 port mode vs 同步 VLAN database |
| Physical vs logical topology | 實際 links vs STP 允許 forwarding 的 paths |
| STP vs RSTP | timer-oriented vs synchronization-oriented convergence |
| Port role vs port state | topology 中的責任 vs 當下 forwarding／learning 行為 |
| Root Guard vs Loop Guard | 收到 superior BPDU vs 預期 BPDU 消失 |
| BPDU Guard vs BPDU Filter | 收到 BPDU 即隔離 vs 抑制／忽略 BPDU，後者風險更高 |

## Important Commands

| 目的 | 命令 |
|---|---|
| 查看 topology | `show spanning-tree` |
| 設定 root priority | `spanning-tree vlan <vlan-id> priority <value>` |
| 切換 Cisco mode | `spanning-tree mode {pvst | rapid-pvst}` |
| 查看 path-cost method | `show spanning-tree pathcost method` |
| PortFast／BPDU Guard | `spanning-tree portfast` / `spanning-tree bpduguard enable` |
| Root／Loop Guard | `spanning-tree guard root` / `spanning-tree guard loop` |

## Common Confusions

- STP 不移除 physical redundancy；它只阻擋 logical forwarding path。
- Root port 是每台 non-root switch 一個；designated port 是每個 segment 一個。
- 「root bridge 上所有 ports 都 designated」在 shared segment 有例外；更精確是 root bridge 為每個相連 segment 的 designated bridge。
- RSTP 與 STP 的 topology algorithm 相同，主要差別是 states、roles、BPDU handling 與 convergence。
- `show spanning-tree` 的 `ieee`／`rstp`，對應設定的 `pvst`／`rapid-pvst`。

## Questions

- [[05_Questions/Unit06-DTPVTPSTPRSTP Questions|Unit06 Reasoning Questions]]

## REVIEW

- [[06_review/Unit06-DTPVTPSTPRSTP REVIEW|Unit06 REVIEW]]
