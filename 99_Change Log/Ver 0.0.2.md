# Change Log

tags: #vault/changelog #acting-ccna

> [!info] 目前版本
> `0.0.2`

## 版本 0.0.2

### 2026-08-15

- 依使用者要求，將 Vault 版本號從 `0.0.1` 更新為 `0.0.2`。
- 建立 [[99_Change Log/Ver 0.0.2|Ver 0.0.2]] 作為目前版本的 Change Log。
- 保留 [[99_Change Log/Ver 0.0.1|Ver 0.0.1]] 作為版本 `0.0.1` 的歷史紀錄。
- 更新 [[98_System/Vault Rules|Vault Rules]] 裡的目前版本與 Change Log 連結，使其指向版本 `0.0.2`。
- 依使用者要求，按照 [[AGENTS|AGENTS.md]] 處理 [[01_Units/Unit01-設備-線材|Unit01-設備-線材]]，以 [[00_Source/Chapter 2 - 網路設備|Chapter 2 - 網路設備]] 與 [[00_Source/Chapter 3 - 線材、接頭與連接埠|Chapter 3 - 線材、接頭與連接埠]] 作為來源，建立 Unit-level Source Note、Concept Notes、Knowledge Maps 與 Questions。
- 本次新增 [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]]、20 篇 Concept Notes、2 張 Knowledge Maps、1 份 Questions；已確認新知識網路檔案中的 wikilinks 皆可對應到現有筆記。
- 依使用者要求更新 [[AGENTS|AGENTS.md]]：後續 Unit 處理時，`Relationships Added` 的內容必須寫入 `04-Maps` / 本 Vault 實際 `04_Maps` 的 Knowledge Map 檔案；`REVIEW` 的內容必須寫入 `06_review` 目錄，最終報告需回指對應 Map / Review 檔案。
- 建立 [[06_review/Unit01-設備-線材 REVIEW|Unit01-設備-線材 REVIEW]] 作為 Unit01 的集中 REVIEW 紀錄，並將既有 Source Note、Concept Notes、Maps、Questions 中的 REVIEW 區塊改為指向此集中 Review 檔。
- 更新 [[04_Maps/Unit01-設備與線材知識地圖|Unit01：設備與線材知識地圖]]，新增 `Relationships Added` 區塊，保存 Unit01 處理時新增的重要關係；已確認相關 wikilinks 皆可對應到現有筆記。
- 依使用者要求更新 [[AGENTS|AGENTS.md]]：`03-Concepts` 中的 Concept Notes 應盡可能加入有助理解的來源圖片，圖片需來自 `00_Source` 或其 `images` 子目錄，並在圖片下方加入底標，標註來源 Chapter 與原始 Figure / caption；避免加入沒有理解價值的裝飾性圖片。
- 依使用者要求，針對現有 `03-Concepts` 執行來源圖片規則；已為 20 篇 Concept Notes 中的 19 篇加入 `Source Figures` 區塊與來源底標，圖片皆來自 `00_Source/images`，並標註來源 Chapter 與 Figure。保留 [[03-Concepts/待整理/Network Standard|Network Standard]] 未加圖，因 Chapter 2 / Chapter 3 沒有直接說明該概念且具高理解價值的圖片。
- 已確認本次加入的 Concept 圖片路徑皆有效、每張圖皆有 `Source` / Chapter / Figure 底標，且相關 wikilinks 皆可對應到現有筆記。

### 2026-08-16

