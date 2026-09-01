# Allowed VLAN List

tags: #concept #acting-ccna #vlan #trunk

## Definition

Allowed VLAN List 是 trunk port 上允許通過的 VLAN 清單。

## Why It Exists

Trunk port 預設可能允許很多 VLAN；限制 allowed VLANs 可以控制哪些 VLAN 的 traffic 能跨越該 trunk link。

## Related Concepts

- [[Trunk Port]]
- [[VLAN]]
- [[IEEE 802.1Q Tag]]

## Mechanism

可用 `switchport trunk allowed vlan <list>` 修改清單。使用 `add` 時是新增到現有清單；若忘記 `add`，可能會覆蓋原本允許的 VLAN 清單。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-228_445_1300_339_346.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Section 12.3.2 — `show interfaces trunk` output including VLANs allowed on trunk.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Questions

- 為什麼修改 allowed VLAN list 時忘記 `add` 可能造成既有 VLAN traffic 中斷？
