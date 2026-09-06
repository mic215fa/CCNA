# Unit03：CLI、Ethernet Switching 與 IPv4

tags: #source-note #unit #acting-ccna #cli #ethernet #ipv4

## Sources

- [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]]
- [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]]
- [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]]

## Core Question

如何從「會操作 Cisco IOS CLI」開始，理解 switch 如何在 LAN 內轉送 Ethernet frames，並進一步理解 IPv4 address 如何讓 router 在 Layer 3 進行跨 LAN 傳遞？

## Key Ideas

- [[Cisco IOS CLI]] 是把網路理論變成設備設定與驗證的操作入口；CCNA 不只要求理解，也常要求 configure and verify。
- [[Cisco IOS Command Mode]] 把可執行的命令依權限與用途分層：user EXEC、privileged EXEC、global configuration mode，以及後續會遇到的子設定模式。
- [[IOS Configuration File]] 解釋為什麼設定後還要儲存：running-config 立即生效但重開會消失，startup-config 才會在開機時載入。
- [[Ethernet Frame]] 是 Layer 2 PDU；[[Switch]] 透過 frame 的 destination/source [[MAC Address]] 進行 switching。
- [[MAC Address Table]] 是 switch forwarding 的核心資料結構；[[MAC Address Learning]] 使用 source MAC 建表，[[Frame Forwarding]] / [[Frame Flooding]] 則依 destination MAC 是否已知做決策。
- [[Address Resolution Protocol]] 是 Layer 2 與 Layer 3 的橋：用已知 [[IPv4 Address]] 找出未知 [[MAC Address]]。
- [[IPv4 Header]] 承載 Layer 3 delivery 所需資訊；其中 source/destination IPv4 address 決定 end-to-end 目的地。
- [[IPv4 Addressing]] 串連 32-bit [[IPv4 Address]]、表示法、network/host boundary 與 local/remote 判斷；[[Prefix Length]] 與 [[Netmask]] 用來界定這兩個部分。
- [[Router]] 需要在不同 LAN 的 interface 上設定對應 IPv4 address，才能連接這些網路。

## Core Concepts

- [[Ethernet Switching]]
- [[IPv4 Addressing]]

