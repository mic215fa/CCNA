# STP Protection Features

tags: #concept #acting-ccna #stp #rstp #security #troubleshooting

> [!info] Concept role
> STP Protection Features 是 [[Spanning Tree Protocol]]／[[Rapid Spanning Tree Protocol]] 的 operational safeguards；它們分別保護 edge、root placement 與 BPDU continuity 等假設。

## Aliases

- STP toolkit
- Spanning-tree protection features

## Why They Exist

STP algorithm 能在資訊正確且 topology 符合假設時防止 loop，但錯誤接線、外部 switch、superior BPDU、單向 control-plane failure 或 BPDU filtering 都可能破壞這些假設。

## Prerequisites

- [[Spanning Tree Protocol]]
- [[Bridge Protocol Data Unit]]
- [[Layer 2 Loop]]

## Protection Matrix

| Feature | 適用位置／假設 | Trigger | Result | Recovery |
|---|---|---|---|---|
| PortFast | 只連 end host 的 edge port | 設定後 port up | 略過 listening／learning，立即 forwarding | 無隔離動作 |
| BPDU Guard | PortFast／edge port 不應收到 BPDU | 收到任何 BPDU | err-disabled | 排除錯誤設備後 shutdown / no shutdown |
| Root Guard | 該 boundary 不應出現新 root | 收到 superior BPDU | root-inconsistent | superior BPDU 消失後自動恢復 |
| Loop Guard | Blocking／discarding path 應持續收到 BPDU | 預期 BPDU 消失 | loop-inconsistent | BPDU 恢復後自動恢復 |
| BPDU Filter | 特殊需求下抑制 BPDU | 依 interface/global 模式不同 | 可能有效停用該 port 的 STP | 依設定方式；高風險 |

## PortFast and BPDU Guard

PortFast 解決 endpoint 必須等待 30 秒的問題，但它以「這裡不會接 switch」為前提。BPDU Guard 把這個設計假設變成可執行 policy：若 edge port 收到 BPDU，立即隔離 port，避免新 switch 影響 topology。

```text
Expected endpoint port
  ↓ PortFast
Immediate forwarding
  + BPDU Guard
Unexpected BPDU → err-disabled instead of topology participation
```

最佳搭配是只在 end-host ports 使用 PortFast，並在所有 PortFast ports 啟用 BPDU Guard。

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-289_514_1410_411_192.jpg)
*Source: [[00_Source/Chapter 14 - STP|Chapter 14 - STP]], Figure 14.18 — PortFast bypasses listening and learning for an endpoint port.*

## Root Guard

Root Guard 保護 root placement。若 boundary port 收到 superior BPDU，它進入 root-inconsistent，不接受該 external switch 改變 root。適合 provider/customer 或管理邊界，而不是單純所有 access ports。

## Loop Guard

Loop Guard 保護 BPDU continuity。若本來因收到 superior BPDU 而 discarding 的 port 突然收不到 BPDU，它不會自行變成 designated/forwarding，而進入 loop-inconsistent。這能防止單向故障或 neighbor control-plane failure 造成兩端都 forwarding。

Root Guard 與 Loop Guard 在同一 port 互斥：前者對「收到更優 BPDU」反應，後者對「應收到卻沒收到 BPDU」反應。

## BPDU Filter

兩種設定行為不同：

- Interface `spanning-tree bpdufilter enable`：不送 BPDU，也忽略收到的 BPDU；等同在該 port 有效停用 STP，若接 switch 可能形成 loop。
- Global `spanning-tree portfast bpdufilter default`：PortFast ports 不送 BPDU；若收到 BPDU，PortFast 與 filtering 會被停用，port 回到正常 STP/RSTP operation。

因 interface-level BPDU Filter 能隱藏正是 STP 用來防 loop 的 evidence，source 建議一般避免使用。

## Common Confusions

- PortFast 是 convergence optimization，不是 security feature。
- BPDU Guard 不選 root，也不保護 blocked link 的 BPDU loss。
- Root Guard 與 BPDU Guard 都對 BPDU 反應，但 Root Guard 只拒絕 superior BPDU 並可自動恢復；BPDU Guard 收到任何 BPDU 就 err-disable。
- BPDU Filter 不是 BPDU Guard 的溫和版本；忽略 BPDU 反而可能讓 loop 無法被 STP 察覺。

## Verification and Troubleshooting

- `show spanning-tree`：檢查 root-inconsistent、loop-inconsistent、role/state/type。
- `show interfaces status err-disabled` 或相關介面狀態：確認 BPDU Guard 隔離。
- 先排除錯接 switch、unexpected superior root 或 BPDU loss，再恢復 port；不要只做 shutdown/no shutdown。

## Important Commands

- `spanning-tree portfast`
- `spanning-tree portfast default`
- `spanning-tree bpduguard enable`
- `spanning-tree portfast bpduguard default`
- `spanning-tree guard root`
- `spanning-tree guard loop`
- `spanning-tree bpdufilter enable`
- `spanning-tree portfast bpdufilter default`

## Appears In

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06：DTP、VTP、STP 與 RSTP]]
- [[00_Source/Chapter 14.5 - PortFast and BPDU Guard]]
- [[00_Source/Chapter 15.4 - Root Guard, Loop Guard, and BPDU Filter]]

## Knowledge Maps

- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map]]

## Questions

- Root Guard、Loop Guard 與 BPDU Guard 各自保護哪一項 topology 假設？
- 為什麼 interface-level BPDU Filter 可能比完全不設定 protection 更危險？
