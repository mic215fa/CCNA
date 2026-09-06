# Unit02：TCP/IP 網路模型 REVIEW

tags: #review #acting-ccna #unit-review

## Sources

- [[01_Units/Unit02-TCPIP-網路模型|Unit02-TCPIP-網路模型]]
- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
- [[04_Maps/Unit02-TCPIP 網路模型知識地圖|Unit02：TCP/IP 網路模型知識地圖]]
- [[04_Maps/Encapsulation 與 PDU 地圖|Encapsulation 與 PDU 地圖]]
- [[05_Questions/Unit02-TCPIP-網路模型 Questions|Unit02：TCP/IP 網路模型 Questions]]

## REVIEW Items

### Concept boundary: Switch and Hop

- Chapter 4 明確說明：範例中的封包通過 switch 不算 hop；hop 是 PC1 → R1、R1 → R2、R2 → SRV1 這類 Layer 2 forwarding 範圍。
- Reason：Chapter 4 先建立觀念，但把 switch forwarding 的原因留到 Chapter 6。
- Update：Unit03 / Chapter 6 已補強此點：[[Switch]] 對 connected hosts 是 transparent，不修改 frames，只依 [[MAC Address Table]] forward/flood；因此 message passing through a switch is not considered a hop。
- Status：PARTIALLY RESOLVED；VLAN 與 STP 對 Layer 2 domain 的影響已由 Unit05／Unit06 補強，EtherChannel 尚待後續 Unit。

### Concept boundary: Ethernet

- Unit02 已把 [[Ethernet]] 連到 [[Physical Layer]] 與 [[Data Link Layer]]，但還沒有拆出 Ethernet Frame、Switching Table、MAC Learning 等細節。
- Reason：Chapter 4 只是用分層模型定位 Ethernet；Chapter 6 會提供更細的 Layer 2 switching 機制。
- Update：Unit03 已新增 [[Ethernet Frame]]、[[MAC Address Table]]、[[MAC Address Learning]]、[[Frame Forwarding]]、[[Frame Flooding]]、[[Address Resolution Protocol]]。
- Status：PARTIALLY RESOLVED；VLAN 與 STP 已由 Unit05／Unit06 補強，EtherChannel 尚待後續 Unit。

### Concept boundary: IP Address

- [[IP Address]] 目前只作為 Network Layer 的 end-to-end logical address。
- Reason：IPv4 / IPv6 address 的格式、子網路與路由行為尚未在 Unit02 展開。
- Update：Unit03 / Chapter 7 已新增 [[IPv4 Address]]、[[IPv4 Header]]、[[Prefix Length]]、[[Netmask]]、[[Network Portion and Host Portion]] 等基礎 IPv4 addressing Concepts。
- Status：PARTIALLY RESOLVED；subnetting / IPv6 still REVIEW after Chapter 11, Chapter 20, Chapter 21。

### Concept boundary: Transport Layer

- [[Transport Layer]] 與 [[Port Number]] 目前只建立「把資料交給正確應用程式」的基本關係。
- Reason：TCP、UDP、connection-oriented、reliability 等細節屬於 Chapter 22。
- Status：REVIEW after Chapter 22 TCP 與 UDP。

### Concept boundary: Application protocols

- Chapter 4 提到 HTTP、HTTPS、FTP、SSH、DNS 等 application layer protocols，但 Unit02 只建立 [[Application Layer]]，未為每個 protocol 建立獨立 Concept Note。
- Reason：目前來源只用這些 protocol 當例子；若後續章節深入處理，再拆成 Concepts 較合適。
- Status：REVIEW when those protocols become core learning targets。

### Model equivalence warning

- [[OSI Model]] 與 [[TCP-IP Model]] 可互相比較，但不應被視為每一層完全等價。
- Reason：Chapter 4 用五層 TCP/IP model 作為 CCNA 學習框架；OSI model 是 reference model。
- Status：Keep as caution in future Maps。

### Cross-unit relationships

- Unit01 的 equipment/cabling Concepts 已接到 Unit02 的 layer model：
  - [[UTP Cable]]、[[Fiber Optic Cable]] → [[Physical Layer]]
  - [[Switch]]、[[MAC Address]] → [[Data Link Layer]]
  - [[Router]]、[[IP Address]] → [[Network Layer]]
  - [[End Host]] → [[Application Layer]] / [[Transport Layer]] / [[Network Layer]]
- Reason：這些關係已記錄於 [[04_Maps/Unit02-TCPIP 網路模型知識地圖|Unit02：TCP/IP 網路模型知識地圖]]。
- Status：ACTIVE relationship set。
