# Frame Forwarding

tags: #concept #acting-ccna #switching #layer2

## Definition

Frame Forwarding 是 switch 將 frame 從正確 port 送往 destination MAC address 所在方向的動作。

## Why It Exists

Forwarding 讓 switch 能把 LAN 內資料有效送到目標，而不必每次都讓所有 hosts 收到。

## Prerequisites

- [[MAC Address Table]]
- [[Ethernet Frame]]
- [[MAC Address]]

## Related Concepts

- [[Known Unicast Frame]]
- [[Frame Flooding]]
- [[Switch]]
- [[Forwarding]]

## Mechanism

若 destination MAC address 已存在於 [[MAC Address Table]]，switch 就把 frame 從 table 指定的 port 送出。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-105_883_1399_179_207.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.5 — PC3 replies to PC1, and switches forward the frame using learned MAC table entries.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Unit04 Additions

- Chapter 10 shows [[Frame Forwarding]] as the switch behavior that carries ARP requests/replies and data frames inside each LAN segment.
- Frame forwarding supports [[Packet Life Cycle]], but does not itself perform Layer 3 [[Route Selection]].

## Additional Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
