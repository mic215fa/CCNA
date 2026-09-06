# Cisco IOS Command Mode

tags: #concept #acting-ccna #cli

> [!abstract] Concept role
> Command Mode 是 IOS CLI 的狀態機：prompt 顯示目前狀態，而狀態決定可用命令與影響範圍。

## Definition

Cisco IOS Command Mode 是 IOS CLI 的命令層級系統；不同 mode 允許不同範圍的查詢、操作與設定。

## Why It Exists

Mode hierarchy 將低權限查詢、高權限操作與設定變更分開，並讓設定命令能依功能進入 interface、line 或 routing 等子模式。

## Prerequisites

- [[Cisco IOS CLI]]

## Mode State Machine

```text
hostname>  User EXEC
    │ enable
    ▼
hostname#  Privileged EXEC
    │ configure terminal
    ▼
hostname(config)#  Global Configuration
    ├── interface ... → hostname(config-if)#
    ├── line ...      → hostname(config-line)#
    └── router ...    → hostname(config-router)#
```

## User EXEC Mode

- Prompt：`hostname>`
- 角色：最低權限 EXEC state，提供有限的基本查詢。
- 典型命令：部分 `show`，例如 `show clock`。
- 轉移：`enable` 進入 [[#Privileged EXEC Mode|Privileged EXEC Mode]]。
- 限制：不能進入完整設定流程，也不提供所有 operational commands。

## Privileged EXEC Mode

- Prompt：`hostname#`
- 角色：完整查詢與高權限操作的樞紐，也是進入 configuration modes 的入口。
- 典型動作：完整 `show`、檔案操作、儲存設定與 `reload`。
- 轉移：`configure terminal` 進入 [[#Global Configuration Mode|Global Configuration Mode]]；`disable` 回到 [[#User EXEC Mode|User EXEC Mode]]。
- 保護：可使用 [[Cisco IOS CLI#Privileged Access Protection|enable secret]] 控制升權。

## Global Configuration Mode

- Prompt：`hostname(config)#`
- 角色：修改全域設定，並進入更細的子設定模式。
- 效果：設定命令通常立即修改 [[IOS Configuration File#Running Configuration|Running Config]]。
- 導航：`exit` 返回上一層；`end`、Ctrl-Z 回 privileged EXEC；可用 `do` 執行 EXEC command，例如 `do show clock`。

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-083_285_1229_1583_316.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — Entering global configuration mode with `configure terminal`.*

## Subconfiguration Modes

子模式是同一狀態機中作用範圍更窄的狀態。遇到新子模式時，應先辨識：從何處進入、prompt 如何改變、命令作用對象，以及 `exit` 返回哪一層。

## Navigation and Verification

| 目的 | 命令或證據 |
|---|---|
| 判斷目前 mode | prompt 的 `>`、`#`、`(config...)#` |
| 進入／離開高權限 EXEC | `enable` / `disable` |
| 進入 global configuration | `configure terminal` |
| 回上一層／回 privileged EXEC | `exit` / `end` |
| 查目前可用命令 | `?` |

## Failure Model

- **Invalid input**：先確認 mode，再檢查拼字與語法。
- **命令作用在錯誤對象**：檢查 prompt 的子模式及先前選取的 interface／line／process。
- **離開層級不如預期**：區分 `exit`（上一層）與 `end`（回 privileged EXEC）。
- **show 在設定模式不可用**：回 privileged EXEC，或在支援時用 `do show ...`。

## Contrast

| 類型 | 主要目的 | 是否通常修改設定 |
|---|---|---|
| EXEC modes | 查詢與操作設備 | 否 |
| Configuration modes | 建立或變更設定 | 是，修改 running configuration |

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-084_257_1208_1751_350.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], Figure 5.6 — Navigation between user EXEC, privileged EXEC, and global configuration mode.*

## Knowledge Maps

- [[04_Maps/Cisco IOS CLI 與 Configuration 地圖|Cisco IOS CLI 與 Configuration 地圖]]

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼 prompt 是排查 CLI command error 的第一個證據？
- `exit` 與 `end` 對 mode state 的影響有何不同？
