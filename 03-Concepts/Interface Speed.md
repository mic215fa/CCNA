# Interface Speed

tags: #concept #acting-ccna #interface #ethernet

## Definition

Interface Speed 是 Ethernet interface 使用的傳輸速率，例如 10 Mbps、100 Mbps、1000 Mbps。

## Why It Exists

連線兩端必須使用相容速度才能建立 link；速度設定錯誤會導致通訊失敗或 interface down。

## Related Concepts

- [[Network Interface]]
- [[Autonegotiation]]
- [[Speed Mismatch]]
- [[Duplex]]

## Mechanism

Interface 可手動設定 speed，也可使用 `speed auto` 透過 [[Autonegotiation]] 與對端協商。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-145_394_1302_820_316.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Section 8.1.2 — Speed settings and default auto behavior on switch interfaces.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
