# Address Resolution Protocol

tags: #concept #acting-ccna #arp #layer2 #layer3

> [!info] Concept role
> Ethernet/IP 交界的核心概念：把 next-hop IPv4 address 解析成建立 Ethernet frame 所需的 MAC address。

## Aliases

- ARP

## Definition

Address Resolution Protocol 是用已知 [[IPv4 Address]] 查詢對應 [[MAC Address]] 的協定。

## Why It Exists

主機要把 Layer 3 packet 封裝進 Ethernet frame 時，需要知道 next-hop 或目的主機的 MAC address；ARP 提供 IP-to-MAC mapping。

## Prerequisites

- [[IPv4 Address]]
- [[MAC Address]]
- [[Broadcast Frame]]
- [[Unicast Frame]]

## Related Concepts

- [[MAC Address Table]]
- [[Frame Flooding]]
- [[Data Link Layer]]
- [[Network Layer]]

## Mechanism

1. Sender 先判斷 next hop：same subnet 使用目的 host IP；remote subnet 使用 [[Default Gateway]] IP。
2. Sender 查詢本機 ARP cache。
3. 若沒有 mapping，送出 destination MAC 為 `ffff.ffff.ffff` 的 ARP request。
4. Switch 使用 [[Frame Flooding]] 把 request 限制在同一 VLAN/broadcast domain 內傳送。
5. 擁有 queried IP 的設備通常以 unicast ARP reply 回覆自己的 MAC。
6. Sender 將 mapping 寫入 ARP cache，再建立 data Ethernet frame。

## ARP Cache (ARP Table)

ARP cache 是 host/router 保存 IPv4 → MAC neighbor mapping 的狀態表，使設備不必在每次送 packet 前重新 broadcast ARP request。Dynamic mapping 具有生命週期，可能因 aging、介面狀態或手動操作被移除。

ARP cache 不告訴 switch 應從哪個 port 送出 frame；那是 [[MAC Address Table]] 的工作。

| State | Mapping | Purpose |
|---|---|---|
| ARP cache | IPv4 → MAC | 建立 next-hop Ethernet frame |
| MAC address table | MAC + VLAN → port | Switch 選擇 Layer 2 egress port |

## Same-Subnet vs Remote-Subnet Resolution

- Same subnet：ARP 最終 destination host 的 IPv4 address。
- Remote subnet：ARP local default gateway 的 IPv4 address；不會在本地 LAN broadcast 詢問遠端 host MAC。
- Router forwarding：router 可能在每一個 egress Ethernet segment 對 next-hop router 或 final host 執行新的 ARP resolution。

## Failure Model

| 現象 | 可能原因 |
|---|---|
| ARP request 重複但沒有 reply | 目標不在線、IP/VLAN 錯誤、broadcast path 中斷 |
| ARP mapping 指向錯誤 MAC | Stale entry、duplicate IP、spoofing |
| Same-LAN 正常但 remote network 不通 | Default gateway mapping、gateway/routing 或回程路徑，而非目的 host ARP |
| Router 對遠端 IP 直接 ARP | Route 只指定 exit interface，或涉及 [[Proxy ARP]] |

## Verification

- Host：`arp -a` 或平台對應的 neighbor-table command。
- Cisco IOS：`show ip arp`。
- 同時比對 IP、MAC、interface、age，並確認該 MAC 在 switch 的 MAC address table 中位於合理 VLAN/port。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-108_698_1401_183_225.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.6 — PC1 sends an ARP request for PC3's MAC address and receives an ARP reply.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼 ARP 可以被視為 Layer 2 與 Layer 3 之間的橋？
- 為什麼傳送到 remote subnet 時，host 查詢的是 default gateway MAC，而不是遠端 host MAC？
- ARP cache 有正確 mapping 時，為什麼 switch 仍需要自己的 MAC address table？

## Knowledge Maps

- [[04_Maps/Ethernet Switching 與 ARP 地圖|Ethernet Switching 與 ARP 地圖]]

## Unit04 Additions

- Unit04 extends ARP from same-LAN host lookup to next-hop MAC resolution.
- A host ARPs for its [[Default Gateway]] when sending to a remote network.
- A router may ARP for the [[Next Hop]] router or final destination host while forwarding a packet during [[Packet Life Cycle]].
- [[Proxy ARP]] can appear when a router answers an ARP request on behalf of another destination it knows how to reach.

## Additional Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
