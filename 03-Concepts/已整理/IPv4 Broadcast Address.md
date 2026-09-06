# IPv4 Broadcast Address

tags: #concept #acting-ccna #ipv4 #broadcast

> [!info] Concept role
> [[IPv4 Addressing]] 的 address-role 子概念：以 host bits 全 1 表示該 subnet 的 broadcast destination。

## Definition

IPv4 Broadcast Address 是某個 IPv4 network 的最後一個 address，可用來對該 local network 的所有 hosts 發送訊息，不能指派給 host。

## Why It Exists

有些通訊需要送給同一 network 的所有 hosts；broadcast address 提供 Layer 3 的 one-to-all addressing。

## Related Concepts

- [[IPv4 Address]]
- [[Broadcast Frame]]
- [[Broadcast Domain]]
- [[Network Portion and Host Portion]]
- [[Usable IPv4 Address Range]]

## Mechanism

當 IPv4 address 的 host portion 全部為 1，該 address 就是 broadcast address。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
