# Unit05：Subnetting 與 VLAN Questions

tags: #questions #acting-ccna #unit-questions #subnetting #vlan

## Source

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
- [[00_Source/Chapter 11 - IPv4 網路子網劃分|Chapter 11 - IPv4 網路子網劃分]]
- [[00_Source/Chapter 12 - VLAN|Chapter 12 - VLAN]]

## Subnetting Reasoning

1. 為什麼 [[Subnetting]] 必須從 [[Network Portion and Host Portion]] 開始理解？
2. 借用更多 [[Borrowed Bits]] 會同時帶來什麼好處與代價？
3. 為什麼 [[FLSM]] 容易計算，但在不同 LAN host 需求差很多時可能浪費 addresses？
4. [[VLSM]] 為什麼通常要從最大 host 需求的 subnet 開始分配？
5. [[Subnet Five Attributes]] 中，哪幾個值不能指派給一般 host？為什麼？
6. Point-to-point link 為什麼常用 /30 或 /31，而不是較大的 subnet？
7. [[Magic Number Method]] 解決的是哪一類 subnetting 心算問題？

## VLAN Reasoning

1. 為什麼只做 [[Layer 3 Segmentation]]，不一定能阻止 Layer 2 broadcast 跨部門？
2. [[VLAN]] 如何把一台 physical switch 變成多個 virtual switches？
3. [[Access Port]] 與 [[Trunk Port]] 的核心差異是什麼？
4. 為什麼 trunk link 需要 [[IEEE 802.1Q Tag]]？
5. [[VLAN ID]] 在 802.1Q tag 中扮演什麼角色？
6. 修改 [[Allowed VLAN List]] 時忘記 `add` 可能造成什麼後果？
7. [[Native VLAN]] 為什麼容易成為 troubleshooting 與安全上的敏感點？
8. [[Native VLAN Mismatch]] 為什麼會讓 frames 被放進錯誤 VLAN？
9. 同樣是 untagged frame，進入 [[Access Port]] 與 [[Trunk Port]] 時，switch 分別依什麼規則判定 VLAN 歸屬？
10. 若同一 VLAN 的 hosts 位於兩台 switches 上卻無法互通，如何依序用 port mode、[[Allowed VLAN List]] 與 [[Native VLAN Mismatch]] 縮小問題範圍？
11. 為什麼 `switchport trunk allowed vlan 40` 與 `switchport trunk allowed vlan add 40` 可能造成完全不同的 traffic 結果？

## Inter-VLAN Routing Reasoning

1. 為什麼 VLAN 間不能只靠 switch Layer 2 forwarding 互通？
2. [[Inter-VLAN Routing]] 如何重用 Unit04 的 [[Default Gateway]] 與 [[Routing Table]]？
3. Router 每個 VLAN 一條 physical interface 的設計，優點與限制是什麼？
4. [[12.4-Router on a Stick]] 如何用一條 trunk link 支援多個 VLAN？
5. [[Subinterface]] 的編號為什麼不一定要等於 VLAN ID？真正決定 VLAN mapping 的設定是什麼？
6. [[12.4-Multilayer Switch]] 與外接 router 相比，對 inter-VLAN routing 有什麼優勢？
7. [[Switch Virtual Interface]] 要 up/up 需要哪些條件？
8. [[Routed Port]] 與一般 switchport 的差異是什麼？

## Cross-unit

1. Unit03 的 [[IPv4 Address]]、[[Prefix Length]]、[[Netmask]] 如何成為 Unit05 subnetting 的前提？
2. Unit03 的 [[Broadcast Domain]] 在 Unit05 如何被 [[VLAN]] 變成可設定邊界？
3. Unit04 的 [[Network Interface]] 為什麼在 Unit05 需要拆成 access port、trunk port、subinterface、SVI、routed port？
4. Unit04 的 [[Packet Life Cycle]] 如何套用到 VLAN 間通訊？
5. Unit05 的 [[VLAN]] / [[Trunk Port]] 將如何銜接後續 DTP、VTP、STP、EtherChannel？

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit05-SubVLAN REVIEW|Unit05-SubVLAN REVIEW]]