- 依使用者要求，按照 [[AGENTS|AGENTS.md]] 處理 [[01_Units/Unit02-TCPIP-網路模型|Unit02-TCPIP-網路模型]]，以 [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]] 作為來源，建立 Unit-level Source Note、Concept Notes、Knowledge Maps、Questions、Cross-document / Cross-unit relationships 與 REVIEW。
- 新增 [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]] 作為 Unit02 的 Source Note，整理 TCP/IP model、OSI model、五層功能、encapsulation / de-encapsulation、PDU、payload、adjacent-layer interaction 與 same-layer interaction。
- 新增 20 篇 Unit02 Concept Notes：[[Networking Model]]、[[Protocol]]、[[OSI Model]]、[[TCP-IP Model]]、[[Physical Layer]]、[[Data Link Layer]]、[[Network Layer]]、[[Transport Layer]]、[[Application Layer]]、[[Hop]]、[[MAC Address]]、[[IP Address]]、[[Port Number]]、[[Forwarding]]、[[Encapsulation and De-encapsulation]]、[[Header and Trailer]]、[[Protocol Data Unit]]、[[Payload]]、[[Adjacent-layer Interaction]]、[[Same-layer Interaction]]。
- 重用並更新 9 篇既有 Unit01 Concepts：[[Network Standard]]、[[Ethernet]]、[[Bit and Byte]]、[[UTP Cable]]、[[Fiber Optic Cable]]、[[Node]]、[[End Host]]、[[Switch]]、[[Router]]，補上它們與 Unit02 layer model 的關係。
- 新增 [[04_Maps/Unit02-TCPIP 網路模型知識地圖|Unit02：TCP/IP 網路模型知識地圖]]，並依 AGENTS.md 規則將 `Relationships Added` 持久記錄在 `04_Maps` 中。
- 新增 [[04_Maps/Encapsulation 與 PDU 地圖|Encapsulation 與 PDU 地圖]]，整理 encapsulation、de-encapsulation、header/trailer、PDU 與 payload 的關係，並加入 Chapter 4 來源圖片與底標。
- 新增 [[05_Questions/Unit02-TCPIP-網路模型 Questions|Unit02：TCP/IP 網路模型 Questions]]，包含 recall、conceptual 與 cross-unit questions。
- 新增 [[06_review/Unit02-TCPIP-網路模型 REVIEW|Unit02-TCPIP-網路模型 REVIEW]]，集中記錄 Switch/Hop、Ethernet、IP Address、Transport Layer、Application protocols、OSI/TCP-IP model equivalence 與 cross-unit relationships 的待複查事項。
- 更新 [[06_review/Unit01-設備-線材 REVIEW|Unit01-設備-線材 REVIEW]]，把 Unit02 已建立的 Unit01 → Unit02 cross-unit relationships 記錄回 Unit01 REVIEW。
- 已確認本次 Unit02 新增/更新範圍內圖片路徑皆有效，且相關 Obsidian wikilinks 皆可對應到現有筆記。

### 2026-08-17

- 依使用者要求，按照 [[AGENTS|AGENTS.md]] 處理 [[01_Units/Unit03-CLI-Eth-IPV4|Unit03-CLI-Eth-IPV4]]，以 [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]]、[[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]]、[[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]] 作為同一 logical Unit 處理。
- 新增 [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]，整理 Cisco IOS CLI、configuration files、Ethernet switching、ARP、ping、IPv4 header 與 IPv4 addressing 的 combined knowledge structure。
- 新增 49 篇 Unit03 Concept Notes，涵蓋 CLI / IOS modes / configuration files、Ethernet frame / MAC address table / switching behavior、ARP / ping / ICMP、IPv4 header / binary / prefix / netmask / address range / classful addressing / TTL / MTU 等核心概念。
- 重用並更新既有 Concepts：[[Switch]]、[[Router]]、[[LAN]]、[[Ethernet]]、[[MAC Address]]、[[IP Address]]、[[Data Link Layer]]、[[Network Layer]]、[[Forwarding]]、[[Bit and Byte]]，補上 Unit03 的來源、圖片與跨 Unit 關係。
- 新增 [[04_Maps/Unit03-CLI-Eth-IPV4 知識地圖|Unit03：CLI、Ethernet、IPv4 知識地圖]]，並依 AGENTS.md 規則將 `Relationships Added` 持久記錄在 `04_Maps` 中。
- 新增 [[04_Maps/Ethernet Switching 與 ARP 地圖|Ethernet Switching 與 ARP 地圖]]，整理 switch learning / forwarding / flooding 與 ARP request/reply 的流程。
- 新增 [[04_Maps/IPv4 Addressing 基礎地圖|IPv4 Addressing 基礎地圖]]，整理 IPv4 address、octet、dotted decimal、prefix length、netmask、network/broadcast/usable range 的關係。
- 新增 [[05_Questions/Unit03-CLI-Eth-IPV4 Questions|Unit03：CLI、Ethernet、IPv4 Questions]]，包含 CLI、Ethernet switching、ARP/ping、IPv4 addressing 與 cross-unit reasoning questions。
- 新增 [[06_review/Unit03-CLI-Eth-IPV4 REVIEW|Unit03-CLI-Eth-IPV4 REVIEW]]，集中記錄 IOS modes、password/secret、Ethernet frame fields、ARP scope、IPv4 header fields、IPv4 addressing vs subnetting 等後續需複查項目。
- 更新 [[06_review/Unit02-TCPIP-網路模型 REVIEW|Unit02-TCPIP-網路模型 REVIEW]]，標記 Switch/Hop、Ethernet、IP Address 三項已由 Unit03 部分補強。
- 已確認本次 Unit03 新增/更新範圍內圖片路徑皆有效，且相關 Obsidian wikilinks 皆可對應到現有筆記。
- 依使用者要求，更新 [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]]，保留英文原文並於每一個自然段落下方新增 `> [!translation] 逐句繁體中文翻譯` Obsidian callout；已確認自然段落皆有對應翻譯區塊。
### 2026-08-18

