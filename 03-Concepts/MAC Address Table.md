# MAC Address Table

tags: #concept #acting-ccna #switching #layer2

## Aliases

- CAM Table
- MAC table

## Definition

MAC Address Table 是 switch 用來記錄「MAC address 在哪個 port 可到達」的表。

## Why It Exists

Switch 需要根據 destination MAC address 判斷 frame 應該從哪個 port 送出，而不是永遠送給所有人。

## Prerequisites

- [[MAC Address]]
- [[Switch]]
- [[Ethernet Frame]]

## Related Concepts

- [[MAC Address Learning]]
- [[MAC Aging]]
- [[Frame Forwarding]]
- [[Frame Flooding]]

## Mechanism

Switch 收到 frame 時查看 source MAC，將該 MAC 與收到 frame 的 port 寫入 table。之後收到目的 MAC 已知的 frame，就依 table 指定 port 轉送。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-102_700_1269_1225_238.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.3 — SW1 and SW2 learn host MAC addresses and associate each MAC with a switch port.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼 switch 要學 source MAC，但轉送 frame 時卻查 destination MAC？

