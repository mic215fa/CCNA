# Unit01：設備與線材 REVIEW

tags: #review #acting-ccna #unit-review

## Sources

- [[01_Units/Unit01-設備-線材|Unit01-設備-線材]]
- [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]]
- [[04_Maps/Unit01-設備與線材知識地圖|Unit01：設備與線材知識地圖]]
- [[04_Maps/UTP 與 Fiber 決策地圖|UTP 與 Fiber 決策地圖]]
- [[05_Questions/Unit01-設備-線材 Questions|Unit01：設備與線材 Questions]]

## REVIEW Items

### Concept boundary: Ethernet

- 後續處理 Chapter 4～6 時，應檢查 [[Ethernet]] 是否需要拆出或新增更細概念，例如 Ethernet Frame、MAC Address、Switching。
- Reason：Unit01 只涵蓋 Ethernet 作為標準家族與實體連線；Chapter 6 會進一步處理 Ethernet LAN switching，可能需要更細的 Concept Notes。
- Update：Unit02 已新增 [[MAC Address]]，並將 [[Ethernet]] 連到 [[Physical Layer]] 與 [[Data Link Layer]]；但 Ethernet Frame 與 Switching 細節仍待 Chapter 6。
- Status：REVIEW after Chapter 6。

### Concept boundary: Firewall

- 後續處理 Security 或 ACL 相關 Unit 時，應補充 [[Firewall]] 與 ACL、Security Concepts、可能的 stateful inspection 等概念關係。
- Reason：Unit01 只高層次定義 network firewall 的角色；後續安全章節會提供更多機制與關係。
- Status：REVIEW after Security / ACL Units。

### Concept boundary: WAN

- 後續處理 WAN 或 Volume 2 相關章節時，應擴充 [[WAN]] 的類型、技術、以及與 [[Router]] 的關係。
- Reason：Unit01 只把 WAN 定義為跨大地理範圍的網路，沒有展開 WAN connection types。
- Status：REVIEW when WAN materials are available。

### Map expansion: Unit01 knowledge map

- 後續處理 Chapter 4～6 時，檢查 [[04_Maps/Unit01-設備與線材知識地圖|Unit01：設備與線材知識地圖]] 是否需要加入 OSI/TCP-IP model、Ethernet frame、MAC address、Switching 等概念。
- Reason：目前 Map 只涵蓋設備角色、實體線材、標準與 pin/cable 機制。
- Status：REVIEW after Chapter 4～6。

### Cabling decision details

- Future Units may add exact SMF/MMF standards, connector types, transceiver names, and real deployment constraints to [[04_Maps/UTP 與 Fiber 決策地圖|UTP 與 Fiber 決策地圖]]。
- Reason：Unit01 提供 UTP vs Fiber 的高層取捨，但未涵蓋更細的光纖規格與部署細節。
- Status：REVIEW when later cabling or infrastructure material appears。

### Directory naming consistency

- `AGENTS.md` 使用建議目錄名 `02-Source-Notes`、`04-Maps`、`06-Review`；本 Vault 實際使用 `02_Source_Notes`、`04_Maps`、`06_review`。
- Reason：目前依實際目錄寫入；未來若要統一目錄命名，需要使用者明確指示。
- Status：REVIEW if user requests directory normalization。

### Cross-unit relationships

- Unit02 已建立第一批 cross-unit relationships，將 Unit01 的設備與線材概念接到 TCP/IP 分層模型。
- Relationships：[[UTP Cable]] / [[Fiber Optic Cable]] → [[Physical Layer]]；[[Switch]] → [[Data Link Layer]]；[[Router]] → [[Network Layer]]；[[End Host]] → [[Application Layer]] / [[Transport Layer]] / [[Network Layer]]。
- Durable record：[[04_Maps/Unit02-TCPIP 網路模型知識地圖|Unit02：TCP/IP 網路模型知識地圖]]。
- Status：ACTIVE relationship set。
