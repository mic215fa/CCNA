# Autonegotiation

tags: #concept #acting-ccna #interface #ethernet

## Definition

Autonegotiation 是 Ethernet 兩端交換支援的 speed 與 duplex capabilities，並選擇雙方都支援的最佳組合的機制。

## Why It Exists

它降低手動設定錯誤的機會，讓不同設備可以自動建立合適的 link 參數。

## Prerequisites

- [[Interface Speed]]
- [[Duplex]]

## Related Concepts

- [[Speed Mismatch]]
- [[Duplex Mismatch]]
- [[Network Interface]]

## Mechanism

當兩端都使用 autonegotiation 時，通常會選擇共同支援的最高能力。若一端手動設定、另一端 auto，auto 端可能能偵測 speed，但 duplex 可能 fallback，造成 mismatch。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-151_472_1187_1039_318.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Figure 8.4 — Router and switch advertise speed and duplex capabilities and select the best common option.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-152_304_1243_535_358.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Figure 8.5 — Only one side uses autonegotiation, creating a potential duplex mismatch scenario.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]

## Questions

- 為什麼「一端 auto、一端手動」比「兩端 auto」更容易出現 duplex mismatch？
