# Duplex Mismatch

tags: #concept #acting-ccna #interface #troubleshooting

## Definition

Duplex Mismatch 是 Ethernet link 兩端使用不同 duplex mode，例如一端 full duplex、另一端 half duplex。

## Why It Exists

Duplex mismatch 常造成效能差、collisions、late collisions 或 interface errors；它可能不像 speed mismatch 那樣直接讓 link down，因此更容易被忽略。

## Related Concepts

- [[Duplex]]
- [[Autonegotiation]]
- [[Interface Error]]
- [[CSMA-CD]]

## Mechanism

常見情境是一端手動設定 full duplex，另一端使用 autonegotiation 且未能正確協商 duplex，導致 auto 端 fallback 到 half duplex。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-152_304_1243_535_358.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Figure 8.5 — Manual speed/duplex on one side and autonegotiation on the other can create duplex mismatch.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
