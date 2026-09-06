# Frame Flooding

tags: #concept #acting-ccna #switching #layer2

> [!info] Concept role
> [[Frame Forwarding]] 的多埠輸出動作，用於 unknown unicast 與 broadcast 等情況。

## Definition

Frame Flooding 是 switch 將 frame 從收到該 frame 以外的所有 ports 送出的動作。

## Why It Exists

當 destination MAC unknown，switch 不知道目標在哪裡，只能 flood 讓 frame 有機會到達；broadcast frames 也必須送給同一 broadcast domain 的所有 hosts。

## Related Concepts

- [[Frame Forwarding#Unknown Unicast Decision|Unknown Unicast]]
- [[Broadcast Frame]]
- [[MAC Address Table]]
- [[Frame Forwarding]]

## Mechanism

Switch 在收到 unknown unicast 或 broadcast frame 時 flood；收到該 frame 的 port 不會再被送回。Flooding 也受 VLAN membership、allowed VLAN、port forwarding state 等條件限制，而不是送到 switch 的每一個 physical port。

## Why It Is Temporary for Unknown Unicast

Unknown-unicast flooding 是資訊不足時的 fallback。只要目的 host 回覆，reply 的 source MAC 就會讓 switches 更新 [[MAC Address Table]]，後續 frames 通常改用單點 forwarding。Broadcast flooding 則是 frame 本身要求同一 broadcast domain 的所有節點接收，不會因學到 MAC 而消失。

## Failure Indicators

- 持續大量 unknown-unicast flooding：檢查 MAC learning、aging、單向 traffic 或 topology instability。
- Flooding 超出預期範圍：檢查 VLAN boundary、trunk allowed VLAN 與 Layer 2 loop prevention。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-104_717_1402_181_236.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.4 — Unknown unicast frame is flooded because switches have empty MAC address tables.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
