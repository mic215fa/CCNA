# Unit03：CLI、Ethernet、IPv4 REVIEW

tags: #review #acting-ccna #unit-review

## Sources

- [[01_Units/Unit03-CLI-Eth-IPV4|Unit03-CLI-Eth-IPV4]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
- [[04_Maps/Unit03-CLI-Eth-IPV4 知識地圖|Unit03：CLI、Ethernet、IPv4 知識地圖]]
- [[04_Maps/Ethernet Switching 與 ARP 地圖|Ethernet Switching 與 ARP 地圖]]
- [[04_Maps/IPv4 Addressing 基礎地圖|IPv4 Addressing 基礎地圖]]
- [[04_Maps/Cisco IOS CLI 與 Configuration 地圖|Cisco IOS CLI 與 Configuration 地圖]]
- [[05_Questions/Unit03-CLI-Eth-IPV4 Questions|Unit03：CLI、Ethernet、IPv4 Questions]]

## REVIEW Items

### Concept boundary: IOS modes

- [[Cisco IOS Command Mode]] 保留為核心狀態機；User EXEC、Privileged EXEC、Global Configuration 已合併為該頁的 anchors，不再各自維持薄 Concept Note。
- Reason：三者必須一起理解其轉移與權限範圍；後續 interface、line、router configuration modes 可擴充同一狀態機，無須預先碎片化。
- Status：RESOLVED；只有當某個子模式形成跨 Unit 的獨立機制時，才重新評估拆分。

### Concept boundary: Passwords and secrets

- [[Cisco IOS CLI#Privileged Access Protection|Enable Password]] 與 [[Cisco IOS CLI#Privileged Access Protection|Enable Secret]] 已整合為 CLI 核心頁中的比較區塊。
- Reason：目前兩者主要價值是同一升權保護機制內的對比，不足以各自成為可重用核心。
- Status：RESOLVED for Unit03；後續 security Unit 應建立較上層的 device access control / AAA 概念，而非恢復兩篇薄頁。

### Concept boundary: Configuration states

- Running Config 與 Startup Config 已整合為 [[IOS Configuration File#Running Configuration|Running Configuration]]、[[IOS Configuration File#Startup Configuration|Startup Configuration]] anchors。
- Reason：兩者的知識價值來自 apply / save / boot lifecycle 的對照，分開後反而失去狀態轉換脈絡。
- Status：RESOLVED；生命週期已記錄於 [[04_Maps/Cisco IOS CLI 與 Configuration 地圖|Cisco IOS CLI 與 Configuration 地圖]]。

### Concept boundary: Ethernet Frame fields

- Unit03 目前只拆出 [[Preamble and SFD]]、[[EtherType]]、[[Frame Check Sequence]]，沒有為 Destination / Source fields 各自獨立成概念。
- Reason：Destination / Source 的重點已由 [[MAC Address]]、[[MAC Address Learning]]、[[Frame Forwarding]] 承載；目前不需要額外碎片化。
- Status：OK unless later source gives deeper field-specific behavior。

### Concept boundary: ARP scope

- [[Address Resolution Protocol]] 目前以 same-LAN ARP 為主。
- Reason：Chapter 7 Figure 7.11 提到 PC1 到 PC3 經過 router 時，PC1 需 ARP R1 G0/0 MAC，R1 需 ARP PC3 MAC；這會在 packet life-cycle / routing Units 變得更重要。
- Status：REVIEW after Chapter 10 packet life-cycle and routing Units。

### Concept boundary: IPv4 Header fields

- [[IPv4 Header]] 目前沒有為所有 14 個 fields 各自拆 Concept。
- Reason：AGENTS.md 要求避免每個名詞都拆筆記；目前只拆出後續高重用度較高的 [[Time To Live]]、[[Packet Fragmentation]]、[[Maximum Transmission Unit]]。
- Status：REVIEW when QoS、ACL、OSPF、fragmentation troubleshooting appear。

### Concept boundary: IPv4 addressing vs subnetting

- Unit03 只建立 classful / basic IPv4 addressing；subnetting 尚未處理。
- Reason：Chapter 7 明確說 subnetting 會在 Chapter 11 深入。
- Status：REVIEW after Chapter 11。

### Cross-unit relationship resolution

- Unit02 REVIEW 中 Switch/Hop 與 Ethernet boundary 已被 Unit03 部分補強：
  - Switch transparent to hosts。
  - Switch 不修改 frames，只依 destination MAC forward/flood。
  - Ethernet frame / MAC address table / ARP 補強 Data Link Layer。
- Reason：Chapter 6 提供 Unit02 尚未展開的 Layer 2 switching details。
- Status：PARTIALLY RESOLVED；VLAN 與 STP 已由 Unit05／Unit06 補強，EtherChannel 尚待後續 Unit。
