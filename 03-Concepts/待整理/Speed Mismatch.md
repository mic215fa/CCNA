# Speed Mismatch

tags: #concept #acting-ccna #interface #troubleshooting

## Definition

Speed Mismatch 是 link 兩端 interface speed 設定不一致的狀態。

## Why It Exists

Ethernet link 兩端必須使用相容速度才能正常通訊；速度不一致可能導致 interface down/down 或無法互通。

## Related Concepts

- [[Interface Speed]]
- [[Autonegotiation]]
- [[Interface Error]]
- [[Network Interface]]

## Mechanism

若 R1 與 SW1 的連接兩端被設定成不同 speed，Chapter 8 範例顯示它們無法通訊，interface 會呈現 down/down。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-154_286_1243_352_358.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Figure 8.6 — Speed mismatch between a router and a switch causes their interfaces to be down/down.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
