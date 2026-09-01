# Subinterface

tags: #concept #acting-ccna #interface #routing #vlan

## Definition

Subinterface 是建立在 router physical interface 之上的 virtual interface，例如 `G0/0.10`。

## Why It Exists

在 ROAS 中，一個 router physical interface 需要代表多個 VLAN/subnets；subinterfaces 讓每個 VLAN 都有自己的 Layer 3 gateway interface。

## Prerequisites

- [[Router on a Stick]]
- [[IEEE 802.1Q Tag]]
- [[VLAN ID]]
- [[IPv4 Address]]

## Related Concepts

- [[Network Interface]]
- [[Default Gateway]]
- [[Native VLAN]]

## Mechanism

Subinterface 使用 `encapsulation dot1q <vlan-id>` 綁定 VLAN，並設定該 VLAN/subnet 的 IP address。Subinterface 編號不一定要等於 VLAN ID；真正決定 VLAN 對應的是 encapsulation command。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-236_204_1050_1345_346.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Configuring ROAS — Router subinterfaces are created under a physical interface.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-236_190_1237_1918_346.jpg)
*Source: [[00_Source/Chapter 12 - VLAN]], Configuring ROAS — Subinterface VLAN ID and IP address configuration.*

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
