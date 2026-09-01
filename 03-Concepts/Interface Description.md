# Interface Description

tags: #concept #acting-ccna #interface

## Definition

Interface Description 是設定在 interface 上的文字標籤，用來描述或命名該介面，例如標示連到哪台設備。

## Why It Exists

它不改變轉送行為，但能讓管理者在查看設定或排錯時快速理解連線用途，降低誤判與操作錯誤。

## Related Concepts

- [[Network Interface]]
- [[Cisco IOS CLI]]

## Mechanism

在 interface configuration mode 中使用 `description <text>` 設定。Chapter 8 強調 description 是 optional，但實務上很有價值。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-142_240_1279_1671_348.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Section 8.1.1 — Interface descriptions used to identify connected devices.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
