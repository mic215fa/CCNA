# Netmask

tags: #concept #acting-ccna #ipv4 #subnetting

## Aliases

- Subnet Mask
- 子網路遮罩

## Definition

Netmask 是與 IPv4 address 配對的 32-bit 值，用 1 表示 network portion，用 0 表示 host portion。

## Why It Exists

設備需要知道 IPv4 address 哪部分代表 network，哪部分代表 host，才能判斷目標是否在本地 network 或遠端 network。

## Related Concepts

- [[IPv4 Address]]
- [[Prefix Length]]
- [[Network Portion and Host Portion]]
- [[IPv4 Network Address]]
- [[IPv4 Broadcast Address]]

## Mechanism

`255.255.255.0` 的 binary 是前 24 bits 為 1，因此等同 `/24` prefix length。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-123_261_1300_1144_318.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.7 — Network and host portions of an IPv4 address are identified by prefix length.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Unit05 Additions

- Unit05 uses netmask / prefix length as the operational boundary for [[Subnetting]].
- The subnet mask determines subnet size, usable address range, and the interesting octet used by [[Magic Number Method]].

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
