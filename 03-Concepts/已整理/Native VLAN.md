# Native VLAN

tags: #concept #acting-ccna #vlan #trunk

> [!info] Concept role
> [[Trunk Port]] 的子概念：定義 802.1Q trunk 上 untagged frames 的 VLAN 歸屬。

## Definition

Native VLAN 是 802.1Q trunk 上通常以 untagged frame 傳送的 VLAN。

## Why It Exists

802.1Q 支援在 trunk link 上處理 untagged traffic；native VLAN 提供這些 untagged frames 的 VLAN 歸屬。

## Prerequisites

- [[Trunk Port]]
- [[IEEE 802.1Q Tag]]
- [[VLAN]]

## Related Concepts

- [[Native VLAN Mismatch]]
- [[Allowed VLAN List]]

## Mechanism

當 frame 屬於 native VLAN 時，送出 trunk port 可不加 802.1Q tag；接收端會把 untagged frames 歸入該 port 設定的 native VLAN。

## Failure Model

若 trunk 兩端設定不同 native VLAN，同一個 untagged frame 會被兩端歸入不同 VLAN，形成 [[Native VLAN Mismatch]]。這不是單純的標籤顯示問題，而是兩端對 Layer 2 boundary 的解讀不一致。

## Operational Guidance

- Trunk 兩端必須使用相同 native VLAN。
- Native VLAN 仍應受 [[Allowed VLAN List]] 與整體 VLAN design 管理。
- 排錯時同時檢查 `show interfaces trunk` 的 native VLAN 與 trunk operational state。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-231_763_1420_1210_190.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.7 — Native VLAN traffic is sent untagged over a trunk link, while non-native VLAN traffic is tagged.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
