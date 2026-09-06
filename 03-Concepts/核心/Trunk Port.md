# Trunk Port

tags: #concept #acting-ccna #vlan #trunk

> [!info] Concept role
> [[VLAN]] 的直接子概念：讓多個 VLAN 共用一條基礎設施連線。

## Definition

Trunk Port 是能在同一 physical link 上承載多個 VLAN traffic 的 switch/router interface。

## Why It Exists

若兩台 switches 都有多個 VLAN，為每個 VLAN 拉一條獨立 access link 會浪費 ports 與線材；trunk link 用一條 link 承載多個 VLAN。

## Prerequisites

- [[VLAN]]
- [[IEEE 802.1Q Tag]]

## Related Concepts

- [[Access Port]]
- [[Allowed VLAN List]]
- [[Native VLAN]]
- [[12.4-Router on a Stick]]

## Mechanism

Switch 送 frame 出 trunk port 時通常加入 802.1Q tag，接收端根據 tag 判斷 VLAN。Native VLAN traffic 通常例外，不加 tag。

Trunk 能否承載某個 VLAN，同時取決於 VLAN 是否存在、port 是否實際處於 trunking state，以及該 VLAN 是否包含在 [[Allowed VLAN List]]。因此「介面 up/up」不等於「所有 VLAN 都可通過」。

## Depends On

- [[IEEE 802.1Q Tag]] 保存非 native VLAN 的 VLAN identity。
- [[Native VLAN]] 定義 untagged traffic 的歸屬。
- [[Allowed VLAN List]] 控制每條 trunk 實際承載的 VLAN 範圍。

## Leads To

- [[12.4-Router on a Stick]] 使用 trunk 把多個 VLAN 送到 router subinterfaces。
- [[13-Dynamic Trunking Protocol]] 可以協商 switch port 的 trunk operational mode。
- [[13-VLAN Trunking Protocol]] 的 messages 透過 trunk links 在 switches 之間傳送。

## Verification

- `show interfaces switchport`：比較 administrative mode 與 operational mode。
- `show interfaces trunk`：確認 trunking state、native VLAN 與 allowed/active VLANs。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-225_744_1366_183_190.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.5 — SW1 and SW2 connected by a trunk link carrying traffic in multiple VLANs.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Knowledge Maps

- [[04_Maps/VLAN Knowledge Map|VLAN Knowledge Map]]
- [[04_Maps/VLAN Trunking Troubleshooting|VLAN Trunking Troubleshooting]]
