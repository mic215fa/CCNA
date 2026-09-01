# Usable IPv4 Address Range

tags: #concept #acting-ccna #ipv4 #subnetting

## Definition

Usable IPv4 Address Range 是某個 IPv4 network 中可指派給 hosts 的 address 範圍，不包含 network address 與 broadcast address。

## Why It Exists

Network address 與 broadcast address 有特殊用途，不能給 host；網管需要知道真正可分配的 host addresses。

## Related Concepts

- [[IPv4 Network Address]]
- [[IPv4 Broadcast Address]]
- [[Prefix Length]]
- [[Network Portion and Host Portion]]

## Mechanism

- First usable address：network address + 1。
- Last usable address：broadcast address - 1。
- Maximum hosts：`2^host_bits - 2`。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Unit05 Additions

- [[Subnet Five Attributes]] reuses usable address range calculation for each generated subnet.
- In VLSM design, usable host capacity is used to choose the smallest prefix length that still satisfies each LAN/WAN requirement.

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