- 依使用者告知，Vault 已搬移到 iCloud Drive；Codex follow-up 已確認目前可讀取的新 Vault 路徑為 `/Users/lukechen/Library/Mobile Documents/com~apple~CloudDocs/Acting CCNA V1 CH1 - CH15`。
- 已確認新 iCloud Vault 中仍可讀取 [[AGENTS|AGENTS.md]]、[[98_System/Vault Rules|Vault Rules]]、[[99_Change Log/Ver 0.0.2|Ver 0.0.2]]、`01_Units`、`00_Source` 等核心目錄與檔案。
- 後續操作應以 iCloud Drive 內的新 Vault 路徑為準，避免回寫到舊的 Documents 路徑。
- 新增 [[98_System/Codex Startup Prompt|Codex Startup Prompt]]，提供 Windows / VS Code / 新 Codex session 可直接貼上的啟動提示，協助 Codex 讀取 AGENTS.md、Vault Rules 與目前 Change Log 後接續處理 Vault。
### 2026-08-21

- 依使用者告知，Vault 已從 iCloud Drive 搬回本機；Codex follow-up 已確認目前可讀取的新本機 Vault 路徑為 `/Users/lukechen/Documents/Acting CCNA V1 CH1 - CH15`。
- 已確認 iCloud Drive 原路徑 `/Users/lukechen/Library/Mobile Documents/com~apple~CloudDocs/Acting CCNA V1 CH1 - CH15` 目前未找到該 Vault copy。
- 已確認新本機 Vault 中仍可讀取 [[AGENTS|AGENTS.md]]、[[98_System/Vault Rules|Vault Rules]]、[[98_System/Codex Startup Prompt|Codex Startup Prompt]]、[[99_Change Log/Ver 0.0.2|Ver 0.0.2]]、`00_Source`、`01_Units`、`02_Source_Notes`、`03-Concepts`、`04_Maps`、`05_Questions`、`06_review` 等核心目錄與檔案。
- 後續操作應以本機新 Vault 路徑為準，避免回寫到舊的 iCloud Drive 路徑或最早的 Documents 舊路徑。
- 依使用者要求，開始處理 [[01_Units/Unit03-CLI-Eth-IPV4|Unit03-CLI-Eth-IPV4]] 所引用章節的逐句繁體中文翻譯。已完成 [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]] 與 [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]] 的每一自然段落 translation callout；並已處理 [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]] 前半段，新增 76 個逐句繁中翻譯區塊。
- 本次已執行缺漏掃描：Chapter 5 自然段落缺漏 `0`、Chapter 6 自然段落缺漏 `0`；Chapter 7 尚餘後半段自然段落待下一批補完。
- 依使用者要求，補完 [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]] 剩餘 65 個自然段落的逐句繁體中文翻譯；保留英文原文、圖片連結、表格與原有章節結構，並在自然段落下方新增 `> [!translation] 逐句繁體中文翻譯` Obsidian callout。
- 已重新驗證 Unit03 來源章節逐句翻譯覆蓋率：[[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5]] 缺漏 `0`、[[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6]] 缺漏 `0`、[[00_Source/Chapter 7 - IPv4 位址|Chapter 7]] 缺漏 `0`。

## 2026-08-22 - Unit04 Router/Switch/Packet 來源章節逐句翻譯

- 依使用者要求，處理 [[01_Units/Unit4-RouterSwitch-Packet|Unit4-RouterSwitch-Packet]] 所引用的 [[00_Source/Chapter 8 - Router 與 Switch 介面|Chapter 8 - Router 與 Switch 介面]]、[[00_Source/Chapter 9 - 路由基礎|Chapter 9 - 路由基礎]]、[[00_Source/Chapter 10 - 封包的一生|Chapter 10 - 封包的一生]]。
- 在每一自然段落下方新增 `> [!translation] 逐句繁體中文翻譯` Obsidian callout，保留英文原文、圖片連結、表格與原有章節結構。
- 完成缺漏驗證：Chapter 8 缺漏 `0`、Chapter 9 缺漏 `0`、Chapter 10 缺漏 `0`。

## 2026-08-23 - Unit04 RouterSwitch-Packet 知識網路處理

