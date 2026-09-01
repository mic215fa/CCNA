# Network Interface

tags: #concept #acting-ccna #interface

## Aliases

- Interface
- Router interface
- Switch interface
- 介面

## Definition

Network Interface 是 router 或 switch 連接網路的入口；Cisco IOS 中常以 G0/0、G0/1、F0/1 等名稱表示。

## Why It Exists

設備需要透過具體 interface 收送 frames / packets。沒有可用 interface，[[Router]] 與 [[Switch]] 即使存在也無法參與 forwarding。

## Prerequisites

- [[Router]]
- [[Switch]]
- [[Cisco IOS CLI]]

## Related Concepts

- [[Interface Description]]
- [[Interface Speed]]
- [[Duplex]]
- [[Autonegotiation]]
- [[IPv4 Address]]
- [[Connected Route]]

## Mechanism

Router interface 通常需要設定 [[IPv4 Address]] 並啟用，才能連接一個 Layer 3 network；switch interface 在本 Unit 多數作為 Layer 2 port，負責接收與轉送 Ethernet frames。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-141_868_1410_978_190.jpg)
*Source: [[00_Source/Chapter 8 - Router 與 Switch 介面]], Figure 8.1 — Interface configurations on R1 and SW1, including IP address, description, speed, and duplex settings.*

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]

## Unit05 Additions

- Unit05 refines [[Network Interface]] into several roles: [[Access Port]], [[Trunk Port]], [[Subinterface]], [[Switch Virtual Interface]], and [[Routed Port]].
- The same physical cabling can carry different logical meanings depending on switchport mode, VLAN membership, and Layer 3 configuration.

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
