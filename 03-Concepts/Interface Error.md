# Interface Error

tags: #concept #acting-ccna #interface #troubleshooting

## Definition

Interface Error 是 interface 統計輸出中顯示的錯誤或異常計數，例如 input errors、CRC、collisions、late collisions 等。

## Why It Exists

它們提供 Layer 1 / Layer 2 troubleshooting 線索，幫助判斷是否存在 speed mismatch、duplex mismatch、線材或實體媒介問題。

## Related Concepts

- [[Network Interface]]
- [[Speed Mismatch]]
- [[Duplex Mismatch]]
- [[Duplex]]

## Mechanism

使用 `show interfaces` 可看到 interface counters。錯誤類型與數量本身不是結論，而是排錯時的證據。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-152_423_1048_1670_347.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Section 8.3 — Interface output listing errors and statistics.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
