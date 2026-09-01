# Access Port

tags: #concept #acting-ccna #vlan #switching

## Definition

Access Port 是被指派到單一 VLAN 的 switch port，通常用來連接 end host。

## Why It Exists

End host 通常不需要知道 VLAN tag；access port 讓 host 以一般 untagged Ethernet frame 通訊，同時由 switch 決定該 frame 屬於哪個 VLAN。

## Related Concepts

- [[VLAN]]
- [[Trunk Port]]
- [[Default VLAN]]
- [[Layer 2 Segmentation]]

## Mechanism

使用 `switchport mode access` 與 `switchport access vlan <vlan-id>` 將 port 放入指定 VLAN。Switch 只會在同一 VLAN 的 ports 之間 forward/flood traffic。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-220_873_1321_1052_223.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.4 — Switch interfaces assigned to VLAN 10, VLAN 20, and VLAN 30.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