- 依 `AGENTS.md` 處理 [[01_Units/Unit4-RouterSwitch-Packet|Unit4-RouterSwitch-Packet]]，解析 [[00_Source/Chapter 8 - Router 與 Switch 介面|Chapter 8]]、[[00_Source/Chapter 9 - 路由基礎|Chapter 9]]、[[00_Source/Chapter 10 - 封包的一生|Chapter 10]] 作為一個 logical Unit。
- 建立 Unit-level Source Note：[[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]。
- 建立 Unit knowledge map：[[04_Maps/Unit04-RouterSwitch-Packet 知識地圖|Unit04：Router/Switch/Packet 知識地圖]]，並將 Relationships Added 持久記錄於 04_Maps。
- 建立 reasoning questions：[[05_Questions/Unit4-RouterSwitch-Packet Questions|Unit04：Router/Switch/Packet Questions]]。
- 建立集中 REVIEW：[[06_review/Unit4-RouterSwitch-Packet REVIEW|Unit04：Router/Switch/Packet REVIEW]]，記錄概念邊界、Proxy ARP、route selection depth 與週期性知識檢查提醒。
- 建立 Unit04 相關 Concepts，並更新既有 [[Router]]、[[Switch]]、[[Address Resolution Protocol]]、[[IPv4 Header]]、[[Time To Live]]、[[Frame Forwarding]]、[[Forwarding]]。

## 2026-08-30 - Unit05 SubVLAN 知識網路處理

- 依 `AGENTS.md` 處理 [[01_Units/Unit05-SubVLAN|Unit05-SubVLAN]]，解析 [[00_Source/Chapter 11 - IPv4 網路子網劃分|Chapter 11]] 與 [[00_Source/Chapter 12 - VLAN|Chapter 12]] 作為一個 logical Unit。
- 建立 Unit-level Source Note：[[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]。
- 建立 Unit knowledge map：[[04_Maps/Unit05-SubVLAN 知識地圖|Unit05：Subnetting 與 VLAN 知識地圖]]，並將 Relationships Added 持久記錄於 04_Maps。
- 建立 reasoning questions：[[05_Questions/Unit05-SubVLAN Questions|Unit05：Subnetting 與 VLAN Questions]]。
- 建立集中 REVIEW：[[06_review/Unit05-SubVLAN REVIEW|Unit05：Subnetting 與 VLAN REVIEW]]，記錄 subnetting calculation boundaries、VLAN/subnet 概念邊界、ISL 是否需獨立成 Concept、SVI operational requirements 與週期性知識檢查提醒。
- 建立 Unit05 相關 Concepts，並更新既有 [[IPv4 Address]]、[[Prefix Length]]、[[Netmask]]、[[Network Portion and Host Portion]]、[[Usable IPv4 Address Range]]、[[Broadcast Domain]]、[[Layer 2 Domain]]、[[Switch]]、[[Router]]、[[Routing Table]]、[[Default Gateway]]、[[Network Interface]]、[[Ethernet Frame]]。

## 2026-08-30 - Unit05 Source Chapters 逐句繁體中文翻譯

- 依使用者要求，處理 [[01_Units/Unit05-SubVLAN|Unit05-SubVLAN]] 所引用的 [[00_Source/Chapter 11 - IPv4 網路子網劃分|Chapter 11 - IPv4 網路子網劃分]] 與 [[00_Source/Chapter 12 - VLAN|Chapter 12 - VLAN]]。
- 在每一自然段落下方新增 `> [!translation] 逐句繁體中文翻譯` Obsidian callout，保留英文原文、圖片連結、表格、命令輸出與原有章節結構。
- 完成缺漏驗證：Chapter 11 translation callouts `88`、缺漏 `0`；Chapter 12 translation callouts `137`、缺漏 `0`。
- 外部批次翻譯未使用於正式處理；正式寫入內容由 Codex 於本機產生並寫入。

## 2026-08-30 - Chapter 12 Section 檔案拆分

- 依使用者要求，為 [[00_Source/Chapter 12 - VLAN|Chapter 12 - VLAN]] 建立同層 section 檔，方便在 Obsidian 中直接點選閱讀。
- 新增 [[00_Source/Chapter 12.1 - 為什麼需要 VLAN|Chapter 12.1 - 為什麼需要 VLAN]]、[[00_Source/Chapter 12.2 - 設定 VLAN 與 Access Ports|Chapter 12.2 - 設定 VLAN 與 Access Ports]]、[[00_Source/Chapter 12.3 - 使用 Trunk Ports 連接 Switches|Chapter 12.3 - 使用 Trunk Ports 連接 Switches]]、[[00_Source/Chapter 12.4 - Inter-VLAN Routing|Chapter 12.4 - Inter-VLAN Routing]]。
- 在 Chapter 12 原檔開頭新增 `Chapter 12 Section Index` callout；原完整章節內容保留不刪。
- Section 檔保留原文、圖片連結、命令輸出與既有逐句繁體中文翻譯 callout。
