# Inter-VLAN Routing

tags: #concept #acting-ccna #vlan #routing

> [!info] Concept role
> [[VLAN]] 的直接子概念：以 Layer 3 forwarding 恢復不同 VLAN 之間的受控通訊。

## Definition

Inter-VLAN Routing 是讓不同 VLAN / subnets 之間透過 Layer 3 device 通訊的機制。

## Why It Exists

VLAN 在 Layer 2 分隔 broadcast domains；不同 VLAN 的 hosts 不能直接互相 forward frames，因此需要 router 或 multilayer switch 進行 Layer 3 routing。

## Prerequisites

- [[VLAN]]
- [[Default Gateway]]
- [[Routing Table]]
- [[Subnetting]]

## Related Concepts

- [[12.4-Router on a Stick]]
- [[Subinterface]]
- [[12.4-Multilayer Switch]]
- [[Switch Virtual Interface]]
- [[Routed Port]]

## Mechanism

Host 將遠端 VLAN/subnet 的 packet 送給自己的 default gateway；Layer 3 device route packet，並把它重新封裝到目標 VLAN 的 Ethernet frame 中。

### Packet invariants

- 原始 IP packet 的 source/destination IP 通常維持不變。
- 每通過一個 Layer 3 hop，原本的 Ethernet frame 會被移除並為下一段 link 重新建立。
- Source/destination MAC address 會因 link 而改變。
- Router 或 multilayer switch 會遞減 IPv4 TTL，並查詢 routing table 決定出口。

## Implementation Choices

| 方法 | Layer 3 gateway | VLAN 接入方式 | 主要取捨 |
|---|---|---|---|
| Separate router interfaces | Router physical interfaces | 每個 VLAN 一條 access link | 直觀但耗用 ports/線材 |
| [[12.4-Router on a Stick]] | Router subinterfaces | 單一 802.1Q trunk | 節省 interfaces，但共用一條 uplink |
| [[12.4-Multilayer Switch]] | [[Switch Virtual Interface]] | Switch 內部 VLAN context | Campus LAN 常見，routing 不必繞行外接 router |

## Depends On

- 每個 VLAN 使用正確且不重疊的 IP subnet。
- Hosts 設定位於本 VLAN 的 [[Default Gateway]]。
- Gateway interface/subinterface/SVI 處於可用狀態。
- Layer 3 device 的 [[Routing Table]] 包含目的 subnet。
- 若使用 ROAS，switch-to-router [[Trunk Port]] 必須允許相關 VLAN。

## Troubleshooting Order

1. 驗證 host IP address、prefix 與 default gateway。
2. 驗證來源與目的 VLAN 的 access membership。
3. 驗證 trunk、802.1Q encapsulation 與 allowed VLANs。
4. 驗證 router subinterface 或 SVI 是否 up/up。
5. 驗證 routing table 與回程路徑。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-234_763_1419_184_220.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Figure 12.9 — Inter-VLAN routing using separate router interfaces for each VLAN/subnet.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Knowledge Maps

- [[04_Maps/VLAN Knowledge Map|VLAN Knowledge Map]]
- [[04_Maps/Inter-VLAN Routing Packet Flow|Inter-VLAN Routing Packet Flow]]