- [[Cisco IOS CLI]]
- [[Console Port]]
- [[Rollover Cable]]
- [[Terminal Emulator]]
- [[Cisco IOS Command Mode]]
- [[IOS Configuration File]]
- [[Ethernet Frame]]
- [[Preamble and SFD]]
- [[EtherType]]
- [[Frame Check Sequence]]
- [[MAC Address Table]]
- [[MAC Address Learning]]
- [[MAC Address Table#Dynamic Entries and Aging|MAC Aging]]
- [[Frame Forwarding]]
- [[Frame Flooding]]
- [[Unicast Frame]]
- [[Broadcast Frame]]
- [[Layer 2 Domain]]
- [[Broadcast Domain]]
- [[Address Resolution Protocol]]
- [[Address Resolution Protocol#ARP Cache (ARP Table)|ARP Table]]
- [[Ping]]
- [[ICMP]]
- [[IPv4 Address]]
- [[IPv4 Header]]
- [[Binary Number System]]
- [[IPv4 Addressing#Representation|Dotted Decimal Notation]]
- [[IPv4 Addressing#Representation|Octet]]
- [[Prefix Length]]
- [[Netmask]]
- [[Network Portion and Host Portion]]
- [[IPv4 Network Address]]
- [[IPv4 Broadcast Address]]
- [[Usable IPv4 Address Range]]
- [[IPv4 Address Class]]
- [[Classful Network]]
- [[Packet Fragmentation]]
- [[Maximum Transmission Unit]]
- [[Time To Live]]

## Reused Concepts

- [[Switch]]
- [[Router]]
- [[LAN]]
- [[Ethernet]]
- [[Data Link Layer]]
- [[Network Layer]]
- [[MAC Address]]
- [[IP Address]]
- [[Forwarding]]
- [[Header and Trailer]]
- [[Protocol Data Unit]]
- [[Payload]]
- [[Bit and Byte]]

## Important Definitions

- [[Cisco IOS CLI]]：Cisco router / switch 上用文字命令操作設備的介面。
- [[Cisco IOS Command Mode]]：Cisco IOS 依權限與設定範圍分出的命令層級。
- [[Ethernet Frame]]：Layer 2 PDU，包含 Ethernet header、trailer 與 payload。
- [[MAC Address Table]]：switch 用來記錄 MAC address 對應到哪個 port 的表。
- [[Address Resolution Protocol]]：用 IP address 查詢 MAC address 的協定。
- [[IPv4 Address]]：32-bit Layer 3 address，通常以 dotted decimal notation 表示。
- [[Netmask]]：與 IPv4 address 配對，用 1/0 指出 network portion 與 host portion。
- [[Time To Live]]：IPv4 header 欄位，每經過一台 router 減 1，用來避免封包無限迴圈。

## Why These Concepts Exist

### 從理論到操作

Unit01 / Unit02 先建立設備、線材與 TCP/IP layers。Unit03 開始進入「怎麼真的設定與驗證設備」：[[Cisco IOS CLI]] 讓你能進入 [[Router]] / [[Switch]]，查看狀態、修改設定、儲存設定，並用 show / ping 等命令驗證結果。

### 從 Layer 2 到 Layer 3

Chapter 6 延伸 Unit02 的 [[Data Link Layer]]：switch 不是隨便把 frame 丟出去，而是依 [[MAC Address Table]] 判斷要 [[Frame Forwarding]] 還是 [[Frame Flooding]]。Chapter 7 再往上到 [[Network Layer]]：router 使用 [[IPv4 Address]] 與 [[IPv4 Header]] 連接不同 LAN。

### 從「知道 IP」到「知道 MAC」

要把 packet 放進 Ethernet frame，主機必須知道 next-hop 的 MAC address。[[Address Resolution Protocol]] 因此成為 [[IPv4 Address]] 與 [[MAC Address]] 之間的必要橋梁。

## Knowledge Progression

```text
[[Cisco IOS CLI]]
  ↓ configure / verify
[[Switch]] and [[Router]]
  ↓ Layer 2 behavior
[[Ethernet Frame]]
  ↓ destination/source MAC
[[MAC Address Table]]
  ↓ known or unknown destination
[[Frame Forwarding]] / [[Frame Flooding]]
  ↓ L2-L3 bridge
[[Address Resolution Protocol]]
  ↓ Layer 3 addressing
[[IPv4 Address]] + [[IPv4 Header]]
  ↓ inter-LAN connectivity
[[Router]]
```

## Cause and Effect

- CLI must change device behavior → configuration modes exist → commands update [[IOS Configuration File#Running Configuration|Running Config]]。
- Running-config is stored in RAM → reload/power loss removes unsaved changes → copy to [[IOS Configuration File#Startup Configuration|Startup Config]]。
- Switch receives a frame → learns source MAC → updates [[MAC Address Table]]。
- Destination MAC is known → switch forwards out one port → [[Frame Forwarding]]。
- Destination MAC is unknown or broadcast → switch sends out all other ports → [[Frame Flooding]]。
- Host knows destination IP but not MAC → sends ARP request → learns MAC via ARP reply → stores mapping in [[Address Resolution Protocol#ARP Cache (ARP Table)|ARP Table]]。
- Packet might loop through routers → [[Time To Live]] decreases each hop → packet is dropped at 0。
- IPv4 network needs host addresses → reserve network/broadcast addresses → usable range excludes first and last address。

## Prerequisites

- [[Bit and Byte]] before [[Binary Number System]] and [[IPv4 Address]]。
- [[Data Link Layer]]、[[MAC Address]]、[[Ethernet]] before [[Ethernet Frame]] and [[MAC Address Table]]。
- [[Network Layer]]、[[IP Address]] before [[IPv4 Address]] and [[IPv4 Header]]。
- [[Switch]] before [[Frame Forwarding]] / [[Frame Flooding]]。
- [[Router]] before configuring IPv4 addresses on router interfaces。

## Depends On

- [[Frame Forwarding]] depends on [[MAC Address Table]]。
- [[MAC Address Table]] depends on [[MAC Address Learning]] and source MAC addresses in [[Ethernet Frame]]。
- [[Address Resolution Protocol]] depends on both [[IPv4 Address]] and [[MAC Address]]。
- [[IPv4 Address]] depends on [[Binary Number System]]、[[IPv4 Addressing#Representation|Octet]]、[[Prefix Length]] / [[Netmask]]。
- Router inter-LAN connectivity depends on correct IPv4 addressing on router interfaces。

## Leads To

- [[Cisco IOS CLI]] → later interface, VLAN, routing, ACL, SSH, and security configurations。
- [[Ethernet Frame]] / [[MAC Address Table]] → VLAN, trunking, STP, EtherChannel。
- [[Address Resolution Protocol]] → packet life-cycle and troubleshooting。
- [[IPv4 Address]] / [[Prefix Length]] / [[Netmask]] → subnetting in Chapter 11。
- [[IPv4 Header]] → routing, QoS, ACL matching, OSPF, fragmentation, IPv6 comparison。

## Relationships

Unit 整體關係記錄於 [[04_Maps/Unit03-CLI-Eth-IPV4 知識地圖|Unit03：CLI、Ethernet、IPv4 知識地圖]]；CLI modes、管理存取與 configuration lifecycle 的可重用關係另集中於 [[04_Maps/Cisco IOS CLI 與 Configuration 地圖|Cisco IOS CLI 與 Configuration 地圖]]。

## Contrast

### [[IOS Configuration File#Running Configuration|Running Config]] vs [[IOS Configuration File#Startup Configuration|Startup Config]]

- Running Config：目前正在運作的設定，存於 RAM，設定命令會立即修改它。
- Startup Config：開機時載入的設定，存於 NVRAM，需要手動儲存才會更新。

### [[Frame Forwarding]] vs [[Frame Flooding]]

- Forwarding：destination MAC 已在 MAC address table 中，switch 只從對應 port 送出。
- Flooding：destination MAC 未知或 frame 是 broadcast，switch 從收到該 frame 以外的 ports 送出。

### [[MAC Address]] vs [[IPv4 Address]]

- MAC Address：Layer 2，通常由製造商配置，用於 hop-to-hop。
- IPv4 Address：Layer 3，由網路管理者設定或分配，用於 end-to-end。

## Cross-Document Relationships

- Chapter 5 提供操作入口：CLI、modes、configuration files。
- Chapter 6 說明 switch 如何用 Ethernet frame / MAC address table 完成 LAN 內 Layer 2 forwarding。
- Chapter 7 說明 IPv4 address / IPv4 header 如何讓 router 在不同 LAN 之間進行 Layer 3 delivery。
- Chapter 6 的 ARP 依賴 Chapter 7 才深入說明的 IPv4 address 結構；這是 Unit03 最重要的跨文件橋接。

## Cross-Unit Relationships

- Unit01 的 [[Switch]] / [[Router]] 在 Unit03 變成可設定、可驗證的設備。
- Unit02 的 [[Data Link Layer]] 在 Unit03 具體化為 [[Ethernet Frame]]、[[MAC Address Table]]、[[Frame Forwarding]]、[[Frame Flooding]]。
- Unit02 的 [[Network Layer]] / [[IP Address]] 在 Unit03 具體化為 [[IPv4 Address]]、[[IPv4 Header]]、[[Prefix Length]]、[[Netmask]]。
- Unit02 的 encapsulation 觀念在 Unit03 被 Chapter 7 Figure 7.2 強化：packet 必須被封裝進 frame 才能送到 physical medium。

## Important Commands / Examples

```text
enable
configure terminal
hostname R1
show running-config
show startup-config
copy running-config startup-config
show mac address-table
clear mac address-table dynamic
ping <ip-address>
show ip interface brief
show ip interface <interface-name>
```

## Common Confusions

- Console port 不是用來傳一般網路流量；它是管理/設定用。
- `enable password` 與 `enable secret` 都保護 privileged EXEC，但 `enable secret` 優先且安全性較高。
- Switch 會學 source MAC，但 forwarding decision 主要看 destination MAC。
- Unknown unicast 與 broadcast 都會被 flooded，但原因不同。
- ARP request 是 broadcast；ARP reply 是 unicast。
- IP packet alone 無法直接上線傳送；必須封裝進 Layer 2 frame。
- Network address 與 broadcast address 不能指派給 host。

## Questions

- 見 [[05_Questions/Unit03-CLI-Eth-IPV4 Questions|Unit03-CLI-Eth-IPV4 Questions]]。

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit03-CLI-Eth-IPV4 REVIEW|Unit03-CLI-Eth-IPV4 REVIEW]]
