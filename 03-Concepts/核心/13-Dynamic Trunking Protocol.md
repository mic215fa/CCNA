# Dynamic Trunking Protocol

tags: #concept #acting-ccna #dtp #vlan #trunk #security

> [!info] Concept role
> [[VLAN]] 的控制機制子概念：協商 switch port 的 access/trunk operational mode；不建立 VLAN，也不負責 frame forwarding。

## Aliases

- DTP

## Definition

Dynamic Trunking Protocol（DTP）是 Cisco proprietary protocol，讓相鄰 Cisco switches 交換 port administrative mode，並協商 port 最終採用 access 或 trunk operational mode。

## Why It Exists

DTP 的設計目標是減少部署 switches 時手動設定 port modes 的工作量。

## Prerequisites

- [[Access Port]]
- [[Trunk Port]]
- [[VLAN]]

## Mechanism

- `dynamic auto`：參與 DTP negotiation，但不主動形成 trunk。
- `dynamic desirable`：參與 negotiation，並主動嘗試形成 trunk。
- 手動設定 `switchport mode trunk` 的 port 仍可能傳送 DTP messages。
- Router 與非 Cisco switch 不使用 DTP；需要 trunk 時必須手動設定。

## Practical Evaluation

### 為何通常不實用

- 現代 network 通常希望 port role 明確且可預測；DTP 自動協商帶來的便利有限。
- DTP 僅適用於 Cisco switches，無法作為 multi-vendor trunk configuration 方法。
- DTP messages 是額外且通常不必要的 control traffic。
- 惡意裝置可能偽造 DTP messages，嘗試把原本的 access link 協商成 trunk，進而接觸多個 VLANs。

### 建議做法

- Access port：明確設定 `switchport mode access`。
- Trunk port：明確設定 `switchport mode trunk`。
- 使用 `switchport nonegotiate` 停止傳送 DTP messages。

### 為何仍需理解

DTP 在部分 Cisco switch ports 上可能預設啟用。即使實務設計不依賴它，仍需理解 administrative mode 與 operational mode 的差異，才能驗證 port 狀態並正確停用 negotiation。

## Relationships

- DTP → negotiates → [[Trunk Port]] operational mode
- Explicit port-mode configuration → reduces need for → DTP
- DTP negotiation → may create → unintended trunk access
- `switchport nonegotiate` → disables → DTP message transmission
- Unintended trunk → may expand → [[Spanning Tree Protocol]] and VLAN trust boundaries

## Appears In

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06：DTP、VTP、STP 與 RSTP]]
- [[00_Source/Chapter 13 - DTP 與 VTP]]
- [[00_Source/Chapter 13.1 - Dynamic Trunking Protocol]]

## Knowledge Maps

- [[04_Maps/VLAN Knowledge Map|VLAN Knowledge Map]]
- [[04_Maps/Unit06-DTPVTP Operational Decision Map|DTP/VTP Operational Decision Map]]
- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map|Unit06 Knowledge Map]]

## Questions

- 為什麼手動設定 trunk mode 後，仍可能需要 `switchport nonegotiate`？
- DTP 的自動化便利為什麼不足以抵銷 unintended trunk 的風險？
- `dynamic auto` 與 `dynamic desirable` 如何影響最後的 operational mode？
