# 03-Concepts 整理狀態

這個目錄以「目前整理狀態」管理 Concept Notes。分類是互斥的：每份 Concept Note 只放在一個資料夾。

## 分類規則

### `核心/`

已確認能跨章節或跨 Unit 重複使用，而且其他多個概念會依賴的知識樞紐。核心筆記應至少包含：定義、存在原因、概念結構、機制、依賴、重要關係、常見混淆、排錯或驗證，以及來源追蹤。

### `已整理/`

已完成內容強化與關係整理，但定位為某核心概念的子概念。它仍具獨立機制或跨筆記引用價值，因此不合併刪除。

### `部分移轉/`

Concept 本身仍值得保留，但原筆記中的詳細流程、情境或大型比較已移到 `04_Maps`。Concept Note 負責解釋「它是什麼與如何運作」，Map 負責呈現完整流程。

### `待整理/`

尚未依整合標準逐批審核。此分類不表示內容錯誤或概念不重要；其中可能包含未來的核心概念、可保留子概念、應合併內容或應移往 Maps／Questions 的材料。

## 目前狀態

| 分類 | Concept Notes | 說明 |
|---|---:|---|
| 核心 | 22 | VLAN、Ethernet Switching、IPv4 Addressing、Routing、Cisco IOS CLI、STP／RSTP 六批核心 |
| 已整理 | 25 | 已強化並保留的子概念 |
| 部分移轉 | 2 | ROAS 與 Multilayer Switching，packet flow 已移至 Maps |
| 待整理 | 77 | 等待後續逐主題審核 |

`README.md` 是分類說明，不計入 Concept Note 數量。

## 已確認核心

### VLAN 群

- [[VLAN]]
- [[Trunk Port]]
- [[Inter-VLAN Routing]]
- [[13-Dynamic Trunking Protocol|Dynamic Trunking Protocol]]
- [[13-VLAN Trunking Protocol|VLAN Trunking Protocol]]

### Ethernet Switching／ARP 群

- [[Ethernet Switching]]
- [[MAC Address Learning]]
- [[Frame Forwarding]]
- [[Address Resolution Protocol]]

### IPv4 Addressing／Subnetting 群

- [[IPv4 Addressing]]
- [[IPv4 Address]]
- [[Subnetting]]

### Routing Table／Static Route 群

- [[Routing Table]]
- [[Route Selection]]
- [[Static Route]]

### Cisco IOS CLI／Configuration 群

- [[Cisco IOS CLI]]
- [[Cisco IOS Command Mode]]
- [[IOS Configuration File]]

保留為已整理子概念：[[Console Port]]、[[Rollover Cable]]、[[Terminal Emulator]]。Context-sensitive Help、三個主要 command modes、Running／Startup Config 與 Enable Password／Secret 已吸收到核心頁的 anchors，不再保留碎片化空殼。

### STP／RSTP 群

- [[Layer 2 Loop]]
- [[Spanning Tree Protocol]]
- [[Bridge Protocol Data Unit]]
- [[Rapid Spanning Tree Protocol]]

保留為已整理子概念：[[STP Protection Features]]。Root Bridge、port roles、port states、timers 與 RSTP link types 保留為核心頁中的 anchors，避免依名詞碎片化。

## 狀態轉移原則

```text
待整理
  ↓ 審核概念重用價值與內容邊界
  ├──→ 核心
  ├──→ 已整理
  ├──→ 部分移轉
  └──→ 內容被其他 Concept 完整吸收後移除原空殼
```

每次移動都應同步檢查：

1. Obsidian wikilinks 是否仍能解析。
2. Source Figures 的相對路徑是否正確。
3. 關係是否已持久記錄在 `04_Maps`。
4. 問題是否已收錄在 `05_Questions`。
5. 不確定的合併或分類是否寫入 `06_review`。
