# Subnetting

tags: #concept #acting-ccna #subnetting #ipv4

> [!info] Concept role
> [[IPv4 Addressing]] 的核心規劃機制：改變 prefix boundary，將一個 address block 分割成多個不重疊 subnets。

## Aliases

- IPv4 subnetting
- 子網劃分

## Definition

Subnetting 是把一個 IPv4 address block / network 切成多個較小 subnets 的方法。

## Why It Exists

單一大型 network 通常無法符合實務上的部門、地點、broadcast domain 或 routing boundary 需求；subnetting 讓管理者能把 address space 分配給多個較小 networks。

## Prerequisites

- [[IPv4 Address]]
- [[Prefix Length]]
- [[Netmask]]
- [[Network Portion and Host Portion]]
- [[Binary Number System]]

## Related Concepts

- [[FLSM]]
- [[VLSM]]
- [[Point-to-Point Subnet]]
- [[VLAN]]

## Mechanism

Subnetting 透過增加 prefix length、向 host portion 借 bits 形成新的 network/subnet portion。每多借 1 bit，subnets 數量加倍，但每個 subnet 的 address 數量減半。

## Borrowing Bits

若原 prefix 是 `/p`，新 prefix 是 `/q`：

- Borrowed bits：`q - p`
- 產生的 equal-size subnets：`2^(q-p)`
- 每個 subnet 的 total addresses：`2^(32-q)`
- 一般 subnet 的 usable hosts：`2^(32-q) - 2`

```text
More borrowed bits
  ├── more subnets
  └── fewer host addresses per subnet
```

## Five Subnet Attributes

每個 subnet 都應能推導：

1. Network address：host bits 全 0。
2. Broadcast address：host bits 全 1。
3. First usable：一般為 network + 1。
4. Last usable：一般為 broadcast - 1。
5. Maximum hosts：一般為 `2^host_bits - 2`。

`/31` point-to-point 與 `/32` 不應機械套用一般的 `-2` host formula。

## Magic Number Method

Magic number 是快速定位 subnet boundaries 的計算技巧：找到 subnet mask 的 interesting octet，以 `256 - mask_octet` 得到 block size，再列出該 octet 中的 network increments。

例如 `/26` 的 mask 是 `255.255.255.192`：

```text
256 - 192 = 64
Network boundaries: 0, 64, 128, 192
```

Magic number 是運算捷徑，不取代 binary network/host boundary 的原理。

## Planning Workflow

1. 確認 parent address block 與 prefix。
2. 列出每個 segment 的 host requirements 與成長空間。
3. 選擇 [[FLSM]] 或 [[VLSM]]。
4. VLSM 由最大需求開始配置。
5. 對每個 subnet 計算 five attributes。
6. 檢查 overlap、遺漏、capacity 與 route summarization potential。
7. 將 subnet 與 LAN、WAN link、VLAN 或 routed interface 對應。

## Failure Model

| 錯誤 | 結果 |
|---|---|
| Prefix 太短 | Subnet 數不足，broadcast domain 過大 |
| Prefix 太長 | Host capacity 不足 |
| Boundary 算錯 | Subnets overlap 或 address 落入錯誤 network |
| VLSM 未由大到小配置 | Address fragmentation、後續大 subnet 無法放入 |
| 忘記 network/broadcast roles | 指派不可用 address |

## Contrast

- [[FLSM]]：所有 subnets 使用相同 prefix，簡單但可能浪費 addresses。
- [[VLSM]]：依需求使用不同 prefixes，效率較高但規劃更複雜。
- Subnetting vs VLAN：subnetting 建立 Layer 3 boundaries；VLAN 建立 Layer 2 broadcast boundaries。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-193_807_1391_902_190.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.1 — 192.168.1.0/24 divided into smaller subnets as prefix length increases.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-194_569_1161_1361_348.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.2 — Borrowing host bits creates additional subnet bits and longer prefixes.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-196_636_1353_946_223.jpg)
*Source: [[00_Source/Chapter 11 - IPv4 網路子網劃分]], Figure 11.4 — Five attributes of the 192.168.1.64/26 subnet.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Questions

- 為什麼 prefix length 每增加 1 bit，subnet 數會加倍但每個 subnet 的大小會減半？
- Magic number 為什麼只是 binary subnet boundary 的捷徑，而不是另一套規則？
- VLSM 未先配置最大 subnet 時，為什麼可能造成後續 address fragmentation？

## Knowledge Maps

- [[04_Maps/IPv4 Addressing 基礎地圖|IPv4 Addressing 與 Subnetting 地圖]]
- [[04_Maps/Unit05-SubVLAN 知識地圖|Subnetting 與 VLAN 知識地圖]]
