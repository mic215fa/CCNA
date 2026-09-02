# Unit05：Subnetting 與 VLAN

tags: #source-note #unit #acting-ccna #subnetting #vlan #layer2 #layer3

## Sources

- [[00_Source/Chapter 11 - IPv4 網路子網劃分|Chapter 11 - IPv4 網路子網劃分]]
- [[00_Source/Chapter 12 - VLAN|Chapter 12 - VLAN]]

## Core Question

如何把一個 IPv4 address block 切成符合需求的 subnets，並用 VLAN 在 Layer 2 建立對應的 broadcast-domain segmentation，最後再透過 inter-VLAN routing 讓不同 VLAN/subnet 能受控地通訊？

## Key Ideas

- [[Subnetting]] 是把較大的 IPv4 address block 切成較小 subnets；它延伸 Unit03 的 [[Network Portion and Host Portion]]、[[Prefix Length]]、[[Netmask]] 與 [[Usable IPv4 Address Range]]。
- [[Borrowed Bits]] 是 subnetting 的核心：向 host portion 借 bits 變成 subnet/network portion，使 subnets 數量增加、每個 subnet 可用 hosts 數量下降。
- [[FLSM]] 產生相同大小的 subnets，適合練習與需求一致的設計；[[VLSM]] 依不同 LAN/WAN host 需求分配不同大小 subnets，地址使用效率更好。
- [[Subnet Five Attributes]] 把 subnetting 計算收斂成五個重點：network address、broadcast address、first usable、last usable、maximum hosts。
- [[Magic Number Method]] 是快速找 subnet boundary 的計算方法，可用來處理 CCNA 常見 subnetting 題。
- Chapter 12 說明只有 Layer 3 subnetting 還不夠；如果 switches 仍在同一 [[Broadcast Domain]]，broadcast 仍會跨部門 flooding。
- [[VLAN]] 把一台 physical switch 分成多個 virtual switches；每個 VLAN 是獨立 broadcast domain，讓 Layer 2 segmentation 與 Layer 3 subnets 對齊。
- [[Access Port]] 屬於單一 VLAN；[[Trunk Port]] 可承載多個 VLAN，靠 [[IEEE 802.1Q Tag]] 標記 frame 屬於哪個 VLAN。
- [[Native VLAN]] 的 frames 在 802.1Q trunk 上通常不加 tag；兩端 native VLAN 不一致會造成 [[Native VLAN Mismatch]]。
- [[12-VLAN Question|VLAN Trunking Troubleshooting]] 把 port mode、native VLAN 與 allowed VLAN list 串成排錯流程：先確認 port role，再確認 VLAN 歸屬與 trunk 兩端設定。
- 不同 VLAN/subnet 之間要通訊，需要 [[Inter-VLAN Routing]]；可用多條 router physical interfaces、[[12.4-Router on a Stick]]、或 [[12.4-Multilayer Switch]] + [[Switch Virtual Interface]] 實作。

## Core Concepts

- [[Subnetting]]
- [[Borrowed Bits]]
- [[FLSM]]
- [[VLSM]]
- [[Subnet Five Attributes]]
- [[Point-to-Point Subnet]]
- [[Magic Number Method]]
- [[VLAN]]
- [[Layer 3 Segmentation]]
- [[Layer 2 Segmentation]]
- [[Default VLAN]]
- [[VLAN ID]]
- [[Access Port]]
- [[Trunk Port]]
- [[IEEE 802.1Q Tag]]
- [[Allowed VLAN List]]
- [[Native VLAN]]
- [[Native VLAN Mismatch]]
- [[12-VLAN Question|VLAN Trunking Troubleshooting]]
- [[Inter-VLAN Routing]]
- [[12.4-Router on a Stick]]
- [[Subinterface]]
- [[12.4-Multilayer Switch]]
- [[Switch Virtual Interface]]
- [[Routed Port]]

## Reused Concepts

- [[IPv4 Address]]
- [[Prefix Length]]
- [[Netmask]]
- [[Network Portion and Host Portion]]
- [[IPv4 Network Address]]
- [[IPv4 Broadcast Address]]
- [[Usable IPv4 Address Range]]
- [[LAN]]
- [[Switch]]
- [[Router]]
- [[Broadcast Domain]]
- [[Layer 2 Domain]]
- [[Ethernet Frame]]
- [[EtherType]]
- [[Default Gateway]]
- [[Routing Table]]
- [[Network Interface]]

## Important Definitions

