# Rollover Cable

tags: #concept #acting-ccna #cabling #cli

> [!info] Concept role
> Rollover Cable 是 RJ45 console access chain 的專用實體媒介；其 pinout 與用途都不同於 Ethernet patch cable。

## Aliases

- Console Cable
- 反轉線

## Definition

Rollover Cable 是用來連接 PC 與 RJ45 console port 的 console cable，其 pin 對應完全反轉：1↔8、2↔7、3↔6、4↔5。

## Why It Exists

它讓電腦可以透過 RJ45 console port 直接存取 Cisco device CLI，進行本機管理與初始設定。

## Related Concepts

- [[Console Port]]
- [[Cisco IOS CLI]]
- [[Straight-through and Crossover Cable]]

## Contrast With

- [[Straight-through and Crossover Cable]]：用於 Ethernet network traffic。
- Rollover Cable：用於 console 管理，不用於一般 Ethernet LAN 傳輸。

## Dependencies and Limitations

- 一端必須對應設備的 RJ45 [[Console Port]]；PC 端可能還需要適合的 serial adapter。
- 它只建立實體 console 路徑；仍需 [[Terminal Emulator]] 才能互動。
- 外觀類似 RJ45 Ethernet cable，不代表能互換使用。

## Troubleshooting

- 先確認插入的是 console port，而非 Ethernet port。
- 若 terminal 找不到 serial interface，檢查 adapter、driver 與作業系統裝置名稱。
- 若連線仍無輸出，以已知良好的 cable／adapter 交叉測試，區分媒介與軟體設定問題。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-080_316_759_266_360.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], Figure 5.4 — Rollover cable wiring for connecting a PC to an RJ45 console port.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
