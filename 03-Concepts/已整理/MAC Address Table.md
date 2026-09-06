# MAC Address Table

tags: #concept #acting-ccna #switching #layer2

> [!info] Concept role
> [[Ethernet Switching]] 的狀態子概念：保存 MAC address、VLAN 與 egress port 的關聯。

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
- [[Frame Forwarding]]
- [[Frame Flooding]]

## Mechanism

Switch 收到 frame 時查看 source MAC，將該 MAC 與收到 frame 的 port 寫入 table。之後收到目的 MAC 已知的 frame，就依 table 指定 port 轉送。

典型 entry 不只有 MAC address 與 port，還包含 VLAN 與 entry type。相同 MAC 若存在於不同 VLAN contexts，不能省略 VLAN 直接解讀 forwarding location。

## Dynamic Entries and Aging

Dynamic entry 由 [[MAC Address Learning]] 建立。若一段時間沒有再看到該 source MAC，switch 會讓 entry aging out，避免 host 移動或斷線後仍保留過期位置。Chapter 6 的範例使用約 5 分鐘作為 dynamic aging 時間；實際值需以平台設定為準。

Entry aging out 後，下一個以該 MAC 為 destination 的 unicast frame 會暫時成為 unknown unicast 並觸發 flooding；switch 再看到該 host 發出的 frame 後即可重新學習。

## Entry Types

- Dynamic：從 ingress frame 的 source MAC 自動學習，會 aging。
- Static：由管理者明確設定，不依一般 dynamic aging 移除。
- Secure/system entries：由安全功能或平台內部行為建立，生命週期依功能而異。

## Contrast With ARP Cache

| State | Key → value | 主要使用者 | 用途 |
|---|---|---|---|
| MAC address table | MAC + VLAN → switch port | Switch | Layer 2 egress decision |
| [[Address Resolution Protocol#ARP Cache (ARP Table)|ARP cache]] | IPv4 → MAC | Host／router | 建立 next-hop Ethernet frame |

## Verification

- `show mac address-table`：查看所有可見 entries。
- `show mac address-table dynamic`：聚焦自動學習結果。
- 排錯時同時核對 MAC、VLAN、port 與 entry type，不能只搜尋 MAC 是否出現。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-102_700_1269_1225_238.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.3 — SW1 and SW2 learn host MAC addresses and associate each MAC with a switch port.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼 switch 要學 source MAC，但轉送 frame 時卻查 destination MAC？
- 為什麼 MAC address table 與 ARP cache 不能互相取代？
