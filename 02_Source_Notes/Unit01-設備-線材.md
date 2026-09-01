# Unit01：設備與線材

tags: #source-note #unit #acting-ccna #network-fundamentals

## Sources

- [[00_Source/Chapter 2 - 網路設備|Chapter 2 - 網路設備]]
- [[00_Source/Chapter 3 - 線材、接頭與連接埠|Chapter 3 - 線材、接頭與連接埠]]

## Core Question

網路要讓端點交換資源，需要哪些設備角色、共同標準與實體連線條件？

## Key Ideas

- [[Computer Network]] 的核心不是「設備很多」，而是讓 [[Node]] 透過網路共享 [[Resource]]。
- [[Client-Server Model]] 說明端點如何使用或提供服務；[[End Host]] 是通訊的端點，而不是承載通訊的基礎設施。
- [[Switch]] 解決 LAN 內多個端點彼此連通的問題；[[Router]] 解決 LAN 與外部網路或其他 LAN 互通的問題；[[Firewall]] 解決開放連線帶來的安全風險。
- [[Ethernet]] 與 [[Network Standard]] 讓不同廠商、不同設備能遵守共同規則通訊。
- [[Bit and Byte]] 是網路傳輸與速度單位的基礎；網路速度以 bits per second 衡量。
- 實體連線的主要選擇是 [[UTP Cable]] 與 [[Fiber Optic Cable]]：前者便宜且常見於端點到 Switch，後者距離更遠且較常用於網路基礎設施之間。
- [[Straight-through and Crossover Cable]] 與 [[Auto MDI-X]] 說明早期 Ethernet 銅纜針腳配對如何影響設備能否通訊，以及現代設備如何自動調整。

## Core Concepts

- [[Computer Network]]
- [[LAN]]
- [[WAN]]
- [[Node]]
- [[Resource]]
- [[Client-Server Model]]
- [[End Host]]
- [[Switch]]
- [[Router]]
- [[Firewall]]
- [[Network Standard]]
- [[Ethernet]]
- [[Bit and Byte]]
- [[UTP Cable]]
- [[8P8C Connector]]
- [[Straight-through and Crossover Cable]]
- [[Auto MDI-X]]
- [[Fiber Optic Cable]]
- [[MMF and SMF]]
- [[SFP Transceiver]]

## Important Definitions

- [[LAN]]：有限區域內互相連接的一組設備，例如辦公室。
- [[WAN]]：跨越較大地理範圍的網路，例如城市之間的連線。
- [[Computer Network]]：允許 [[Node]] 分享 [[Resource]] 的 telecommunications network。
- [[Node]]：任何連上網路的設備，包含端點與網路基礎設施設備。
- [[Resource]]：可透過網路存取或使用的東西，例如網頁、印表機、遊戲伺服器或雲端軟體。
- [[Ethernet]]：一組標準家族，定義實體有線連線以及資料如何格式化並透過網路傳送。
- [[UTP Cable]]：非遮蔽雙絞線，內含 8 條線，扭成 4 組線對。
- [[Fiber Optic Cable]]：透過玻璃纖維傳送光訊號的線材。

## Why These Concepts Exist

### 從「共享資源」到「需要設備角色」

網路存在是為了讓節點共享資源。當節點只是在存取或提供服務時，可以用 [[Client-Server Model]] 理解；但當節點數量增加，就需要 [[Switch]] 作為 LAN 內的連接基礎設施。

### 從「LAN 內通訊」到「跨網路通訊」

[[Switch]] 負責 LAN 內連通，但不負責把 LAN 接到外部網路。當 LAN 要連到 Internet 或其他 LAN 時，需要 [[Router]]。當連到 Internet 帶來安全風險時，需要 [[Firewall]] 依規則檢查、允許或拒絕流量。

### 從「設備角色」到「實體與標準」

設備能不能互通，不只取決於角色，還取決於共同的 [[Network Standard]] 與通訊媒介。[[Ethernet]] 提供實體有線連線與資料格式化規則；[[UTP Cable]] 與 [[Fiber Optic Cable]] 則提供不同成本、距離與支援條件的實體媒介。

