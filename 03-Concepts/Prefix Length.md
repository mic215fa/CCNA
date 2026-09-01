# Prefix Length

tags: #concept #acting-ccna #ipv4 #subnetting

## Definition

Prefix Length 是 IPv4 address 中 network portion 的 bit 數，格式通常寫成 `/X`。

## Why It Exists

IPv4 address 本身只有 32 bits；prefix length 告訴設備哪幾個 bits 是 network portion，哪幾個 bits 是 host portion。

## Related Concepts

- [[IPv4 Address]]
- [[Netmask]]
- [[Network Portion and Host Portion]]
- [[IPv4 Network Address]]
- [[IPv4 Broadcast Address]]

## Mechanism

若 prefix length 是 `/24`，代表前 24 bits 是 network portion，剩餘 8 bits 是 host portion。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-123_261_1300_1144_318.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.7 — Prefix length indicates the size of the IPv4 network portion.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Unit05 Additions

- In [[Subnetting]], increasing [[Prefix Length]] borrows bits from the host portion to create more subnets.
- Longer prefix length means more subnet bits and fewer host bits per subnet.
- [[FLSM]] uses one prefix length for all subnets; [[VLSM]] uses different prefix lengths for different subnets.

## Additional Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
