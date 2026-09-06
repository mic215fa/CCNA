# Unit04：Router / Switch 介面、路由與封包生命週期

tags: #source-note #unit #acting-ccna #routing #interface #packet-life-cycle

## Sources

- [[00_Source/Chapter 8 - Router 與 Switch 介面|Chapter 8 - Router 與 Switch 介面]]
- [[00_Source/Chapter 9 - 路由基礎|Chapter 9 - 路由基礎]]
- [[00_Source/Chapter 10 - 封包的一生|Chapter 10 - 封包的一生]]

## Core Question

Router 與 Switch 的介面狀態、速度/雙工協商、routing table、static/default routes 與 ARP 如何共同決定一個 packet 是否能從來源主機跨越多個網段到達目的主機？

## Key Ideas

- [[Network Interface]] 是設備參與網路的入口；router interface 通常需要 Layer 3 [[IPv4 Address]]，switch port 多半用於 Layer 2 forwarding。
- [[Interface Description]]、[[Interface Speed]]、[[Duplex]] 與 [[Autonegotiation]] 是實務上設定、驗證與排錯介面的基礎。
- [[Collision Domain]] 與 [[CSMA-CD]] 說明 hub 時代為什麼需要 half duplex 與 collision handling；switch 讓每個 host 進入自己的 collision domain，使 full duplex 成為常態。
- [[Interface Error]]、[[Speed Mismatch]] 與 [[Duplex Mismatch]] 是 Layer 1 / Layer 2 問題在 Cisco IOS output 中常見的症狀來源。
- End host 送 packet 前會判斷目的 IP 是否在同一網段；不同網段時會把 frame 的 destination MAC 指向 [[Default Gateway]]。
- [[Router]] 收到 frame 後會 de-encapsulate，檢查 [[IPv4 Header]]，依 [[Routing Table]] 做 [[Route Selection]]，再重新 encapsulate 到下一段 link。
- [[Connected Route]] 與 [[Local Route]] 會因為 router interface 設定 IP 並啟用而出現在 routing table；遠端網路則需要 [[Static Route]]、[[Default Route]] 或後續 dynamic routing。
- [[Next Hop]] 與 [[Exit Interface]] 指出 packet 往哪個方向送；static route 可指定 next-hop、exit interface，或兩者。
- [[Packet Life Cycle]] 的核心是：Layer 3 source/destination IP 保持 end-to-end，Layer 2 source/destination MAC 每一 hop 都會改變。

## Core Concepts

