# Unit01：設備與線材知識地圖

tags: #map #knowledge-map #acting-ccna

## Sources

- [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]]

## Core Dependency Chain

```text
[[Computer Network]]
  ↓ requires
[[Node]] + [[Resource]]
  ↓ explains endpoint behavior through
[[Client-Server Model]]
  ↓ endpoints connect through
[[Switch]]
  ↓ inside
[[LAN]]
  ↓ external connectivity requires
[[Router]]
  ↓ exposure to public networks motivates
[[Firewall]]
```

## Physical Connectivity Chain

```text
[[Network Standard]]
  ↓ enables interoperability through
[[Ethernet]]
  ↓ sends
[[Bit and Byte]]
  ↓ over
[[UTP Cable]] or [[Fiber Optic Cable]]
```

## UTP Mechanism Chain

```text
[[UTP Cable]]
  ↓ uses
[[8P8C Connector]]
  ↓ pin pairing creates
[[Straight-through and Crossover Cable]]
  ↓ modern mitigation
[[Auto MDI-X]]
```

## Fiber Mechanism Chain

```text
[[Fiber Optic Cable]]
  ↓ often connects through
[[SFP Transceiver]]
  ↓ comes in
[[MMF and SMF]]
  ↓ chosen based on
distance / cost / device support
```

## Cross-Document Bridge

```text
Chapter 2: What roles do network devices play?
  ↓
[[Switch]] / [[Router]] / [[Firewall]]
  ↓ need physical connectivity
Chapter 3: What standards and media make that connectivity work?
  ↓
[[Ethernet]] / [[UTP Cable]] / [[Fiber Optic Cable]]
```

## Relationships Added

> [!info] Durable relationship record
> 本區塊記錄處理 [[01_Units/Unit01-設備-線材|Unit01-設備-線材]] 時新增的重要關係；最終報告中的 `Relationships Added` 應回指到此類 Map 紀錄。

| Relationship | Type | Confidence | Notes |
|---|---|---|---|
| [[Computer Network]] → [[Node]] + [[Resource]] | Definition structure | Explicit | Chapter 2 定義 computer network 為允許 nodes share resources 的 telecommunications network。 |
| [[Node]] + [[Resource]] → [[Client-Server Model]] | Leads to | Explicit | Chapter 2 從 node/resource 進入分享資源的 client/server 角色。 |
| [[Client-Server Model]] → [[End Host]] | Leads to | Explicit | Client/Server nodes often called endpoints or end hosts。 |
| [[End Host]] → [[Switch]] → [[LAN]] | Problem → Solution | Explicit | 多個 end hosts 要在 LAN 內互通，需要 Switch 作為 infrastructure。 |
| [[LAN]] → [[Router]] → [[WAN]] / Internet | Problem → Solution | Explicit | LAN 要與外部網路或 Internet 通訊，需要 Router。 |
| Internet exposure → [[Firewall]] | Cause → Effect / Problem → Solution | Explicit | 允許 Internet 通訊會帶來安全風險，因此使用 Firewall。 |
| [[Network Standard]] → [[Ethernet]] | General → Specific | Explicit | Ethernet 是 IEEE 802.3 定義的一組標準家族。 |
| [[Ethernet]] → [[UTP Cable]] / [[Fiber Optic Cable]] | Implementation choices | Explicit | Chapter 3 說明 Ethernet 使用 copper 與 fiber-optic cable types。 |
| [[Bit and Byte]] → network speed units | Prerequisite | Explicit | 網路速度以 bits per second 衡量。 |
| [[UTP Cable]] → [[8P8C Connector]] | Physical dependency | Explicit | UTP Ethernet cable 透過 8P8C connector 與 device port 連接。 |
| [[8P8C Connector]] → [[Straight-through and Crossover Cable]] | Mechanism dependency | Explicit | pin pair 連接方式決定 straight-through/crossover 行為。 |
| [[Straight-through and Crossover Cable]] → [[Auto MDI-X]] | Problem → Solution | Explicit | Auto MDI-X 讓設備自動調整 Tx/Rx pins，降低線材選擇問題。 |
| [[Fiber Optic Cable]] → [[SFP Transceiver]] | Physical dependency | Explicit | 典型 fiber 連線接到 SFP transceiver，再插入 SFP port。 |
| [[Fiber Optic Cable]] → [[MMF and SMF]] | Type hierarchy | Explicit | Chapter 3 定義 fiber 的兩大類型：MMF 與 SMF。 |
| [[UTP Cable]] vs [[Fiber Optic Cable]] | Contrast | Explicit | UTP 成本低、端點常用；Fiber 距離遠、成本高、基礎設施間常用。 |
| Chapter 2 device roles → Chapter 3 physical connectivity | Cross-document relationship | Strongly Inferred | Chapter 2 解釋設備角色；Chapter 3 解釋讓這些設備實際連接的標準、線材與接頭。 |

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit01-設備-線材 REVIEW|Unit01-設備-線材 REVIEW]]
