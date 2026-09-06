# VLAN Trunking Protocol

tags: #concept #acting-ccna #vtp #vlan #automation #security

> [!info] Concept role
> [[VLAN]] 的控制與自動化子概念：同步 VLAN database；不決定 individual frame 的 forwarding path。

## Aliases

- VTP

## Definition

VLAN Trunking Protocol（VTP）是 Cisco proprietary protocol，用來讓同一 VTP domain 內的 switches 傳播 VLAN changes，並同步 VLAN database。

## Why It Exists

在包含大量 Cisco switches 的 LAN 中，逐台建立或修改 VLAN 容易耗時並產生設定不一致。VTP 透過集中傳播 VLAN changes，降低重複操作與 human error。

## Prerequisites

- [[VLAN]]
- [[Trunk Port]]
- [[VLAN ID]]

## Depends On

- 相同 VTP domain name
- Trunk connectivity；VTP messages 不會從 access ports 傳送
- VLAN database revision-number comparison

## Mechanism

- VLAN database 儲存在 switch flash 中的 `vlan.dat`。
- VLAN database 每次變更時，revision number 會增加。
- VTP versions 1/2 的 switches 會接受同 domain 中較高 revision number 的 database。
- Server 與 client modes 參與 synchronization；transparent 與 off modes 不同步自己的 VLAN database。

## Practical Evaluation

### 為何常被認為不實用或應避免

- VTP 是 Cisco proprietary protocol，不適合 multi-vendor VLAN management。
- 小型 LAN 的 switch 數量有限，手動建立 VLAN 的成本不高，VTP 的收益可能很小。
- 在 VTP versions 1/2 中，新接入且具有較高 revision number 的 switch 可能讓整個 domain 同步到錯誤 VLAN database，造成 VLANs 消失與 network outage。
- Revision reset 依賴人員遵循正確流程；Chapter 13 指出實務上的疏忽使 VTP 累積了不良評價。
- 若 network 不使用 VTP，應設定 `vtp mode off`，避免不必要的 VTP participation。

### VTPv3 的重要例外

VTP 不應一概視為完全無用。VTP version 3 透過 primary server 限制只有一台 switch 能建立、修改與刪除 VLANs；其他 switches 只與 primary server 同步，因此能降低新接入高 revision-number switch 覆寫 domain database 的核心風險。VTPv3 也支援 extended-range VLANs。

在大型、以 Cisco switches 為主、確實需要集中 VLAN automation，而且具備變更管理程序的環境中，Chapter 13 認為 VTPv3 仍可成為有用工具。

## Operational Decision

```text
不需要集中 VLAN synchronization
  ↓
使用 vtp mode off

需要集中管理，但使用 VTP v1/v2
  ↓
高 revision-number overwrite 風險
  ↓
通常應避免或嚴格執行 revision reset 程序

需要 Cisco-only VLAN automation 且支援 VTPv3
  ↓
使用 primary server 控制 VLAN changes
```

## Relationships

- VTP → synchronizes → VLAN database
- Higher revision number → is treated as → newer VLAN information
- VTP versions 1/2 + newly connected high-revision switch → may cause → VLAN database overwrite
- VTP version 3 primary server → reduces → unintended database overwrite risk
- `vtp mode off` → prevents → VTP participation and message forwarding
- VLAN database changes → add or remove → per-VLAN [[Spanning Tree Protocol]] instances and data-plane boundaries

## Contrast With

- Manual per-switch VLAN configuration：操作較多，但變更範圍較局部且明確。
- VTP transparent mode：保留本機 VLAN management 並轉送同 domain 的 VTP messages，但不參與 database synchronization。

## Appears In

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06：DTP、VTP、STP 與 RSTP]]
- [[00_Source/Chapter 13 - DTP 與 VTP]]
- [[00_Source/Chapter 13.2 - VLAN Trunking Protocol]]

## Knowledge Maps

- [[04_Maps/VLAN Knowledge Map|VLAN Knowledge Map]]
- [[04_Maps/Unit06-DTPVTP Operational Decision Map|DTP/VTP Operational Decision Map]]
- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map|Unit06 Knowledge Map]]

## Questions

- 為什麼較高 revision number 不一定代表正確的 VLAN database？
- VTPv3 primary server 如何改變 VTPv1/v2 的風險模型？
- 在什麼 network 規模與 vendor 組合下，VTP automation 的收益可能大於風險？
- Transparent mode 與 off mode 對 VTP message forwarding 有何不同？
