# Context-sensitive Help

tags: #concept #acting-ccna #cli

## Definition

Context-sensitive Help 是 Cisco IOS CLI 中用 `?` 與 Tab 輔助查詢、補全與理解目前 mode 可用命令的功能。

## Why It Exists

Cisco IOS 命令很多，help / completion 讓使用者在練習與排錯時更容易找到正確 command 或 keyword。

## Related Concepts

- [[Cisco IOS CLI]]
- [[Cisco IOS Command Mode]]

## Mechanism

- `?`：列出目前 mode 可用命令。
- `command ?`：列出 command 後可接的 keywords。
- `partial-command ?`：列出可能補全。
- `Tab`：若只有一個可能選項，自動補全。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-086_347_1279_1008_347.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — Using `?` to view commands available in the current mode.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

