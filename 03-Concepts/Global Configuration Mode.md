# Global Configuration Mode

tags: #concept #acting-ccna #cli #configuration

## Definition

Global Configuration Mode 是 Cisco IOS 中用來修改設備設定的主要 configuration mode，prompt 通常是 `hostname(config)#`。

## Why It Exists

EXEC modes 主要用於查看與操作；真正改變設備設定需要進入 configuration mode，避免設定命令與查詢命令混在一起。

## Related Concepts

- [[Cisco IOS Command Mode]]
- [[Privileged EXEC Mode]]
- [[Running Config]]
- [[Context-sensitive Help]]

## Mechanism

- 從 privileged EXEC 使用 `configure terminal` 進入。
- 設定命令會立即修改 [[Running Config]]。
- 可用 `end`、`exit`、Ctrl-C、Ctrl-Z 回到 privileged EXEC。
- 在設定模式中可用 `do` 執行 EXEC mode commands，例如 `do show clock`。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-083_285_1229_1583_316.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — Entering global configuration mode with `configure terminal`.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

