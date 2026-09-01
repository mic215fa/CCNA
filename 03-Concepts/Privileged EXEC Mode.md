# Privileged EXEC Mode

tags: #concept #acting-ccna #cli

## Definition

Privileged EXEC Mode 是 Cisco IOS CLI 的高權限 EXEC mode，prompt 通常是 `hostname#`。

## Why It Exists

它提供完整 show commands 與 operational commands，例如 reload、save configuration、檔案操作，以及進入設定模式。

## Related Concepts

- [[User EXEC Mode]]
- [[Global Configuration Mode]]
- [[Enable Password]]
- [[Enable Secret]]
- [[Running Config]]
- [[Startup Config]]

## Mechanism

- 從 user EXEC 使用 `enable` 進入。
- 使用 `configure terminal` 進入 [[Global Configuration Mode]]。
- 使用 `disable` 回到 [[User EXEC Mode]]。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

