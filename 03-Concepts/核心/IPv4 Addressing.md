# IPv4 Addressing

tags: #concept #acting-ccna #ipv4 #addressing #subnetting

## Definition

IPv4 Addressing 是使用 32-bit addresses、prefix boundary 與 subnet rules，描述 interface 位於哪個 IPv4 network、哪些 destinations 在本地，以及 address block 如何分配的完整機制。

## Why It Exists

Layer 2 address 只處理目前 link 的 delivery；跨 network 通訊需要階層式 Layer 3 address。IPv4 addressing 讓 host 判斷 local/remote destination、讓 router 聚合並比對 network prefixes，也讓管理者把有限 address space 分配給不同 LAN/WAN segments。

## Prerequisites

- [[IP Address]]
- [[Network Layer]]
- [[Binary Number System]]
- [[Bit and Byte]]

## Concept Structure

```text
IPv4 Addressing（核心）
├── IPv4 Address：32-bit interface address
│   ├── Representation：4 octets / dotted decimal
│   ├── Prefix Length ↔ Netmask
│   ├── Network Portion / Host Portion
│   ├── Network Address
│   ├── Broadcast Address
│   └── Usable Address Range
├── Subnetting：改變 prefix boundary，切分 address block
│   ├── Borrowing Bits
│   ├── Five Subnet Attributes
│   ├── Magic Number Method
│   ├── FLSM
│   └── VLSM
└── Historical Context
    └── IPv4 Address Class / Classful Network
```

## Representation

IPv4 address 是 32 bits。為方便閱讀，分成四個 8-bit octets；每個 octet 可表示 `0–255`，以 dot 分隔成 dotted-decimal notation。

```text
11000000.10101000.01100100.01100100
                  ↓
          192.168.100.100
```

Octet 是表示單位，不是 routing boundary。真正界定 network/host portions 的是 [[Prefix Length]] 或等價的 [[Netmask]]。

## Address Boundary

以 `192.168.10.37/24` 為例：

```text
Address       192.168.10.37
Prefix        /24
Network bits  first 24 bits
Host bits     last 8 bits
Network       192.168.10.0
Broadcast     192.168.10.255
Usable        192.168.10.1–192.168.10.254
```

## Local vs Remote Decision

Host 將自己的 prefix/netmask 分別套用到自身與 destination address：

- Network result 相同 → destination 視為 on-link，直接解析目的 host MAC。
- Network result 不同 → destination 視為 remote，將 frame 送給 [[Default Gateway]]。

這個判斷使用 destination IPv4 address 和 local prefix，不是先查看 destination MAC。

## Address Roles

| Address role | Host bits | Purpose | 一般 host 可否使用 |
|---|---|---|---|
| [[IPv4 Network Address]] | 全 0 | 識別整個 prefix/network | 否 |
| Ordinary host address | 混合值 | 識別 subnet 內 interface | 是 |
| [[IPv4 Broadcast Address]] | 全 1 | 對 subnet 執行 directed one-to-all addressing | 否 |

一般 IPv4 subnet 的 [[Usable IPv4 Address Range]] 位於 network address 與 broadcast address 之間；[[Point-to-Point Subnet|`/31` point-to-point]] 與 `/32` host route 是需要另外理解的例外情境。

## From Addressing to Subnetting

[[Subnetting]] 增加 prefix length，將原本部分 host bits 改作 subnet/network bits。每借 1 bit，subnet 數量加倍，而每個 subnet 的 address capacity 減半。

```text
Original prefix
  ↓ borrow host bits
Longer prefix
  ↓
More subnets × fewer addresses per subnet
```

## Design Invariants

- IPv4 address 必須與 prefix/netmask 一起解讀；單獨一個 address 無法確定 network boundary。
- Prefix 越長，network 越具體，單一 subnet 的 addresses 越少。
- 同 subnet 的 interface addresses 必須唯一。
- IPv4 subnet 與 [[VLAN]] 常採一對一設計，以對齊 Layer 3 與 Layer 2 boundaries。
- Address plan 還必須保留 gateway、infrastructure 與未來成長空間，不能只剛好滿足目前 host count。

## Failure Model

| 現象 | 優先檢查 |
|---|---|
| 同 LAN hosts 對彼此 local/remote 判斷不同 | Prefix/netmask 是否一致 |
| Address 看似正確但無法指派 | 是否為 network/broadcast/reserved address |
| Subnets overlap | Network boundary、block size 與 allocation order |
| Host 數不足 | Prefix 是否保留足夠 host bits |
| Route 選錯或無法聚合 | Address plan 是否連續、routing prefix 是否正確 |

## Verification

```text
show ip interface brief
show interfaces
show ip route
ipconfig /all
ip address
ping
```

驗證時不只看 address，必須同時核對 prefix/netmask、interface、default gateway、connected route 與 subnet overlap。

## Contrast

- IP address vs MAC address：IP 表達端到端 Layer 3 位置；MAC 表達目前 Ethernet link 的 next-hop delivery。
- Prefix length vs netmask：兩種表示相同 network/host boundary 的方法。
- Addressing vs subnetting：addressing 定義位址與邊界；subnetting 是重新規劃邊界以產生多個 networks。
- Classful vs classless：前者依 A/B/C 固定預設 boundary；後者明確攜帶任意 prefix length。

## Related Concepts

- [[IPv4 Address]]
- [[Prefix Length]]
- [[Netmask]]
- [[Network Portion and Host Portion]]
- [[IPv4 Network Address]]
- [[IPv4 Broadcast Address]]
- [[Usable IPv4 Address Range]]
- [[Subnetting]]
- [[FLSM]]
- [[VLSM]]
- [[Point-to-Point Subnet]]

## Knowledge Maps

- [[04_Maps/IPv4 Addressing 基礎地圖|IPv4 Addressing 與 Subnetting 地圖]]
- [[04_Maps/Unit05-SubVLAN 知識地圖|Subnetting 與 VLAN 知識地圖]]

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Questions

- 為什麼只知道 `192.168.10.37`，仍不能判斷另一個 address 是否在相同 subnet？
- Prefix 增加 1 bit 時，subnet 數量與單一 subnet capacity 為什麼呈反向變化？
- 為什麼 IPv4 subnet 與 VLAN 通常應對齊，但兩者仍是不同概念？
