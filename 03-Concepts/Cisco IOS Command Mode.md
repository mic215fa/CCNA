# Cisco IOS Command Mode

tags: #concept #acting-ccna #cli

## Definition

Cisco IOS Command Mode 是 IOS CLI 的命令層級系統；不同 mode 允許不同範圍的命令與設定。

## Why It Exists

Mode hierarchy 讓 IOS 可以區分低風險查詢、進階操作與實際設定變更，避免所有命令都在同一層級混雜。

## Related Concepts

- [[User EXEC Mode]]
- [[Privileged EXEC Mode]]
- [[Global Configuration Mode]]
- [[Cisco IOS CLI]]

## Mechanism

```text
hostname>
  user EXEC
    ↓ enable
hostname#
  privileged EXEC
    ↓ configure terminal
hostname(config)#
  global configuration
```

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-084_257_1208_1751_350.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], Figure 5.6 — Navigation between user EXEC, privileged EXEC, and global configuration mode.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

