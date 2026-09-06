# Bridge Protocol Data Unit

tags: #concept #acting-ccna #stp #rstp #control-plane

> [!abstract] Concept role
> BPDU 是 STP／RSTP switches 對 root、path quality 與 port role 達成一致的 control-plane message。

## Aliases

- BPDU
- STP BPDU

## Definition

Bridge Protocol Data Unit 是 [[Spanning Tree Protocol]] 與 [[Rapid Spanning Tree Protocol]] 交換 topology information 的訊息。

## Why It Exists

每台 switch 只能直接觀察自己的 links。要建立全域一致的 loop-free topology，switches 必須分享它們認為的 root bridge、root path cost、sender bridge 與 sender port 等資訊。

## Prerequisites

- [[Layer 2 Loop]]
- [[Spanning Tree Protocol]]
- [[MAC Address]]

## Important Information

- Root Bridge ID：sender 認為的 root。
- Root Path Cost：sender 到 root 的累積 cost。
- Sender Bridge ID：傳送 BPDU 的 switch。
- Sender Port ID：傳送 BPDU 的 port。
- Timers：協助 switches 判斷 topology information 是否仍有效。

## Mechanism

STP 將較優的資訊稱為 superior BPDU。Topology selection 依序比較 root BID、root cost、sender BID 與 port ID；較低數值通常較優。

- Classic STP：root bridge 產生 BPDU，其他 switches 經 designated ports 轉送更新後的資訊。
- RSTP：所有 switches 每 2 秒由 designated ports 主動送 BPDU，不必先收到 root 的 BPDU。

## Depends On

- Bridge ID 必須能唯一且一致地排序 switches。
- Port cost 必須反映 path preference。
- BPDU reception 必須正常；單向故障可能破壞「沒有 BPDU便可 forwarding」的假設。

## Leads To

- [[Spanning Tree Protocol#Root Bridge Election|Root bridge election]]
- [[Spanning Tree Protocol#Port Role Selection|Root／designated port selection]]
- [[Rapid Spanning Tree Protocol#Rapid Convergence|RSTP synchronization]]
- [[STP Protection Features]]

## Failure Model

- 收到意外 superior BPDU：可能改變 root bridge 與整體 forwarding topology。
- 應持續收到 BPDU 卻停止：blocked port 可能錯誤轉為 forwarding，形成 loop。
- Edge port 收到 BPDU：通常表示連接了 switch 或錯誤橋接設備。

## Verification

- `show spanning-tree`：查看 Root ID、Bridge ID、cost、roles、states 與 port type。
- 發現 root 與設計不符時，沿 root port 方向追蹤 superior BPDU 來源。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-270_759_1326_1233_225.jpg)
*Source: [[00_Source/Chapter 14 - STP|Chapter 14 - STP]], Figure 14.5 — Newly booted switches initially advertise themselves as root in BPDUs.*

## Appears In

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06：DTP、VTP、STP 與 RSTP]]
- [[00_Source/Chapter 14.3 - The STP algorithm]]
- [[00_Source/Chapter 15.2 - STP and RSTP comparison]]

## Knowledge Maps

- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map]]
