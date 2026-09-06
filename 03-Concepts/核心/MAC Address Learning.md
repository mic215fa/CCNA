# MAC Address Learning

tags: #concept #acting-ccna #switching #layer2

> [!info] Concept role
> [[Ethernet Switching]] 的核心子機制：從 source MAC 建立 Layer 2 可達狀態。

## Definition

MAC Address Learning 是 switch 透過收到 frame 的 source MAC address，自動建立 [[MAC Address Table]] entry 的過程。

## Why It Exists

Switch 必須知道各 MAC address 從哪個 port 可到達，才能有效地 forwarding，而不是一直 flooding。

## Related Concepts

- [[MAC Address Table]]
- [[MAC Address Table#Dynamic Entries and Aging|MAC Aging]]
- [[Ethernet Frame]]
- [[Frame Forwarding]]

## Mechanism

當 switch 在某個 port 收到 frame，就把 frame 的 source MAC address、ingress port 與 VLAN context 建立關聯：

1. Table 沒有該 MAC：建立 dynamic entry。
2. MAC 已在相同 port/VLAN：刷新 entry 的 aging timer。
3. MAC 出現在不同 port：更新可達位置；若持續快速移動，可能是 loop 或 topology 問題。

Switch 不會用 destination MAC 建立來源位置，因為 destination 欄位只說明 frame 想去何處，不能證明該設備位於 ingress port。

## Depends On

- [[Ethernet Frame]] 提供 source MAC field。
- [[MAC Address Table]] 保存學習結果。
- VLAN-aware switch 還必須把 entry 放進正確 [[VLAN]] forwarding context。

## Leads To

- 已學到 destination → [[Frame Forwarding#Known Unicast Decision|known-unicast forwarding]]。
- Entry 不存在或已 aging out → [[Frame Forwarding#Unknown Unicast Decision|unknown-unicast flooding]]。

## Failure Indicators

- 同一 MAC address 在不同 ports 間反覆出現（MAC flapping）。
- Dynamic table 長期為空，導致 unicast traffic 持續 flooding。
- Host 移動後 entry 未及時更新，traffic 暫時送往舊 port。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-102_700_1269_1225_238.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.3 — Switches build MAC address tables by examining source MAC addresses of received frames.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Knowledge Maps

- [[04_Maps/Ethernet Switching 與 ARP 地圖|Ethernet Switching 與 ARP 地圖]]