## Knowledge Progression

```text
[[Computer Network]]
  ↓
[[Node]] + [[Resource]]
  ↓
[[Client-Server Model]] / [[End Host]]
  ↓
[[Switch]] within [[LAN]]
  ↓
[[Router]] between [[LAN]] / external networks
  ↓
[[Firewall]] for traffic control and risk reduction
  ↓
[[Network Standard]] + [[Ethernet]]
  ↓
[[UTP Cable]] / [[Fiber Optic Cable]]
```

## Cause and Effect

- 沒有共同通訊規則 → 不同設備無法理解彼此資料格式 → 需要 [[Network Standard]]。
- LAN 內端點變多 → 直接連線不可擴展 → 需要 [[Switch]]。
- LAN 要連到 Internet 或其他 LAN → Switch 不負責外部網路連通 → 需要 [[Router]]。
- 允許 Internet 通訊 → 暴露安全風險 → 需要 [[Firewall]]。
- UTP 超過最大長度 → signal attenuation 與效能下降 → 遠距離或跨樓層/建築更常用 [[Fiber Optic Cable]]。
- 相同 Tx pin pair 的設備用 straight-through cable 直連 → Tx 對 Tx，無法通訊 → 需要 crossover cable 或 [[Auto MDI-X]]。

## Prerequisites

- 理解 [[Bit and Byte]]，才能理解網路速度與 Ethernet 標準中的 Mbps/Gbps。
- 理解 [[LAN]] 與 [[WAN]]，才能區分 [[Switch]] 與 [[Router]] 的角色。
- 理解 [[Network Standard]]，才能理解為何 [[Ethernet]] 能讓不同設備互通。
- 理解 [[8P8C Connector]] 與 pin pairs，才能理解 [[Straight-through and Crossover Cable]] 與 [[Auto MDI-X]]。

## Depends On

- [[Ethernet]] depends on [[Network Standard]]：Ethernet 是被標準化的一組通訊與實體連線規則。
- [[UTP Cable]] practical Ethernet use depends on [[8P8C Connector]]：Chapter 3 以 8P8C port/connector 說明 UTP Ethernet 連線。
- [[Straight-through and Crossover Cable]] depends on pin pair behavior：線材類型的差異來自兩端 pins 的連接方式。
- [[Auto MDI-X]] depends on device ability to change Tx/Rx pins：設備能依對端調整傳送/接收 pins。

## Leads To

- [[Switch]] → 後續 Chapter 6 Ethernet LAN switching。
- [[Router]] → 後續 Chapter 7 IPv4 addressing 與 routing 概念。
- [[Firewall]] → 後續安全與 ACL 相關單元。
- [[Ethernet]] → [[Switch]]、[[UTP Cable]]、[[Fiber Optic Cable]]、後續 VLAN/STP 等 Layer 2 主題。
- [[Bit and Byte]] → IPv4 binary conversion、subnetting、網路速度與頻寬理解。

## Relationships

| Relationship | 說明 | Confidence |
|---|---|---|
| [[Client-Server Model]] contrasts with physical device type | Client/Server 是角色，不是固定硬體種類。 | Explicit |
| [[Switch]] connects devices within [[LAN]] | Switch 為 LAN 內設備提供連通性。 | Explicit |
| [[Router]] connects LANs and external networks | Router 放在 LAN edge，讓 LAN 與外部網路通訊。 | Explicit |
| [[Firewall]] mitigates Internet exposure risk | Firewall 檢查進出網路流量，依規則允許或拒絕。 | Explicit |
| [[Ethernet]] bridges Chapter 2 and Chapter 3 | Chapter 2 的設備連線，在 Chapter 3 由 Ethernet 實體標準與線材具體化。 | Strongly Inferred |
| [[UTP Cable]] contrasts with [[Fiber Optic Cable]] | UTP 成本低、端點常用；Fiber 距離遠、成本高、基礎設施常用。 | Explicit |
| [[Auto MDI-X]] reduces cable-selection burden | 現代設備可自動調整 Tx/Rx pins。 | Explicit |

