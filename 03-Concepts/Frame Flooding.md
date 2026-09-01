# Frame Flooding

tags: #concept #acting-ccna #switching #layer2

## Definition

Frame Flooding 是 switch 將 frame 從收到該 frame 以外的所有 ports 送出的動作。

## Why It Exists

當 destination MAC unknown，switch 不知道目標在哪裡，只能 flood 讓 frame 有機會到達；broadcast frames 也必須送給同一 broadcast domain 的所有 hosts。

## Related Concepts

- [[Unknown Unicast Frame]]
- [[Broadcast Frame]]
- [[MAC Address Table]]
- [[Frame Forwarding]]

## Mechanism

Switch 在收到 unknown unicast 或 broadcast frame 時 flood；收到該 frame 的 port 不會再被送回，避免立即反送。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-104_717_1402_181_236.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.4 — Unknown unicast frame is flooded because switches have empty MAC address tables.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