- [[Subnetting]]：把一個 IPv4 network/address block 切成多個較小 subnets。
- [[FLSM]]：Fixed-Length Subnet Masking，所有 subnets 使用相同 prefix length / subnet mask。
- [[VLSM]]：Variable-Length Subnet Masking，不同 subnets 可使用不同 prefix length，以符合不同 host 數需求。
- [[VLAN]]：在同一 physical switching infrastructure 上建立的 virtual LAN / virtual switch boundary。
- [[Access Port]]：只屬於單一 VLAN 的 switch port。
- [[Trunk Port]]：能在 switches 或 router/switch 之間承載多個 VLAN traffic 的 port。
- [[IEEE 802.1Q Tag]]：插入 Ethernet frame 中的 VLAN tag，用 VID 指出 frame 所屬 VLAN。
- [[Inter-VLAN Routing]]：讓不同 VLAN / subnets 之間透過 Layer 3 routing 通訊。
- [[Switch Virtual Interface]]：multilayer switch 上代表 VLAN 的 Layer 3 virtual interface，可作為該 VLAN 的 default gateway。

## Why These Concepts Exist

### 從「IPv4 位址」到「地址規劃」

Unit03 先建立 IPv4 address 的 network/host portion。Unit05 的 Chapter 11 把它轉成實務設計問題：給定一個 address block，如何切出足夠多 subnets，同時保留每個 subnet 所需的 host addresses？

### 從「切網段」到「隔離 broadcast domain」

Chapter 12 指出只把 hosts 放進不同 subnets 還不一定能隔離 Layer 2 broadcast。若所有 ports 仍在同一 Layer 2 domain，broadcast frame 還是會被 switch flooding。[[VLAN]] 因此存在：讓 Layer 2 segmentation 與 Layer 3 subnetting 對齊。

### 從「隔離」到「受控互通」

VLAN 隔離 traffic，但業務上不同部門仍可能需要互通。[[Inter-VLAN Routing]] 讓 traffic 必須經過 Layer 3 device，於是可以用 routing、ACL 或後續安全機制進行控制。

## Knowledge Progression

```text
[[IPv4 Address]]
  ↓ split by
[[Prefix Length]] / [[Netmask]]
  ↓ extended by
[[Subnetting]]
  ↓ implemented through
[[Borrowed Bits]]
  ↓ design styles
[[FLSM]] / [[VLSM]]
  ↓ Layer 3 segmentation
[[Layer 3 Segmentation]]
  ↓ needs matching Layer 2 boundary
[[VLAN]] / [[Layer 2 Segmentation]]
  ↓ carried across switches by
[[Trunk Port]] + [[IEEE 802.1Q Tag]]
  ↓ restored communication by
[[Inter-VLAN Routing]]
```

## Cause and Effect

- 需要多個 networks → 從 host portion 借 bits → 產生更多 subnets，但每個 subnet hosts 數變少。
- Subnet 大小固定 → 實作簡單 → [[FLSM]]；不同 LAN host 需求不同 → 避免浪費 → [[VLSM]]。
- Subnets 在 Layer 3 分開，但 switch 仍同一 broadcast domain → broadcast 仍跨部門 → 需要 [[VLAN]] 做 Layer 2 segmentation。
- 一個 switch port 只接一個 end host / VLAN → 使用 [[Access Port]]。
- 多個 VLAN 需要跨 switch 傳送 → 使用 [[Trunk Port]] 與 [[IEEE 802.1Q Tag]]。
- Native VLAN 兩端不一致 → untagged frames 被歸到錯誤 VLAN → [[Native VLAN Mismatch]]。
- VLAN 間被 Layer 2 隔離 → 不同 VLAN/subnet 不能直接互通 → 需要 [[Inter-VLAN Routing]]。
- Router physical interfaces 不足或線材太多 → 使用 [[12.4-Router on a Stick]] 與 [[Subinterface]]。
- 需要在 switch 內部高速 routing → 使用 [[12.4-Multilayer Switch]] 與 [[Switch Virtual Interface]]。

## Prerequisites

- [[Binary Number System]] before [[Borrowed Bits]] and subnet boundary calculation。
- [[IPv4 Address]]、[[Prefix Length]]、[[Netmask]] before [[Subnetting]]。
- [[IPv4 Network Address]]、[[IPv4 Broadcast Address]]、[[Usable IPv4 Address Range]] before [[Subnet Five Attributes]]。
- [[Switch]]、[[Ethernet Frame]]、[[Broadcast Domain]] before [[VLAN]]。
- [[Router]]、[[Default Gateway]]、[[Routing Table]] before [[Inter-VLAN Routing]]。

## Depends On

- [[FLSM]] depends on a single shared prefix length for all generated subnets。
- [[VLSM]] depends on choosing subnet sizes by host requirement, usually from largest requirement to smallest。
- [[VLAN]] depends on switch ports being assigned to VLANs and switch forwarding being constrained by VLAN membership。
- [[Trunk Port]] depends on [[IEEE 802.1Q Tag]] to preserve VLAN identity across a shared link。
- [[12.4-Router on a Stick]] depends on trunking and [[Subinterface]] configuration。
- [[Switch Virtual Interface]] inter-VLAN routing depends on [[12.4-Multilayer Switch]] IP routing being enabled and the VLAN/SVI being up/up。

## Leads To