## Contrast

### [[Switch]] vs [[Router]]

- Switch：連接同一 [[LAN]] 內的設備。
- Router：連接不同 LAN 或 LAN 與外部網路。

### Host-based Firewall vs [[Firewall]]

- Host-based firewall：在單一主機上檢查該主機進出流量。
- Network firewall：獨立硬體設備，檢查整個網路進出流量。

> [!note]
> 本 Unit 建立的 Concept Note 使用 [[Firewall]] 作為 network firewall 的主概念；host-based firewall 目前保留在 Source Note 中，不另建概念，避免過度碎片化。

### [[UTP Cable]] vs [[Fiber Optic Cable]]

- UTP：便宜、普遍、端點常用、100 m 上限、易受 EMI 影響。
- Fiber：距離遠、成本高、需 SFP transceiver、較常用於網路設備間。

### Straight-through vs Crossover

- Straight-through：兩端相同 pin pair 對接；PC-to-Switch 這類相反 Tx/Rx 設備適用。
- Crossover：交叉相反 pin pairs；相同 Tx/Rx pin pair 的設備直連時需要。

## Cross-Document Relationships

- Chapter 2 先介紹「設備為什麼存在」：端點、Switch、Router、Firewall 分別解決不同網路通訊問題。
- Chapter 3 接著介紹「設備如何實體連起來」：共同標準、bit/binary、Ethernet、UTP/Fiber、connector/port 與 pin pair。
- [[Switch]] 在 Chapter 2 被定義為 LAN 內基礎設施；Chapter 3 說明 Switch port 與 8P8C connector / UTP cable 如何形成實體連線。
- [[Router]] 在 Chapter 2 被定義為 LAN 與外部網路之間的設備；Chapter 3 的 straight-through/crossover 範例使用 Router-to-Router 直連說明 pin pair 不匹配時的問題。
- [[Firewall]] 在 Chapter 2 被定義為保護網路的設備；Chapter 3 的 pin pair 表格把 Firewall 與 Router/PC/Server 歸為相同 Tx/Rx pin 行為。
- Chapter 2 的 [[LAN]] 需求引出 Chapter 3 的 Ethernet LAN cabling；Chapter 3 的 [[UTP Cable]] 與 [[Fiber Optic Cable]] 又解釋 LAN 內、樓層間、建築間連線如何選擇媒介。

## Cross-Unit Relationships

目前只有一個 Unit 被正式處理，因此沒有已確認的 cross-unit relationship。

## Important Commands / Examples

本 Unit 主要是概念與實體連線，沒有重點 CLI command。重要例子包括：

- PC-to-Switch 使用 straight-through cable 的 pin pair 行為。
- Router-to-Router 使用 straight-through cable 導致 Tx-to-Tx 無法通訊。
- Auto MDI-X 讓 Router 可以自動反轉 Tx/Rx pins。
- UTP 與 Fiber 的使用情境：端點到 Switch vs 網路基礎設施之間。

## Common Confusions

- Server 不是一定指高效能硬體；在 Client/Server 模型中，它首先是一種角色。
- Switch 不負責連接 Internet；Router 才負責 LAN 與外部網路連通。
- 「Ethernet cable」常指 UTP 網路線，但 Ethernet 不只是一條線，而是一組標準。
- RJ45 常被用來稱呼 Ethernet cable connector，但 Chapter 3 指出嚴格來說此名稱並不精確。
- Mbps/Gbps 中的 bit 不等於 byte；1 byte = 8 bits。
- 1 kilobit 在 CCNA 使用 1,000 bits，不是 1,024 bits；1,024 base-2 用 kibibit 等術語。

## Questions

- 見 [[05_Questions/Unit01-設備-線材 Questions|Unit01-設備-線材 Questions]]。

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit01-設備-線材 REVIEW|Unit01-設備-線材 REVIEW]]
