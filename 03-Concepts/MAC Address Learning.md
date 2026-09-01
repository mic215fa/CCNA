# MAC Address Learning

tags: #concept #acting-ccna #switching #layer2

## Definition

MAC Address Learning 是 switch 透過收到 frame 的 source MAC address，自動建立 [[MAC Address Table]] entry 的過程。

## Why It Exists

Switch 必須知道各 MAC address 從哪個 port 可到達，才能有效地 forwarding，而不是一直 flooding。

## Related Concepts

- [[MAC Address Table]]
- [[MAC Aging]]
- [[Ethernet Frame]]
- [[Frame Forwarding]]

## Mechanism

當 switch 在某個 port 收到 frame，就把 frame 的 source MAC address 與該 ingress port 建立關聯。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-102_700_1269_1225_238.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.3 — Switches build MAC address tables by examining source MAC addresses of received frames.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

