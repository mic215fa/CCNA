# Unit03：CLI、Ethernet、IPv4 REVIEW

tags: #review #acting-ccna #unit-review

## Sources

- [[01_Units/Unit03-CLI-Eth-IPV4|Unit03-CLI-Eth-IPV4]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
- [[04_Maps/Unit03-CLI-Eth-IPV4 知識地圖|Unit03：CLI、Ethernet、IPv4 知識地圖]]
- [[04_Maps/Ethernet Switching 與 ARP 地圖|Ethernet Switching 與 ARP 地圖]]
- [[04_Maps/IPv4 Addressing 基礎地圖|IPv4 Addressing 基礎地圖]]
- [[05_Questions/Unit03-CLI-Eth-IPV4 Questions|Unit03：CLI、Ethernet、IPv4 Questions]]

## REVIEW Items

### Concept boundary: IOS modes

- Unit03 目前建立 [[Cisco IOS Command Mode]]、[[User EXEC Mode]]、[[Privileged EXEC Mode]]、[[Global Configuration Mode]]。
- Reason：後續章節會出現 interface configuration mode、line configuration mode、router configuration mode 等子模式，可能需要再拆分或更新。
- Status：REVIEW when later configuration modes appear。

### Concept boundary: Passwords and secrets

- [[Enable Password]] 與 [[Enable Secret]] 已拆成兩篇，因考試常比較兩者。
- Reason：後續 volume 2 security chapter 會補充更多 secret hashing algorithms 與 device access control。
- Status：REVIEW after security / device access control Unit。

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
- Status：PARTIALLY RESOLVED；VLAN/STP/EtherChannel later。