- [[Network Interface]]
- [[Routing Table]]
- [[Route Selection]]
- [[Static Route]]
- [[Interface Description]]
- [[Interface Speed]]
- [[Duplex]]
- [[Autonegotiation]]
- [[Collision Domain]]
- [[CSMA-CD]]
- [[Interface Error]]
- [[Speed Mismatch]]
- [[Duplex Mismatch]]
- [[Default Gateway]]
- [[Connected Route]]
- [[Local Route]]
- [[Static Route#Recursive Static Route|Recursive Static Route]]
- [[Static Route#Fully Specified Static Route|Fully Specified Static Route]]
- [[Default Route]]
- [[Next Hop]]
- [[Exit Interface]]
- [[Proxy ARP]]
- [[Packet Life Cycle]]

## Reused Concepts

- [[Cisco IOS CLI]]
- [[Cisco IOS Command Mode]]
- [[Router]]
- [[Switch]]
- [[Ethernet Frame]]
- [[MAC Address]]
- [[IPv4 Address]]
- [[IPv4 Header]]
- [[Address Resolution Protocol]]
- [[Address Resolution Protocol#ARP Cache (ARP Table)|ARP Table]]
- [[Frame Forwarding]]
- [[Frame Flooding]]
- [[Encapsulation and De-encapsulation]]
- [[Time To Live]]
- [[Forwarding]]

## Important Definitions

- [[Network Interface]]：router 或 switch 連接到網路的邏輯/實體介面，例如 G0/0、G0/1、F0/1。
- [[Autonegotiation]]：Ethernet 兩端交換 speed / duplex capability，選擇雙方都支援的最佳組合。
- [[Routing Table]]：router 已知目的地與轉送指示的資料庫。
- [[Route Selection]]：router 對目的 IP 做 route lookup，通常選擇最 specific / longest prefix match 的 route。
- [[Static Route]]：由管理者手動設定的 route，用來告訴 router 如何前往特定遠端網路。
- [[Default Route]]：`0.0.0.0/0` route；當沒有更 specific route 時使用。
- [[Default Gateway]]：host 用來送往遠端網路的 next-hop router IP。
- [[Packet Life Cycle]]：packet 在端點、switch、router 之間逐 hop 被封裝、轉送、解封裝與重新封裝的過程。

## Why These Concepts Exist

### 從「設備角色」到「介面能不能工作」

Unit01/Unit03 已經建立 [[Router]]、[[Switch]] 與 [[Cisco IOS CLI]]。Unit04 進一步回答：設備存在還不夠，interface 必須正確設定、啟用、速度與雙工相容，才真的能收送 frames / packets。

### 從「知道 IPv4 address」到「知道下一步往哪裡送」

Unit03 建立 [[IPv4 Address]]、[[IPv4 Header]] 與 [[Address Resolution Protocol]]。Unit04 補上 routing decision：host 先判斷 same network vs remote network；router 再用 [[Routing Table]] 決定 next-hop 或 exit interface。

### 從「單一 packet」到「跨多個 hops 的生命週期」

Chapter 10 把前面概念串起來：同一個 IP packet 在每一段 link 都要重新放進新的 Ethernet frame。IP addresses 保持 end-to-end，MAC addresses 則隨每個 hop 的 next-hop 目標改變。

## Knowledge Progression

```text
[[Cisco IOS CLI]]
  ↓ configure / verify
[[Network Interface]]
  ↓ operational parameters
[[Interface Speed]] + [[Duplex]] + [[Autonegotiation]]
  ↓ errors when mismatched
[[Interface Error]] / [[Speed Mismatch]] / [[Duplex Mismatch]]
  ↓ Layer 3 forwarding needs
[[Default Gateway]] + [[Routing Table]]
  ↓ decision process
[[Route Selection]]
  ↓ manual reachability
[[Static Route]] / [[Default Route]]
  ↓ hop-by-hop forwarding
[[Packet Life Cycle]]
```

## Cause and Effect

- Interface 未啟用或 speed mismatch → line protocol / interface 可能 down → router/switch 無法在該 link 通訊。
- Hub 是 shared medium → 多台設備同時傳送會 collision → 需要 [[CSMA-CD]] 與 half-duplex 行為。
- Switch 每個 port 是獨立 collision domain → frames 可以被 store and forward → full duplex 成為常見模式。
- 一端手動設定、另一端 autonegotiation → 可能無法正確偵測 duplex → 造成 [[Duplex Mismatch]]。
- Host 目的 IP 不在本地 network → frame 送給 [[Default Gateway]] 的 MAC → router 接手 Layer 3 forwarding。
- Router 沒有 route → drop packet；有 route → 依 [[Next Hop]] / [[Exit Interface]] forward。
- 多條 route 都 match 目的 IP → [[Route Selection]] 選 longest prefix match。
- 沒有 specific route 但有 [[Default Route]] → 使用 default route 作最後出口。
- 每經過 router 一次 → Ethernet frame 被重建，IPv4 packet 的 Layer 3 destination IP 保持不變。

## Prerequisites

- [[Cisco IOS CLI]] before interface configuration and verification commands。
- [[Ethernet]]、[[Ethernet Frame]]、[[Switch]] before [[Duplex]]、[[Collision Domain]]、[[Autonegotiation]]。
- [[IPv4 Address]]、[[Network Portion and Host Portion]] before [[Default Gateway]] and routing decisions。
- [[Address Resolution Protocol]] before understanding how next-hop MAC addresses are learned。
- [[Encapsulation and De-encapsulation]] before [[Packet Life Cycle]]。

## Depends On

- [[Autonegotiation]] depends on both link partners exchanging compatible capabilities。
- [[Route Selection]] depends on [[Routing Table]] entries。
- [[Static Route]] depends on correct destination network, mask, and next-hop / exit-interface information。
- [[Default Route]] depends on the absence of a more specific match to become useful。
- [[Packet Life Cycle]] depends on [[Routing Table]], [[Address Resolution Protocol]], [[Ethernet Frame]], and [[IPv4 Header]] working together。

## Leads To

- Interface configuration and troubleshooting → later VLAN trunks, routed ports, EtherChannel, STP troubleshooting。
- [[Routing Table]] / [[Route Selection]] → dynamic routing, OSPF, administrative distance, metrics, longest prefix match。
- [[Static Route]] / [[Default Route]] → static routing labs, floating static routes, default internet edge design。
- [[Packet Life Cycle]] → end-to-end troubleshooting, ACL/NAT/firewall path reasoning, traceroute and routing loops。

## Relationships

重要關係已持久記錄於 [[04_Maps/Unit04-RouterSwitch-Packet 知識地圖|Unit04：Router/Switch/Packet 知識地圖]] 的 `Relationships Added`。

## Contrast

### Router interface vs Switch interface

- Router interface：常作為 Layer 3 boundary，需要 IPv4 address 才能連接不同 networks。
- Switch interface：在本 Unit 多數情境是 Layer 2 port，負責接收/轉送 Ethernet frames。

### [[Interface Speed]] vs [[Duplex]]

- Speed：link 傳輸速率，例如 10/100/1000 Mbps。
- Duplex：是否可同時傳送與接收；half duplex 不能同時雙向，full duplex 可以。

### [[Connected Route]] vs [[Local Route]]

- Connected Route：代表 router interface 所連接的整個 network。
- Local Route：代表 router interface 自己的 single IP address，通常是 /32。

### [[Static Route]] vs [[Default Route]]

- Static Route：指向特定 destination network。
- Default Route：`0.0.0.0/0`，當沒有更 specific route 時才使用。

### [[Next Hop]] vs [[Exit Interface]]

- Next Hop：下一台 router 的 IP address。
- Exit Interface：本 router 要送出 packet 的本地介面。

## Cross-Document Relationships

- Chapter 8 解決「link 是否正常」：interface description、speed、duplex、autonegotiation、errors。
- Chapter 9 解決「router 如何決定方向」：routing table、connected/local/static/default routes、route selection。
- Chapter 10 解決「packet 實際怎麼走」：host 使用 default gateway，routers 做 route lookup，並在每一 hop 重新封裝。
- Chapter 8 的 interface 狀態是 Chapter 9 routing 的前提：路由表中的 connected route 與實際轉送能力，都仰賴 interface up/up 與正確 Layer 1/2 條件。
- Chapter 9 的 static/default routes 是 Chapter 10 packet life-cycle 能跨越 R1/R2/R3 的前提。

## Cross-Unit Relationships

- Unit01 的 [[Router]] / [[Switch]] 從設備角色延伸成可設定、可驗證、會因 interface 狀態影響 forwarding 的設備。
- Unit02 的 [[Encapsulation and De-encapsulation]] 在 Unit04 被 Chapter 10 具體化：每一 hop 都重新建立 Layer 2 frame，但保留 Layer 3 destination IP。
- Unit03 的 [[Address Resolution Protocol]] 在 Unit04 擴展為 next-hop MAC resolution：host 找 default gateway MAC，router 找下一台 router 或目的 host MAC。
- Unit03 的 [[IPv4 Address]] / [[Prefix Length]] / [[Network Portion and Host Portion]] 是 Unit04 host 判斷 same network vs remote network 的前提。
- Unit03 的 [[MAC Address Table]] / [[Frame Forwarding]] 支撐 Chapter 10 中 SW1/SW2 對 ARP 與資料 frames 的轉送。

## Important Commands / Examples

```text
interface <interface>
description <text>
speed auto
speed 10 | 100 | 1000
duplex auto
duplex half | full
show interfaces
show ip interface brief
show ip route
show ip route connected
show ip route local
ip route <destination-network> <netmask> <next-hop>
ip route <destination-network> <netmask> <exit-interface>
ip route 0.0.0.0 0.0.0.0 <next-hop-or-exit-interface>
```

## Common Confusions

- Interface description 只幫人理解，不會改變 forwarding 行為。
- Speed mismatch 通常會讓 link 無法正常 up；duplex mismatch 可能 link 還是 up，但會造成 collisions / errors / poor performance。
- Autonegotiation 不是永遠萬能；一端手動、一端 auto 時，duplex 判斷尤其容易出問題。
- Router 不是看 destination MAC 來選路；它先確認 frame 是送給自己，再看 packet 的 destination IP 做 route lookup。
- Default gateway 是 host 的下一跳，不是所有網路的「目的地」。
- Default route 不是「最好的 route」，而是沒有更 specific route 時的 fallback。
- Static route 只設定單向可達不夠；雙向通訊通常還需要 return route。

## Questions

- 見 [[05_Questions/Unit4-RouterSwitch-Packet Questions|Unit4-RouterSwitch-Packet Questions]]。

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit4-RouterSwitch-Packet REVIEW|Unit4-RouterSwitch-Packet REVIEW]]
