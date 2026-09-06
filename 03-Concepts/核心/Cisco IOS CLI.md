# Cisco IOS CLI

tags: #concept #acting-ccna #cli

> [!abstract] Concept role
> Cisco IOS CLI 是把網路設計轉成設備狀態的操作入口；它串起存取路徑、命令模式、設定變更、驗證與持久化。

## Aliases

- IOS CLI
- Cisco command-line interface

## Definition

Cisco IOS CLI 是 Cisco router / switch 上用文字命令操作、設定與驗證設備的介面。命令能否執行、是否改變設定，取決於目前所在的 [[Cisco IOS Command Mode]]。

## Why It Exists

網路工程不只要知道協定如何運作，也必須能把意圖轉成設備設定、確認實際狀態，並留下可在重開機後保留的設定。CLI 提供精確、可重複且可驗證的控制介面。

## Concept Structure

```text
Cisco IOS CLI
├── 本機存取：Console Port → cable → Terminal Emulator
├── 操作狀態：Cisco IOS Command Mode
├── 命令探索：Context-sensitive Help
├── 權限保護：enable secret / enable password
└── 設定生命週期：Running Configuration → Startup Configuration
```

## Prerequisites

- [[Switch]] 或 [[Router]] 的基本角色

## Depends On

- 一條可用的管理路徑；初始設定時常為 [[Console Port]]、[[Rollover Cable]] 與 [[Terminal Emulator]]
- 正確辨識 [[Cisco IOS Command Mode]]

## Mechanism

```text
建立管理連線 → 由 prompt 判斷 mode → 輸入命令 → 用 show 驗證 → 必要時儲存
```

IOS 依目前 mode 解析命令。設定命令通常先立即改變 running configuration；「已生效」不等於「已儲存」。

## Context-sensitive Help

Context-sensitive Help 的輸出會隨目前 mode 與已輸入的字串改變：

- `?`：列出目前 mode 可用的命令。
- `command ?`：列出下一個可用 keyword 或參數。
- `partial-command ?`：列出可能的補全。
- `Tab`：候選唯一時自動補全。

若熟悉的命令沒有出現，先檢查 prompt；常見原因是所在 mode 不對，而非命令不存在。

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-086_347_1279_1008_347.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — Using `?` to view commands available in the current mode.*

## Privileged Access Protection

`enable password` 與 `enable secret` 都保護 privileged EXEC access，但不值得拆成兩個獨立核心概念：

| 設定 | 儲存方式與限制 | 同時存在時 |
|---|---|---|
| `enable password` | 舊式；未加密時會以 cleartext 出現在 configuration，`service password-encryption` 也只是弱加密 | 不會被用來驗證 |
| `enable secret` | 以 hash 形式儲存，較適合保護 privileged EXEC | 優先使用 |

這兩者保護的是升權動作，不代表完整的使用者身分、授權與稽核制度。

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-091_226_1267_480_316.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — `enable password` shown in cleartext in running-config.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-092_266_1431_788_221.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — `enable secret` stored as a hash in running-config.*

## Design Principles

- **先辨識狀態，再輸入命令**：prompt 是 mode 的直接證據。
- **設定與驗證成對進行**：命令被接受，不代表結果符合意圖。
- **套用與儲存分開判斷**：確認 running 與 operational state 後，再決定是否儲存。
- **不要把 console 視為資料埠**：它是管理路徑，不承載一般 LAN traffic。

## Failure Model and Troubleshooting

| 症狀 | 優先檢查 | 原因 |
|---|---|---|
| CLI 沒有輸出或無法輸入 | console port、cable、terminal session | 管理路徑尚未成立 |
| 命令無法使用 | prompt 與 `?` | 位於錯誤 mode 或語法不完整 |
| 命令成功但功能不通 | 對應 `show` 與介面／協定狀態 | configuration 與 operational state 不同 |
| reload 後變更消失 | 比較 running 與 startup | 變更已套用但未儲存 |
| `enable` 密碼與預期不同 | secret 與 password 是否並存 | `enable secret` 優先 |

## Important Commands

| 目的 | 命令 |
|---|---|
| 進入 privileged EXEC | `enable` |
| 進入 global configuration | `configure terminal` |
| 在 configuration mode 執行 EXEC 命令 | `do <command>` |
| 查看目前／開機設定 | `show running-config` / `show startup-config` |
| 儲存設定 | `copy running-config startup-config` |

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-078_556_976_491_352.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], Figure 5.2 — Windows Command Prompt CLI example, used to introduce text-based command interaction.*

## Knowledge Maps

- [[04_Maps/Cisco IOS CLI 與 Configuration 地圖|Cisco IOS CLI 與 Configuration 地圖]]
- [[04_Maps/Unit03-CLI-Eth-IPV4 知識地圖|Unit03：CLI、Ethernet、IPv4 知識地圖]]

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼 command 被接受，仍不能證明網路功能已正確？
- 為什麼 configuration 已生效，reload 後仍可能消失？
- prompt 與 `?` 分別提供什麼排錯證據？
