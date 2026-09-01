# Unit02：TCP/IP 網路模型

tags: #source-note #unit #acting-ccna #network-fundamentals

## Sources

- [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]]

## Core Question

網路通訊為什麼需要分層模型？TCP/IP model 如何把「應用程式資料」一路轉換成能在實體媒介上傳送的 bits，並在目的端還原給正確的應用程式？

## Key Ideas

- [[Networking Model]] 是理解複雜網路通訊的框架；它把通訊所需功能分成 layers，讓不同 [[Protocol]] 能在各層扮演特定角色。
- [[OSI Model]] 雖然不是現代網路實際使用的模型，但它深刻影響網路術語；例如 Application Layer 通常仍稱為 Layer 7。
- [[TCP-IP Model]] 是現代網路使用的模型；本書採用五層版本：[[Physical Layer]]、[[Data Link Layer]]、[[Network Layer]]、[[Transport Layer]]、[[Application Layer]]。
- [[Data Link Layer]] 使用 [[MAC Address]] 做 hop-to-hop delivery；[[Network Layer]] 使用 [[IP Address]] 做 end-to-end delivery；[[Transport Layer]] 使用 [[Port Number]] 把資料送到正確應用程式。
- [[Encapsulation and De-encapsulation]] 解釋資料如何由上層往下層逐步加上 header/trailer，送出後在目的端逐層移除。
- [[Protocol Data Unit]] 與 [[Payload]] 說明每一層如何稱呼與包住上一層資料：segment、packet、frame。
- [[Adjacent-layer Interaction]] 與 [[Same-layer Interaction]] 說明同一台主機內上下層如何互相服務，以及不同主機的同層如何透過 header/trailer 進行邏輯互動。

## Core Concepts

- [[Networking Model]]
- [[Protocol]]
- [[OSI Model]]
- [[TCP-IP Model]]
- [[Physical Layer]]
- [[Data Link Layer]]
- [[Network Layer]]
- [[Transport Layer]]
- [[Application Layer]]
- [[Hop]]
- [[MAC Address]]
- [[IP Address]]
- [[Port Number]]
- [[Forwarding]]
- [[Encapsulation and De-encapsulation]]
- [[Header and Trailer]]
- [[Protocol Data Unit]]
- [[Payload]]
- [[Adjacent-layer Interaction]]
- [[Same-layer Interaction]]

## Reused Concepts

- [[Network Standard]]
- [[Ethernet]]
- [[Bit and Byte]]
- [[UTP Cable]]
- [[Fiber Optic Cable]]
- [[Node]]
- [[End Host]]
- [[Switch]]
- [[Router]]

## Important Definitions

- [[Protocol]]：定義資料應如何在網路設備間通訊的一組規則。
- [[Networking Model]]：定義資料如何從 source 到 destination 所需功能的框架。
- [[TCP-IP Model]]：現代網路使用的模型，本書採用五層版本。
- [[Hop]]：訊息從路徑中的一個 node 到下一個 node 的旅程。
- [[MAC Address]]：Layer 2 用來把訊息送到 next hop 的位址。
- [[IP Address]]：Layer 3 用來把訊息送到 final destination host 的位址。
- [[Port Number]]：Layer 4 用來把訊息送到目的主機上正確 application process 的位址。
- [[Header and Trailer]]：封裝時加入訊息前方或後方的補充資料。
- [[Protocol Data Unit]]：某一層處理後的訊息名稱，例如 segment、packet、frame。

## Why These Concepts Exist

### 從「Ethernet 不夠」到「需要模型」

Unit01 的 [[Ethernet]] 說明實體有線連線與部分通訊規則，但 Chapter 4 明確指出：Ethernet alone isn't sufficient for two computers to communicate over a network。完整通訊需要多種 [[Protocol]] 共同運作，因此需要 [[Networking Model]] 當作組織框架。

### 從「分層」到「模組化」

分層讓每一層只處理自己的角色。應用程式不需要知道底層到底是 [[Ethernet]]、Wi-Fi、[[UTP Cable]] 或 [[Fiber Optic Cable]]；只要每層完成自己的功能，整體通訊就能成立。

### 從「送到主機」到「送到應用程式」

[[Data Link Layer]] 解決 next-hop delivery；[[Network Layer]] 解決 end-to-end delivery；但資料到達主機仍不夠，還需要 [[Transport Layer]] 透過 [[Port Number]] 把資料送到正確應用程式。

## Knowledge Progression

```text
[[Network Standard]]
  ↓ enables common rules
[[Protocol]]
  ↓ organized by
[[Networking Model]]
  ↓ modern model
[[TCP-IP Model]]
  ↓ layered delivery
[[Physical Layer]]
  ↓ bits over medium
[[Data Link Layer]]
  ↓ hop-to-hop via [[MAC Address]]
[[Network Layer]]
  ↓ end-to-end via [[IP Address]]
[[Transport Layer]]
  ↓ application process via [[Port Number]]
[[Application Layer]]
```

## Encapsulation Progression

```text
Application data
  ↓ Layer 4 header
[[Protocol Data Unit|Segment]]
  ↓ Layer 3 header
[[Protocol Data Unit|Packet]]
  ↓ Layer 2 header + trailer
[[Protocol Data Unit|Frame]]
  ↓ Layer 1
Bits on physical medium
```

## Cause and Effect

