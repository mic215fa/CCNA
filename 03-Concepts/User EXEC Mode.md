# User EXEC Mode

tags: #concept #acting-ccna #cli

## Definition

User EXEC Mode 是 Cisco IOS CLI 的最低權限 EXEC mode，prompt 通常是 `hostname>`。

## Why It Exists

它允許使用者查看基本狀態，同時避免未授權者執行重啟、儲存、刪檔或設定變更等高風險操作。

## Related Concepts

- [[Cisco IOS Command Mode]]
- [[Privileged EXEC Mode]]
- [[Cisco IOS CLI]]

## Mechanism

- 可執行部分 show commands，例如 `show clock`。
- 使用 `enable` 進入 [[Privileged EXEC Mode]]。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

