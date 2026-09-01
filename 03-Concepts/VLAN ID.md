# VLAN ID

tags: #concept #acting-ccna #vlan

## Definition

VLAN ID 是用來識別 VLAN 的數字；在 802.1Q tag 中由 VID field 表示。

## Why It Exists

當 trunk link 同時承載多個 VLAN 的 frames，接收端 switch 必須知道每個 frame 屬於哪個 VLAN。

## Related Concepts

- [[VLAN]]
- [[IEEE 802.1Q Tag]]
- [[Trunk Port]]
- [[Subinterface]]

## Mechanism

802.1Q tag 的 VID field 為 12 bits，可支援 4096 個值，其中 VLAN 1–4094 可用；0 與 4095 保留。

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