- Vendor-proprietary protocols → different vendors cannot easily communicate → vendor-neutral [[Protocol]] / [[Networking Model]] become valuable。
- Network communication is complex → functions are split into layers → protocols can be designed for each layer。
- Message must cross multiple nodes → [[Data Link Layer]] handles hop-to-hop delivery with [[MAC Address]]。
- Message must keep the same final destination → [[Network Layer]] handles end-to-end delivery with [[IP Address]]。
- Destination host runs many applications → [[Transport Layer]] uses [[Port Number]] to select the correct application process。
- Data descends through layers before transmission → [[Encapsulation and De-encapsulation]] adds headers/trailer → [[Protocol Data Unit]] names change by layer。

## Prerequisites

- [[Network Standard]] and [[Protocol]] before [[Networking Model]]。
- [[Ethernet]] / [[UTP Cable]] / [[Fiber Optic Cable]] before [[Physical Layer]]。
- [[Node]]、[[Switch]]、[[Router]] before [[Hop]] and [[Data Link Layer]] delivery。
- [[Bit and Byte]] before [[Physical Layer]] and data transmission。

## Depends On

- [[TCP-IP Model]] depends on [[Protocol]]：模型本身由多種 protocols 在不同 layers 扮演角色。
- [[Data Link Layer]] depends on physical medium：Layer 2 prepares data for transmission over a medium provided by Layer 1。
- [[Network Layer]] depends on [[IP Address]]：Layer 3 uses IP address for end-to-end delivery。
- [[Transport Layer]] depends on [[Port Number]]：Layer 4 uses port numbers to address application processes。
- [[Encapsulation and De-encapsulation]] depends on [[Header and Trailer]]：封裝/解封裝的核心就是加入、檢查與移除 headers/trailer。

## Leads To

- [[Data Link Layer]] → Ethernet LAN switching, MAC address table, ARP。
- [[Network Layer]] → IPv4/IPv6 addressing, routing, subnetting。
- [[Transport Layer]] → TCP, UDP, sockets, sessions。
- [[Application Layer]] → HTTP, HTTPS, DNS, FTP, SSH。
- [[Encapsulation and De-encapsulation]] → packet life-cycle, troubleshooting by layer。

## Relationships

重要關係已持久記錄於 [[04_Maps/Unit02-TCPIP 網路模型知識地圖|Unit02：TCP/IP 網路模型知識地圖]] 的 `Relationships Added`。

## Contrast

### [[OSI Model]] vs [[TCP-IP Model]]

- OSI：七層 conceptual model，未實際用於現代網路，但術語仍常用。
- TCP/IP：現代網路使用的模型；本書採用五層版本，但 Application Layer 仍常稱 Layer 7。

### [[Data Link Layer]] vs [[Network Layer]]

- Data Link：hop-to-hop delivery，目的 MAC address 每 hop 改變。
- Network：end-to-end delivery，目的 IP address 在整段旅程中保持不變。

### [[Port Number]] vs physical port

- Port number：Layer 4 address，用來定位 application process。
- Physical port：Layer 1 實體接頭，用來接線。

## Cross-Document Relationships

- Unit01 的 [[Ethernet]]、[[UTP Cable]]、[[Fiber Optic Cable]] 在 Unit02 成為 [[Physical Layer]] 的具體例子。
- Unit01 的 [[Switch]] 與 [[Router]] 在 Unit02 中被放進 Layer 2 / Layer 3 delivery 的路徑：Switch 不算 hop，Router 之間形成 hop。
- Unit01 的 [[Bit and Byte]] 支撐 Unit02 的 Layer 1：bits 透過 copper、fiber 或 wireless medium 傳送。
- Unit02 把 Unit01 的設備與線材知識放進更高層的通訊模型，形成「設備/媒介 → layer function → end-to-end communication」的理解鏈。

## Cross-Unit Relationships

- [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]] 提供實體媒介、設備角色與基礎 network vocabulary。
- Unit02 將 Unit01 的概念組織進 [[TCP-IP Model]]：Layer 1 對應線材/訊號，Layer 2/3 對應 Switch/Router 與 hop/end-to-end delivery。

## Important Commands / Examples

本 Unit 沒有重點 CLI command。重要例子：

- PC1 使用 HTTPS request web page from SRV1。
- PC1 到 SRV1 的訊息經過 PC1 → R1 → R2 → SRV1 三個 hops。
- Layer 2 destination MAC address 每 hop 改變；Layer 3 destination IP address 保持不變。
- HTTPS 使用 port 443；DNS、HTTP、HTTPS 使用不同 Layer 4 port numbers。

## Common Confusions

- OSI model 不等於 TCP/IP model；相似 layers 不完全等價。
- TCP/IP 五層模型的 Application Layer 常被叫 Layer 7，而不是 Layer 5。
- Layer 4 port number 不是設備上的 physical port。
- Switch path 不算 hop；Chapter 4 明確指出 message traveling through a switch does not count as a hop。
- Layer 2 解決 next-hop；Layer 3 解決 final destination；Layer 4 解決 destination application process。

## Questions

- 見 [[05_Questions/Unit02-TCPIP-網路模型 Questions|Unit02-TCPIP-網路模型 Questions]]。

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit02-TCPIP-網路模型 REVIEW|Unit02-TCPIP-網路模型 REVIEW]]