- [[Subnetting]] / [[VLSM]] → route summarization, OSPF network planning, ACL matching, IPv6 prefix design。
- [[VLAN]] → trunking, DTP/VTP, STP per VLAN, EtherChannel trunking, VLAN security。
- [[IEEE 802.1Q Tag]] / [[Native VLAN]] → native VLAN mismatch troubleshooting, VLAN hopping considerations。
- [[Inter-VLAN Routing]] → router-on-a-stick labs, multilayer switching, routed ports, campus design。

## Relationships

重要關係已持久記錄於 [[04_Maps/Unit05-SubVLAN 知識地圖|Unit05：Subnetting 與 VLAN 知識地圖]] 的 `Relationships Added`。

## Contrast

### [[FLSM]] vs [[VLSM]]

- FLSM：每個 subnet 一樣大，計算直覺，但可能浪費 address space。
- VLSM：每個 subnet 可不同大小，較有效率，但需要更小心排序與 boundary 計算。

### [[Layer 3 Segmentation]] vs [[Layer 2 Segmentation]]

- Layer 3 Segmentation：用 subnets 分隔 IP networks，inter-subnet traffic 必須經過 router / L3 device。
- Layer 2 Segmentation：用 VLANs 分隔 broadcast domains，限制 frame forwarding / flooding 的範圍。

### [[Access Port]] vs [[Trunk Port]]

- Access Port：只屬於一個 VLAN，通常連 end host。
- Trunk Port：可承載多個 VLAN，通常連 switch-to-switch 或 switch-to-router。

### [[12.4-Router on a Stick]] vs [[Switch Virtual Interface]]

- ROAS：用 router 單一 physical interface + subinterfaces + trunk link 做 inter-VLAN routing。
- SVI：用 multilayer switch 在內部 VLAN interfaces 上做 routing，通常不需要外接 router 才能在 VLAN 間轉送。

## Cross-Document Relationships

- Chapter 11 建立 Layer 3 address segmentation：如何把 address block 切成 subnets。
- Chapter 12 使用 Chapter 11 的 subnetting 結果解決 LAN segmentation：每個部門使用不同 subnet，並用 VLAN 讓 Layer 2 broadcast domain 同步分離。
- Chapter 11 的 [[VLSM]] 與 Chapter 12 的 VLAN 設計共同形成「依需求規劃 IP subnet，再把 switch ports 放入對應 VLAN」的設計流程。
- Chapter 12 的 inter-VLAN routing 重新使用 Chapter 9/10 的 default gateway、routing table 與 packet life-cycle，只是 routing boundary 從 router physical link 延伸到 VLAN/subinterface/SVI。

## Cross-Unit Relationships

- Unit03 的 [[IPv4 Address]]、[[Prefix Length]]、[[Netmask]] 被 Unit05 擴展為 [[Subnetting]]、[[FLSM]]、[[VLSM]]。
- Unit03 的 [[Broadcast Domain]] 被 Unit05 的 [[VLAN]] 具體化為可設定的 Layer 2 boundary。
- Unit04 的 [[Default Gateway]] 與 [[Routing Table]] 被 Chapter 12 重用於 [[Inter-VLAN Routing]]。
- Unit04 的 [[Network Interface]] 被 Unit05 擴展為 access port、trunk port、router subinterface、SVI 與 routed port 等不同 interface 角色。
- Unit04 的 [[Packet Life Cycle]] 可套用到 VLAN 間通訊：host 送到 default gateway，L3 device route，再重新封裝回目標 VLAN。

## Important Commands / Examples

```text
vlan <vlan-id>
name <vlan-name>
show vlan brief
switchport mode access
switchport access vlan <vlan-id>
switchport mode trunk
switchport trunk allowed vlan <list>
switchport trunk native vlan <vlan-id>
show interfaces trunk
interface <physical>.<subinterface-id>
encapsulation dot1q <vlan-id>
encapsulation dot1q <vlan-id> native
ip routing
interface vlan <vlan-id>
no switchport
```

## Common Confusions

- Subnetting 切的是 Layer 3 IP address space；VLAN 切的是 Layer 2 broadcast domain。
- 不同 subnets 若還在同一 VLAN/broadcast domain，Layer 2 broadcast 仍可能跨部門擴散。
- VLAN 不會自動讓不同 VLAN 互通；inter-VLAN traffic 需要 routing。
- Access port 通常不加 802.1Q tag；trunk port 才需要 tag 來辨識 VLAN。
- Native VLAN traffic 通常不加 tag，因此兩端 native VLAN mismatch 很危險。
- ROAS 的 subinterface 編號不一定要等於 VLAN ID；真正綁定 VLAN 的是 `encapsulation dot1q`。
- SVI 要能 up/up，不只要存在 IP address；相關 VLAN 與 active port/trunk 條件也要成立。

## Questions

- 見 [[05_Questions/Unit05-SubVLAN Questions|Unit05-SubVLAN Questions]]。

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit05-SubVLAN REVIEW|Unit05-SubVLAN REVIEW]]
