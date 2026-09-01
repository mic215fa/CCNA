# IEEE 802.1Q Tag

tags: #concept #acting-ccna #vlan #ethernet

## Aliases

- 802.1Q
- Dot1Q
- VLAN tag

## Definition

IEEE 802.1Q Tag 是插入 Ethernet frame 中的 4-byte VLAN tag，用來指出 frame 屬於哪個 VLAN。

## Why It Exists

Trunk link 同時承載多個 VLAN；若沒有 tag，接收端無法知道收到的 frame 應歸入哪個 VLAN。

## Prerequisites

- [[Ethernet Frame]]
- [[VLAN]]
- [[Trunk Port]]

## Related Concepts

- [[VLAN ID]]
- [[Native VLAN]]
- [[EtherType]]

## Mechanism

802.1Q tag 插在 Ethernet frame 的 Source 與 EtherType 欄位之間。TPID 通常為 `0x8100`；TCI 包含 PCP、DEI 與 VID，其中 VID 指出 VLAN ID。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-226_314_1298_457_225.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.6 — Position and fields of the 802.1Q tag inside an Ethernet frame.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
