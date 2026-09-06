# IOS Configuration File

tags: #concept #acting-ccna #cli #configuration

> [!abstract] Concept role
> IOS Configuration File 描述「現在生效」與「下次開機載入」兩種狀態，是理解設定套用、儲存與 reload 結果的核心模型。

## Definition

IOS Configuration File 是 Cisco IOS device 用來表達設備設定的文字內容。最重要的兩個狀態是 running configuration 與 startup configuration。

## Why It Exists

設備必須讓設定立即生效，同時允許操作者驗證後再決定是否跨 reload 保留。因此 IOS 將運作中設定與開機設定分開管理。

## Prerequisites

- [[Cisco IOS CLI]]
- [[Cisco IOS Command Mode#Global Configuration Mode|Global Configuration Mode]]

## Configuration Lifecycle

```text
configuration commands
  ↓ immediate change
Running Configuration (RAM, currently effective)
  │ copy running-config startup-config
  ▼
Startup Configuration (NVRAM, loaded at boot)
  │ reload / boot
  └──────────────────────────────→ new Running Configuration
```

## Running Configuration

- Alias：running-config、Running Config。
- 儲存位置：RAM。
- 意義：設備目前正在使用的設定。
- 行為：configuration command 通常立即改變它，也可能立即改變設備行為。
- 風險：未複製到 startup configuration 時，reload 或斷電後變更消失。
- 查看：`show running-config`。

## Startup Configuration

- Alias：startup-config、Startup Config。
- 儲存位置：NVRAM。
- 意義：設備開機時用來建立 running configuration 的設定。
- 行為：修改 running 不會自動同步到此處。
- 查看：`show startup-config`。
- 寫入：`copy running-config startup-config`；`write memory` 或 `write` 是常見短形式。

## Key Distinctions

| 問題 | Running Configuration | Startup Configuration |
|---|---|---|
| 代表什麼 | 現在生效的設定 | 下次開機要載入的設定 |
| 主要儲存位置 | RAM | NVRAM |
| 設定命令是否直接修改 | 通常是 | 否 |
| reload 後是否直接保留 | 未儲存的差異不保留 | 開機時被載入 |

## Apply Is Not Save

```text
命令被接受 ≠ 功能一定正確 ≠ 已經儲存
```

安全流程是「修改 → 驗證 running/operational state → 儲存 → 必要時再比較」。先儲存錯誤設定，錯誤也會跨 reload 保留；驗證正確卻忘記儲存，故障可能延後到 reload 才出現。

## Failure Model and Troubleshooting

| 症狀 | 可能原因 | 驗證方式 |
|---|---|---|
| 設定後有效，reload 後消失 | running 未存入 startup | 比較兩個 `show ...-config` |
| 修改後功能立即中斷 | running 已即時生效 | 檢查剛修改區段與 operational output |
| startup 正確但現在行為不同 | running 與 startup 已分歧 | 分別查看兩份設定 |
| 儲存後仍不符預期 | 設定錯誤或 prerequisite 缺失 | 驗證介面、協定與鄰接狀態 |

## Important Commands

| 目的 | 命令 |
|---|---|
| 查看目前生效設定 | `show running-config` |
| 查看開機設定 | `show startup-config` |
| 保存目前設定 | `copy running-config startup-config` |

## Related Concepts

- [[Cisco IOS Command Mode]]
- [[Cisco IOS CLI]]

## Knowledge Maps

- [[04_Maps/Cisco IOS CLI 與 Configuration 地圖|Cisco IOS CLI 與 Configuration 地圖]]

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼「設定已生效」與「設定已保存」必須分別驗證？
- 哪一種疏失會造成延遲到 reload 才出現的故障？
